<template>
    <div>
    <h1>Sélection de l'exoplanète :</h1>
    <button v-for="planet in exoplanets" :key="planet.id" @click="showPlanet(planet.pl_name)">{{planet.pl_name}}</button>
    <h2>Caractéristiques de l'exoplanète :</h2>
    <p>Nom : {{pl_name}}</p>
    <p>Période de révolution (jours) : {{pl_orbper}} </p>
    <p>Masse (Jupiter Mass) : {{pl_bmassj}} </p>
    <p>Rayon (Jupiter Radius) : {{pl_radj}} </p>
    <p>Température effective (°K): {{st_teff}} </p>
    <dataviz :orbper="pl_orbper"></dataviz>
    </div>
</template>

<script>
import axios from 'axios'
import dataviz from "../components/dataviz"
export default {
  components: {
    dataviz
  },
  methods: {
      showPlanet(name) {
          const index = this.exoplanets.findIndex(planet => planet.pl_name == name)
          this.pl_name= this.exoplanets[index].pl_name
          this.pl_orbper= this.exoplanets[index].pl_orbper
          this.pl_radj= this.exoplanets[index].pl_radj
          this.pl_bmassj= this.exoplanets[index].pl_bmassj
          this.st_teff= this.exoplanets[index].st_teff
      }
  },
  data() {
      return {
          pl_name:undefined,
          pl_orbper: undefined,
          pl_radj: undefined,
          pl_bmassj :undefined,
          st_teff : undefined
      }

  },
    asyncData() {
         return axios.get('https://exoplanetarchive.ipac.caltech.edu/cgi-bin/nstedAPI/nph-nstedAPI?table=exoplanets&select=pl_hostname,pl_name,pl_orbper,pl_bmassj,pl_radj,st_teff&format=json&where=pl_orbper%3E250%20and%20pl_orbper%3C304')
          .then(res => {
              return {
                  exoplanets : res.data
              }    
          })
              
    },
    mounted() {
        this.pl_name= this.exoplanets[0].pl_name
        this.pl_orbper= this.exoplanets[0].pl_orbper
        this.pl_radj= this.exoplanets[0].pl_radj
        this.pl_bmassj= this.exoplanets[0].pl_bmassj
        this.st_teff= this.exoplanets[0].st_teff
    }
}
</script>