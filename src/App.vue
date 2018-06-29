
<template>
  <div id="app">
    <img src="./assets/logo.png">
    <Temporizador :opcao ="opcao"></Temporizador>
    <div id="containerDeBotoesPomodoro">
      <button type="button" name="button" @click="setOption(1)">Pomodoro</button>
      <button type="button" name="button" @click="setOption(2)">Pausa Curta</button>
      <button type="button" name="button" @click="setOption(3)">Pausa Longa</button>
  </div>
  </div>
</template>

<script>
import Temporizador from './components/Temporizador'

export default {
  name: 'App',
  components:{ Temporizador },
  data(){
    return{
      mudou: true,
      opcao: 25
    }
  },
  methods:{
    setOption(op){
      if(op == 1){
        this.opcao = 25;
        this.mudou = false;
      }
      else if(op == 2) {
        this.opcao = 5;
        this.mudou = false;
      }
      else if(op == 3){
        this.opcao = 10;
        this.mudou = false;
      }
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
