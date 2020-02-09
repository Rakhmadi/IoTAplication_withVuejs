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
            <v-icon>mdi-contact-mail</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>Contact</v-list-item-title>
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

        <v-list-item-title class="headline mb-1"></v-list-item-title>
        <v-list-item>
        <v-list-item-icon>
        <v-icon>mdi-water</v-icon>
      </v-list-item-icon>
      <v-list-item-subtitle>Humidity   <h3>{{dats.kelembapan}}</h3> </v-list-item-subtitle>
    </v-list-item>

    <v-list-item>
      <v-list-item-icon>
        <v-icon>mdi-thermometer</v-icon>
      </v-list-item-icon>
      <v-list-item-subtitle>Temprature <h3>{{dats.temperatur}}</h3></v-list-item-subtitle>
    </v-list-item>
     <v-list-item>
      <v-list-item-icon>
        <v-icon>mdi-wifi</v-icon>
      </v-list-item-icon>
      <v-list-item-subtitle>RSSI <h3>{{dats.signalstr}} dbm</h3></v-list-item-subtitle>
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
        <v-list-item-subtitle>Status Relay Condition <h2>{{g.condition}}</h2> </v-list-item-subtitle>
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
        dats:null,
         drawer: null,
         g:'off',

      }
    },
    mounted() {
      setInterval(() => {
      const yu =this;
    axios.get("http://192.168.43.100/dht").then(function(resp){
      yu.dats=resp.data;

    }).catch(function(err){
      yu.dats=err.response;
    })
   }, 500);
    },
    methods: {
       on(){
         const yu =this;
      axios.get("http://192.168.43.100/relay",{ params: { condition:"on" }}).then(function(resp){
      yu.g=resp.data;

    }).catch(function(err){
      yu.g=err.response;
    })
    },
    off(){
         const yu =this;
      axios.get("http://192.168.43.100/relay",{ params: { condition:"off" }}).then(function(resp){
      yu.g=resp.data;

    }).catch(function(err){
      yu.g=err.response;
    })
    }
    },
  }

</script>