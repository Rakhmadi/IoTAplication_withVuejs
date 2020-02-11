<template>
  <v-app id="inspire">
    <v-navigation-drawer
      v-model="drawer"
      app
    >
      <v-list dense>
        <v-list-item link>
          <v-list-item-action>
            <v-icon>mdi-home</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>Home</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
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
        <v-icon>mdi-water</v-icon>
      </v-list-item-icon>
      <v-list-item-subtitle>Humidity   <h3>{{kelembapan}}</h3> </v-list-item-subtitle>
    </v-list-item>

    <v-list-item>
      <v-list-item-icon>
        <v-icon>mdi-thermometer</v-icon>
      </v-list-item-icon>
      <v-list-item-subtitle>Temprature <h3>{{temperatur}}</h3></v-list-item-subtitle>
    </v-list-item>
     <v-list-item>
      <v-list-item-icon>
        <v-icon>mdi-wifi</v-icon>
      </v-list-item-icon>
      <v-list-item-subtitle>RSSI <h3>{{signalstr}} dbm</h3></v-list-item-subtitle>
    </v-list-item>
      </v-list-item-content>

    
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
         
        <v-list-item-title class="headline mb-1">Relay</v-list-item-title>
        <v-list-item-subtitle>Status Relay Condition <h2 v-if="rely === 1">OFF</h2><h2 v-if="rely === 0">ON</h2> </v-list-item-subtitle>
      </v-list-item-content>


    </v-list-item>

    <v-card-actions>
      <v-btn @click.native='on()' text>On</v-btn>
      <v-btn @click.native='off()' text>Off</v-btn>
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
          <v-btn  text @click="dialog = false">close</v-btn>
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
    
    data(){
      return{
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
          pending:false,
             ip: '', // or null
            port: '8888', // or null
            ips:null,
            dialog: false

      }
    },
    mounted() {
      
      setInterval(() => {
        var url =localStorage.getItem('ip_device');
      const yu =this;
      yu.ips=localStorage.getItem('ip_device');
     
    axios.get(`http://${url}/dht`).then(function(resp){
      yu.dats=resp.data;
      yu.kelembapan=resp.data.kelembapan
      yu.temperatur=resp.data.temperatur
      yu.signalstr=resp.data.signalstr
    }).catch(error => console.log(error)).finally(function () { yu.pending = true })
   }, 1000);
      setInterval(() => {
      const yu =this;
      var url =localStorage.getItem('ip_device');
    axios.get(`http://${url}/`).then(function(resp){
     yu.noe=resp.data
     yu.rely=resp.data.rely
    }).catch(function(err){
      yu.noe=err.response;
      console.log(err.response);
    })
   }, 1000);
    },
    methods: {
        save(){
          var t = this;
           localStorage.setItem('ip_device',t.ip );
           console.log(t.ip)
        },
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
   
    },
  }

</script>