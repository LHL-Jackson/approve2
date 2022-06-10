<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <div v-cloak>
  
  <hr/>
  <label> {{urlData(0)}} - Approval App. ({{appLevel}}) </label>
  <hr/>
  <label> Customer Name:  &#160  {{urlData(3)}}  </label>
  <hr/>
  <label> Order Number:  &#160  </label> 
    <a  href="https://ap.lhlconsulting.com/a.pdf"  target="_blank"> {{urlData(1)}}   </a>
  <hr/>
  
  <p>  {{appMessage()}} </p> 
  <hr/>
  <p><label> Remark   &#160  </label> </p>
  <input  type="text" v-model="remark"  >
  <hr/>
  <button @click.prevent="approved">Approve</button>
  <button style="margin-left:25px;" @click.prevent="rejected">Reject</button>
   
  </div>
  </template>

<script>
// params: 1=ZP, 2=OrdNo, 3=ApproveLevel, 4=CustName, 
//https:ap.lhlconsulting.com/app/?data=TEhMQ08xPT1PUkQxPT0xPT1hYmMgVHJhZGluZyBQL0w= (level-1)
//https:ap.lhlconsulting.com/app/?data=TEhMQ08xPT1PUkQxPT0yPT1hYmMgVHJhZGluZyBQL0w=   (level-2)
//http://localhost:8080/app/?data=data=TEhMQ08xPT1PUkQxPT0xPT1hYmMgVHJhZGluZyBQL0w=

export default {
  name: 'App',
  props: {
    msg: String
  },
  data() {
    return {
      zp: '',
      soNo:  '',    // new URL(location.href).searchParams.get('so'),
      cmpName: '',  //new URL(location.href).searchParams.get('cmp'),
      remark: '',  //new URL(location.href).searchParams.get('cmp'),
      appLevel: '1',
    }
  },
  //components: { HelloWorld }
  methods: {
    urlData(index) {
       const encoded64Data = new URL(location.href).searchParams.get('data')
       const plainTextData = window.atob(encoded64Data); // decode a string from base64
       let data1 = plainTextData.split("==");
       return data1[index]
     },

    appMessage() {
      var msg ='Please check Draft Invoice carefully.' 
      const encoded64Data = new URL(location.href).searchParams.get('data')
      const plainTextData = window.atob(encoded64Data); // decode a string from base64
      let data1 = plainTextData.split("==");
      if (data1[2]!="1") {
        msg ='Please Approve/Reject Draft Invoice carefully.'
      } 
      this.appLevel=data1[2]
      return msg  
    },
     // params: 1=ZP, 2=OrdNo, 3=Approved/Rejected, 4=ApproveLevel, 5=Remark 
    approved() {
            const plainText = this.urlData(0) + "==" + this.urlData(1) 
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
        .catch(error => console.log('error', error));  

      window.alert("Accpeted")
      //window.close()  
    },

    rejected() {
      const plainText = this.urlData(0) + "==" + this.urlData(1) + "==Rejected"  + "==" + this.appLevel +"==" + this.remark
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
    }
    
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
</style>
