<template>
  <div id="app">
    <div class="imagePlace">
      <img v-bind:src="image">
    </div>
    <div class="textPlace">

      <span>{{joke}}</span>
    </div>
    <br>
    <button @click="axiosFunk()">Get joke</button>
    {{category}}


  </div>
</template>

<script>
import axios from 'axios';
import axiosTiming from 'axios-timing'
// Koristio sam dva nacina za mjernje vremena requesta (oba su u konzoli)
// 1. Putem axios Timinga (koji je precizniji)
axiosTiming(axios, window.console.log)

export default {
  name: 'app',
  data(){
    return{
      joke:null,
      image:null,
      category:null,
      errors:[],
      start:null,
      diff:null,
      categories:[],
    }
  },
  methods:{
    axiosFunk:function(){
      // 2. uzimamo trenutno vrijeme na pocetku axios funkcije te njime oduzimamo vrijeme
      // zavrsetka axios funkcije.. milisekunde ispisujemo u consolu
      this.start=new Date().getTime()
      // uzimamo link s random kategorijom
      axios.get(`https://api.chucknorris.io/jokes/random?category=${this.getRandomCategory()}`)
      .then(response => {
        this.diff=new Date().getTime()-this.start
        this.joke=response.data.value
        this.category=response.data.categories[0]
        this.image= response.data.icon_url
        window.console.log(`drugi rezultat ${this.diff} ms `)

      })
      .catch(e => {
        this.errors.push(e)
      })
    },
    // Biramo random broj te returnamo kategoriju koja je pod tim rednim brojem 
    getRandomCategory:function(){
      let randomNum = Math.floor(Math.random() * this.categories.length)
      return this.categories[randomNum]

    }
    },
    // Pri pokretanju stranice dohvaćamo kategorije te ih spremamo u categories array
    // i ispisujemo ih u konzoli
  created(){
    axios.get(`https://api.chucknorris.io/jokes/categories`)
    .then(response => {
    this.categories=response.data
    window.console.log(this.categories)
      })
    }    
  }

</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  margin: 10px;
}
div{
  display: inline-block;
  margin: 5px;
}
.imagePlace{
  background-color: rgb(235, 172, 101);
  border-radius: 10px;
  width: 70px;
  height: 70px;

}
.textPlace{
    width: 700px;
    height: 70px;
    border-radius: 10px;
    background-color: rgb(235, 235, 235);
    position: absolute;
    padding: 5px;

}
button{
  margin: 10px;
}

img{
    margin: 5px;

}
</style>
