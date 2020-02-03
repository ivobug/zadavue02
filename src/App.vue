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

  </div>
</template>

<script>
import axios from 'axios';
import axiosTiming from 'axios-timing'


axiosTiming(axios, window.console.log)

export default {
  name: 'app',
  data(){
    return{
      joke:null,
      image:null,
      errors:[],
      start:null,
      diff:null
    }
  },
  methods:{
    axiosFunk:function(){
      // uzimamo trenutno vrijeme na pocetku axios funkcije te njime oduzimamo vrijeme
      // zavrsetka axios funkcije.. milisekunde ispisujemo u consolu
      this.start=new Date().getTime()
      axios.get(`https://api.chucknorris.io/jokes/random`)
      .then(response => {
        this.diff=new Date().getTime()-this.start
        this.joke=response.data.value
        this.image= response.data.icon_url
        window.console.log(this.diff)
      })
      .catch(e => {
        this.errors.push(e)
      })
    },


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
