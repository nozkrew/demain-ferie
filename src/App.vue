<template>
  <div id="app" >
    <div v-if=loading>
      <p>Chargement ...</p>
    </div>
    <div v-else>
      <h3>Demain c'est férié ?</h3>
      <h1>{{isFerie ? "Oui" :  "Non"}}<small>{{isFerie ? "😁" :  "😕"}}</small></h1>
      <p>{{ demain.format("DD/MM/YYYY") }}</p>
    </div>
    
  </div>
</template>

<script>

import axios from 'axios'
import moment from 'moment'

export default {
  name: 'App',
  components: {
    
  },
  data(){
    return {
      feries: {},
      isFerie: false,
      demain: moment().add('1', 'd'),
      loading: true
    }
  },
  mounted(){
      axios
      .get('https://calendrier.api.gouv.fr/jours-feries/metropole.json')
      .then(response => (this.feries = response.data))
      .then(() => {
        if(this.demain.format('YYYY-MM-DD') in this.feries){
          this.isFerie = true
        }

        if(this.isFerie){
          document.querySelector('body').style.backgroundColor = '#88B04B'
        }
        else{
          document.querySelector('body').style.backgroundColor = '#FF6F61'
        }

        this.loading = false
        
      })
    
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: white;
  margin-top: 60px;
}

h3{
  font-size: 40px;
}

h1{
  font-size: 200px;;
}
</style>
