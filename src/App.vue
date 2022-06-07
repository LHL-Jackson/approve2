<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <hr/>
  <label style="font-size:20px"> {{splitData(0)}} - Approval App. </label>
  <hr/>
  <label style="font-size:20px"> Customer Name:  &#160  {{splitData(2)}}  </label>
  <hr/>
  <label  style="font-size:20px"> Order Number:  &#160  </label> 
    <a  style="font-size:25px" href="https://ap.lhlconsulting.com/a.pdf"  target="_blank"> {{splitData(1)}}   </a>
  <hr/>
  <p style="font-size:20px">  Please view from the above link </p> 
  <p style="font-size:20px">  and select yout approval carefully. </p> 
  <hr/>
  <p><label  style="font-size:20px"> Remark   &#160  </label> </p>
  <input style="font-size:20px; width: 500px; "  type="text" v-model="remark"  >
  <hr/>
  <button style="height:50px;width:120px; font-size: 20px; margin-right:20px;" @click.prevent="approved">Approve</button>
  <button style="height:50px;width:120px; font-size: 20px; margin:10px;" @click.prevent="rejected">Reject</button>
   <hr/>
  <input style="font-size:15px; width: 500px; "  type="text" v-model="urlApp"  >
  </template>

<script>
//import HelloWorld from './components/HelloWorld.vue'
//https://localhost:8081/index.html?so=D1001&cmp=ABM P/L
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
      urlApp: ""
    }
  },
  //components: { HelloWorld }
  methods: {
    splitData(index) {
       const encoded64Data = new URL(location.href).searchParams.get('data')
       const plainTextData = window.atob(encoded64Data); // decode a string from base64
       let data1 = plainTextData.split("==");
       return data1[index]
     },
    approved() {
            const plainText = this.splitData(0) + "==" + this.splitData(1)  + "==Approved" + "==" + this.remark
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
      const plainText = this.splitData(0) + "==" + this.splitData(1) + "==Rejected"  + "==" + this.remark
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
</style>
