<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <div v-cloak>
  
  <hr/>
  <label> {{zp}} - Approval App. ({{appLevel}}) </label>
  <hr/>
  <label> Customer Name:  &#160  {{cmpName}}  </label>
  <hr/>
  <label> Order Number:  &#160  {{soNum}} </label> 
    <!-- <a  href="https://ap.lhlconsulting.com/a.pdf"  target="_blank"> {{urlData(1)}}   </a> -->
  <hr/>
  
  <p>  {{appMessage()}}  &#160  {{getLastInv()}} </p> 
  <hr/>

  <p v-if = "showButton">

    <p><label> Remark   &#160  </label> </p>
  <input  type="text" v-model="remark"  >
  <hr/>

  <button  @click.prevent="approved">Approve</button>
  <button  style="margin-left:25px;" @click.prevent="rejected">Reject</button> 
  
  <!-- <button @click.prevent="getLastInv">Check</button>
  <p><label > Last inv1:  &#160  {{LastInvNum}} </label> </p>
  <p><label > Last inv2:  &#160  {{getLastInv()}} </label> </p> -->

</p>

  </div>
  </template>

<script>
// params: 1=ZP, 2=OrdNo, 3=ApproveLevel, 4=CustName, 
//https:ap.lhlconsulting.com/app/?data=TEhMQ08xPT1PUkQxPT0xPT1hYmMgVHJhZGluZyBQL0w= (level-1)
//https:ap.lhlconsulting.com/app/?data=TEhMQ08xPT1PUkQxPT0yPT1hYmMgVHJhZGluZyBQL0w=   (level-2)
//http://localhost:8080/app/?data=data=TEhMQ08xPT1PUkQxPT0xPT1hYmMgVHJhZGluZyBQL0w=
// posted https://ap.lhlconsulting.com/app/?data=TEhMQ08xPT1aMDE9PTE9PWFiYyBUcmFkaW5nIFAvTA==
export default {
  name: 'App',
  props: {
    msg: String,
    soNo:  '',    
  },
  data() {
    return {
      encodedData:  new URL(location.href).searchParams.get('data'),
      remark: '',  
      LastInvNum: '',
      showButton: true,
    }
  },
  //components: { HelloWorld }
  computed: {
       //approveLevel: () => urlData(2),
    zp () {
       const plainTextData = window.atob(this.encodedData); // decode a string from base64
       let data1 = plainTextData.split("==");
       return data1[0]
      },
    soNum () {
       const plainTextData = window.atob(this.encodedData); // decode a string from base64
       let data1 = plainTextData.split("==");
       return data1[1]
      },
    appLevel () {
       const plainTextData = window.atob(this.encodedData); // decode a string from base64
       let data1 = plainTextData.split("==");
       return data1[2]
      },
    cmpName () {
       const plainTextData = window.atob(this.encodedData); // decode a string from base64
       let data1 = plainTextData.split("==");
       return data1[3]
      },
    },

  methods: {
    appMessage() {
      var msg ='Please check Draft Invoice carefully. (L1)' 
      if (this.appLevel!="1")
        msg ='Please Approve/Reject Draft Invoice carefully. (L2)';
      if (this.LastInvNum!="")
        {
          msg ='Invoice already Approved. ';        
        }
        
      return msg  
    },

// params: 1=ZP, 2=OrdNo, 3=Approved/Rejected, 4=ApproveLevel, 5=Remark 
    approved() {
            // const plainText = this.urlData(0) + "==" + this.urlData(1) 
            //                  + "==Approved" + "==" + this.appLevel +"==" + this.remark
      const plainText = this.zp + "==" + this.soNum
                             + "==Approved" + "==" + this.appLevel +"==" + this.remark            
      const encoded64 = window.btoa(plainText); // encode a string
      const url = 'https://ap.lhlconsulting.com/approval/api/approve?data=' + encoded64
      this.urlApp =url
      
      const requestOptions = {
        method: "GET",
        mode: 'no-cors',
      };
      
      fetch(url, requestOptions)
        .then(response =>  response.text())        //response.json()
        .then(data => this.result1 = data)
        .catch(error => console.log('error****************', error));  

      window.alert("Accpeted")
      window.close()  
    },

    rejected() {
      const plainText = this.urlData(0) + "==" + this.urlData(1) 
                        + "==Rejected"  + "==" + this.appLevel +"==" + this.remark
      const encoded64 = window.btoa(plainText); // encode a string
      const url = 'https://ap.lhlconsulting.com/approval/api/approve?data=' + encoded64
      //https://ap.lhlconsulting.com/approval/api/approve?data=UzAyMDAxPT1BQkMgUC9M
      //https://localhost:44301/api/approve?data=UzAyMDAxPT1BQkMgUC9M
      const requestOptions = {
        method: "GET",
        mode: 'no-cors',
      };
      
      fetch(url, requestOptions)
        .then(response =>  response.text())        //response.json()
        .then(data => this.result1 = data)
        .catch(error => console.log('error', error));  

      window.alert("Rejected")
      window.close()  
    },


   getLastInv() {
      const encoded64Data = new URL(location.href).searchParams.get('data')
      //const plainTextData = window.atob(encoded64Data); // decode a string from base64
      //const url = 'https://ap.lhlconsulting.com/approval/api/approve/lastInvNum?data=' + encoded64Data
      //const url ='https://localhost:44301/api/approve/LastInvNum?data=TEhMQ08xPT1BQTIyMDIwMT09MT09YWJjIFRyYWRpbmcgUC9M'
      //const url ='https://ap.lhlconsulting.com/approval/api/approve/LastInvNum?data=TEhMQ08xPT1BQTIyMDIwMT09MT09YWJjIFRyYWRpbmcgUC9M'
      const url = 'https://ap.lhlconsulting.com/approval/api/approve/lastInvNum?data=' + encoded64Data;
      this.urlApp =url;
      const requestOptions = {
        method: "GET",
        mode: 'no-cors',
      };
      
      fetch(url, requestOptions)
         .then(response =>  response.json())
         .then(data => (this.LastInvNum = data.LastInvNum))
         .catch(error => console.log('error-1111-2222', error));
  
       if (this.LastInvNum!='')
           this.showButton=false;

      // if (this.LastInvNum!='')
      //   this.msg = "Order already posted.";

      return this.LastInvNum
      
    }
  
  },
  mounted() {
     this.getLastInv(); 
  }

}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
  margin-top: 20px;
  margin-left: 20px;
}
body {
  font-size: 20px;
}

input[type="text"] {
  height: 30px;
  width: 400px;
  margin-top: 0px 0;
  display: block; 
}

button {
  font-size: 20px;
  height: 50px;
  width: 110px;
  /* padding: 10px; */
}


[v-cloak] {
  display: none;
}

[x-hidden] {
  display: none;
}


</style>
