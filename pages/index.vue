<template>
  <div class="container">
    <div>
      <logo />
      <h1 class="title">
        POC_Arcachon
      </h1>

      <div>
        <b-button variant="outline-primary" v-on:click="getCity">Récupérer la position</b-button>
        <p>Latitude : {{lat}} - Longitude : {{lon}}</p>
      </div>

      <hr/>

      <div>
        <p class="errormessage"> {{ errorMessage }}</p>
        <b-form-input v-model="text" placeholder="Enter your name"></b-form-input>
        <div class="mt-2">Météo de : {{ text }}</div>
          <p>{{ weather }} - {{ wdetails }}</p>
          <b-button variant="outline-primary" v-on:click="meteoSearch">Récupérer la météo</b-button>
        </div>

      <div>
        <b-form-input v-model="ip" placeholder="Enter your IP"></b-form-input>
        <div class="mt-2">Votre adresse IP est : {{ ip }}</div>
          <p> Vous êtes situé à : {{ipVille}}</p>
          <b-button variant="outline-primary" v-on:click="geoSearch">Récupérer la ville</b-button>
        </div>
      </div>

  </div>
</template>

<script>
import Logo from '~/components/Logo.vue'
import axios from 'axios'

export default {
  components: {
    Logo
  },
  data: function (){
    return {
      text: "",
      weather: "",
      wdetails: "",
      errorMessage: "",
      ip: "",
      ipVille: "",
      lat: "",
      lon: "",
      position: "",
    }
  },
  mounted() {  
    this.getCity()

},
  methods: {
    meteoSearch: function (event){
      console.log(this.text)
          axios.get("http://localhost:3072/"+this.text)
      .then(response => {
        console.log(response.data)
        this.weather = response.data.main
        this.wdetails = response.data.description
        this.errorMessage = ""
      })
      .catch(function (error) {
        this.errorMessage = error.message
        console.log(error);
        }.bind(this))
    },
    geoSearch: function (event){
      console.log(this.ip)
          axios.get("http://localhost:3030/"+this.ip)
      .then(response => {
        console.log(response.data)
        this.ipVille = response.data;
      })
      .catch(function (error) {
        this.errorMessage = error.message
        console.log(error);
        }.bind(this))
    },
    getCity: function (){
      if (navigator.geolocation) {
        navigator.geolocation.watchPosition(this.tellPosition);
      } else { 
        x.innerHTML = "Geolocation is not supported by this browser.";
      }
    },
    tellPosition: function(position){
      this.lat = position.coords.latitude;
      this.lon = position.coords.longitude;
    }
  }
}
</script>

<style>
.errormessage{
  color: red;
}
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 75px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
