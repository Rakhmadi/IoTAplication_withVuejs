<template>
  <v-app id="inspire">
    <v-navigation-drawer
      v-model="drawer"
      app
    >
      <v-list dense>
        <v-list-item link>
          <v-list-item-action>
          </v-list-item-action>
          <v-list-item-content  v-on:click="dialog = !dialog">
            <v-list-item-title> Ip Setting</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar
      app
      color="white"
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-toolbar-title>IoT Dinamika</v-toolbar-title>
    </v-app-bar>
    <v-content>
      <v-container
        fluid
      >
       <v-card
    class="mx-auto"
    style="
       margin-bottom:10px;
        background: rgb(58,28,113);
          background: linear-gradient(137deg, rgba(58,28,113,1) 0%, rgba(215,109,119,1) 100%);
    "
     dark
  >
    <v-list-item three-line>
      <v-list-item-content>

      
        <v-list-item>
        <v-list-item-icon>
        <v-icon>mdi-water-percent</v-icon>
      </v-list-item-icon>
      <v-list-item-subtitle>Humidity   <h3>{{kelembapan}}</h3> </v-list-item-subtitle>
    </v-list-item>

    <v-list-item>
      <v-list-item-icon>
        <v-icon>mdi-thermometer</v-icon>
      </v-list-item-icon>
      <v-list-item-subtitle>Temprature <h3>{{temperatur}} &#8451;</h3></v-list-item-subtitle>
    </v-list-item>
     <v-list-item>
      <v-list-item-icon>
        <v-icon>mdi-wifi</v-icon>
      </v-list-item-icon>
      <v-list-item-subtitle>RSSI <h3>{{signalstr}} dBm</h3></v-list-item-subtitle>
    </v-list-item>
      </v-list-item-content>
   <v-btn   @click.native='refresh()' large icon color="white" >  <v-icon> mdi mdi-refresh</v-icon> </v-btn>
    
    </v-list-item>

    <v-card-actions>
    </v-card-actions>
  </v-card>

  <v-card
    class="mx-auto"
    style="
     margin-bottom:10px;
     background: rgb(58,28,113);
background: rgba(58,28,113,1) ;
    "
   dark
  >
    <v-list-item three-line>
      <v-list-item-content>
         
        <v-list-item-title class="headline mb-1">{{nrelay1}}</v-list-item-title>
        <v-list-item-subtitle>Status Relay  <h2 v-if="rely === 1">OFF</h2><h2 v-if="rely === 0">ON</h2> </v-list-item-subtitle>
     <!--- <v-switch
       v-model="modl"
       @change="op()"
    ></v-switch>-->
      </v-list-item-content>

    </v-list-item>

    <v-card-actions>
      <v-btn @click.native='on()' text>On</v-btn>
      <v-btn @click.native='off()' text>Off</v-btn>
    </v-card-actions>
  </v-card>
  

   <v-card
    class="mx-auto"
    style="
     margin-bottom:10px;
     background: rgb(58,28,113);
background: rgba(58,28,113,1) ;
    "
   dark
  >
    <v-list-item three-line>
      <v-list-item-content>
         
        <v-list-item-title class="headline mb-1">{{nrelay2}}</v-list-item-title>
        <v-list-item-subtitle>Status Relay  <h2 v-if="rely1 === 1">OFF</h2><h2 v-if="rely1 === 0">ON</h2> </v-list-item-subtitle>
      </v-list-item-content>

    </v-list-item>

    <v-card-actions>
      <v-btn @click.native='on1()' text>On</v-btn>
      <v-btn @click.native='off1()' text>Off</v-btn>
    </v-card-actions>
  </v-card>


   <v-card
    class="mx-auto"
    style="
     margin-bottom:10px;
     background: rgb(58,28,113);
background: rgba(58,28,113,1) ;
    "
   dark
  >
    <v-list-item three-line>
      <v-list-item-content>
         
        <v-list-item-title class="headline mb-1">{{nrelay3}}</v-list-item-title>
        <v-list-item-subtitle>Status Relay  <h2 v-if="rely2 === 1">OFF</h2><h2 v-if="rely2 === 0">ON</h2> </v-list-item-subtitle>
      </v-list-item-content>

    </v-list-item>

    <v-card-actions>
      <v-btn @click.native='on2()' text>On</v-btn>
      <v-btn @click.native='off2()' text>Off</v-btn>
    </v-card-actions>
  </v-card>

   <v-card
    class="mx-auto"
    style="
     margin-bottom:10px;
     background: rgb(58,28,113);
background: rgba(58,28,113,1) ;
    "
   dark
  >
    <v-list-item three-line>
      <v-list-item-content>
         
        <v-list-item-title class="headline mb-1">{{nrelay4}}</v-list-item-title>
        <v-list-item-subtitle>Status Relay  <h2 v-if="rely3 === 1">OFF</h2><h2 v-if="rely3 === 0">ON</h2> </v-list-item-subtitle>
      </v-list-item-content>

    </v-list-item>

    <v-card-actions>
      <v-btn @click.native='on3()' text>On</v-btn>
      <v-btn @click.native='off3()' text>Off</v-btn>
    </v-card-actions>
  </v-card>



        <v-row
          align="center"
          justify="center"
          style="
          margin-left:1px;
          margin-right:1px;
          "
        >
     
  
        </v-row>
        
      </v-container>
    </v-content>
     <v-dialog v-model="dialog" persistent max-width="360">
     
      <v-card dark
       style="
     margin-bottom:10px;
        background: rgb(58,28,113);
          background: linear-gradient(137deg, rgba(58,28,113,1) 0%, rgba(215,109,119,1) 100%);
     "
      >
        <v-card-title class="headline">Setting Ip</v-card-title>
        <v-card-text>  <v-text-field label="IP" :placeholder="ips" v-model="ip" hide-details="auto"></v-text-field></v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn  text @click="dialog = false , refresh()">close</v-btn>
          <v-btn text @click="save()">Save</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    
   

  </v-app>
  
