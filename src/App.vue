
<template>
  <div id="app">
    <img src="./assets/logo.png" class="logo">
    <Temporizador :opcao ="opcao"></Temporizador>
    <div id="containerDeBotoesPomodoro">
      <button type="button" name="button" @click="setOption(1)">Pomodoro</button>
      <button type="button" name="button" @click="mostrarModal">Pomodoro Customizado</button>
      <button type="button" name="button" @click="setOption(2)">Pausa Curta</button>
      <button type="button" name="button" @click="setOption(3)">Pausa Longa</button>
  </div>

  <div id="containerTabela">
    <Tabela/>
  </div>

    <Modal v-show="modalVisivel" @close="closeModal" />
  </div>
</template>

<script>
import Temporizador from './components/Temporizador'
import Modal from './components/Modal'
import Tabela from './components/Tabela'

export default {
  name: 'App',
  components:{ Temporizador, Modal, Tabela },
  data(){
    return{
      mudou: true,
      opcao: 25,
      modalVisivel: false
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
    },

    mostrarModal(){
      this.modalVisivel = true;
    },
    closeModal(value){
      this.modalVisivel = false;
      this.opcao = parseFloat(value.minutos)+parseFloat(value.segundos)/60;
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
  color: #fff;
  margin-top: 60px;
}

body{
  background-color: #2c3e50;
}

.logo{
  height: 7vh;
  width: auto;
}

#containerDeBotoesPomodoro button {
  color: white;
  background: #4AAE9B;
  width: auto;
  height: 8vh;
  border: 1px solid #4AAE9B;
  border-radius: 2px;
}

#containerTabela{
  padding-top: 50px;
}
</style>