</template>

<script>
 
import axios from 'axios';
  export default {
    props: {
      source: String,
    },
    data: ()=>({
        dats:{
          },
        drawer: null,
        g:'off',
        kelembapan:'0',
        temperatur:'0',
        signalstr:null,
        condition:'',
        noe:'',
        rely:'',
        rely1:'',
        rely2:'',
        rely3:'',
        pending:false,
        ip: '', // or null
        port: '8888', // or null
        ips:null,
        dialog: false,
        stu:'',
        intervallrsgt:true,
        nrelay1:'Relay 1',
        nrelay2:'Relay 2',
        nrelay3:'Relay 3',
        nrelay4:'Relay 4',
        modl:'',
    }),
    mounted() {
      setInterval(() => {
        var url =localStorage.getItem('ip_device');
      const yu =this;
      yu.ips=localStorage.getItem('ip_device');
      axios.get(`http://${url}/`).then((resp)=>{
        yu.stu=resp.data;
        yu.intervallrsgt=true
      }).catch((err)=>{
         console.log(err.message)
         if (err.message == "Network Error") {
           yu.intervallrsgt=false
         } 
         
      });
      },2000);
      setInterval(() => {
        var url =localStorage.getItem('ip_device');
      const yu =this;
      yu.ips=localStorage.getItem('ip_device');
     axios.get(`http://${url}/dht`).then(function(resp){
      yu.dats=resp.data;
      yu.kelembapan=resp.data.kelembapan
      yu.temperatur=resp.data.temperatur
      yu.signalstr=resp.data.signalstr
      yu.stu=resp.response
    }).catch(error => console.log(error)).finally(function () { yu.pending = true })
   }, 1000);
      setInterval(() => {
      const yu =this;
      var url =localStorage.getItem('ip_device');
    axios.get(`http://${url}/`).then(function(resp){
     yu.noe=resp.data
     yu.rely=resp.data.relay1
     yu.rely1=resp.data.relay2
     yu.rely2=resp.data.relay3
     yu.rely3=resp.data.relay4
    }).catch(function(err){
      yu.noe=err.response;
      console.log(err.response);
    })
   }, 800);
    },
    methods: {

      op(){
        var ep =this;
        if (ep.modl === true) {
          ep.on();
        }else{
          ep.off();
        }
      },

        refresh(){
           window.location.reload(true);
        },
        save(){
          var t = this;
           localStorage.setItem('ip_device',t.ip );
           console.log(t.ip)
        },
        /////////
       on(){
         const yu =this;
         var url =localStorage.getItem('ip_device');
      axios.get(`http://${url}/relay`,{ params: { condition:"on" }}).then(function(resp){
      yu.g=resp.data;
      yu.condition=resp.data.condition
      

    }).catch(function(err){
      yu.g=err.response;
      
    })
    },
    off(){
         const yu =this;
         var url =localStorage.getItem('ip_device');
      axios.get(`http://${url}/relay`,{ params: { condition:"off" }}).then(function(resp){
      yu.g=resp.data;
       yu.condition=resp.data.condition

    }).catch(function(err){
      yu.g=err.response;
    })
    },  
    //////////////////
      on1(){
         const yu =this;
         var url =localStorage.getItem('ip_device');
      axios.get(`http://${url}/relay1`,{ params: { condition:"on" }}).then(function(resp){
      yu.g=resp.data;
      yu.condition=resp.data.condition
      

    }).catch(function(err){
      yu.g=err.response;
      
    })
    },
    off1(){
         const yu =this;
         var url =localStorage.getItem('ip_device');
      axios.get(`http://${url}/relay1`,{ params: { condition:"off" }}).then(function(resp){
      yu.g=resp.data;
       yu.condition=resp.data.condition

    }).catch(function(err){
      yu.g=err.response;
    })
    }, 
    ////////////////////
      on2(){
         const yu =this;
         var url =localStorage.getItem('ip_device');
      axios.get(`http://${url}/relay2`,{ params: { condition:"on" }}).then(function(resp){
      yu.g=resp.data;
      yu.condition=resp.data.condition
      

    }).catch(function(err){
      yu.g=err.response;
      
    })
    },
    off2(){
         const yu =this;
         var url =localStorage.getItem('ip_device');
      axios.get(`http://${url}/relay2`,{ params: { condition:"off" }}).then(function(resp){
      yu.g=resp.data;
       yu.condition=resp.data.condition

    }).catch(function(err){
      yu.g=err.response;
    })
    },  
    //////////////////// 
      on3(){
         const yu =this;
         var url =localStorage.getItem('ip_device');
      axios.get(`http://${url}/relay3`,{ params: { condition:"on" }}).then(function(resp){
      yu.g=resp.data;
      yu.condition=resp.data.condition
      

    }).catch(function(err){
      yu.g=err.response;
      
    })
    },
    off3(){
         const yu =this;
         var url =localStorage.getItem('ip_device');
      axios.get(`http://${url}/relay3`,{ params: { condition:"off" }}).then(function(resp){
      yu.g=resp.data;
       yu.condition=resp.data.condition

    }).catch(function(err){
      yu.g=err.response;
    })
    },  
    },
    watch: {
      
    },
  }
</script>