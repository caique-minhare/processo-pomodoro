
<template>
  <div id="app">
    <b-container>
      <b-row align-h="center">
        <img src="./assets/logo.png" class="logo">
      </b-row>

      <b-row align-h="center">
          <Temporizador :opcao ="opcao" :estadoAtivado="botaoAtivado" @termino="atualizaDados"></Temporizador>
      </b-row>

      <div id="containerDeBotoesPomodoro">
        <b-container>
          <b-row align-h="center">
            <b-col cols="3">
              <b-button :size="lg" @click="setOption(1)">Pomodoro</b-button>
            </b-col>
            <b-col cols="3">
                <b-button :size="sm" @click="mostrarModal({nomeHeader:'Pomodoro Customizado',tipoModal:0})">Pomodoro Cust</b-button>
            </b-col>
          </b-row>
          <br>
          <b-row align-h="center">
            <b-col cols="3">
              <b-button :size="lg" @click="setOption(2)">Pausa Curta</b-button>
            </b-col>
            <b-col cols="3">
              <b-button :size="lg" @click="setOption(3)">Pausa Longa</b-button>
            </b-col>
          </b-row>
          <br>
          <b-row align-h="center">
            <b-col cols="12">
              <b-button @click="mostrarModal({nomeHeader: 'Dados do usuário', tipoModal: 1})"
                        :titulo="Dados">Ver dados</b-button>
            </b-col>
          </b-row>
        </b-container>
      </div>

      <Modal ref="modalRef" @setarTempo="fecharModal"/>
      </b-container>
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
      modalVisivel: false,
      quantidadeDePausasCurtas: 0,
      quantidadeDePausasLongas: 0,
      quantidadeDePomodoros:0,
      botaoAtivado: null
    }
  },
  methods:{
    setOption(op){
      if(op == 1){
        this.opcao = 25;
        this.mudou = false;
        this.botaoAtivado = 1;
      }
      else if(op == 2) {
        this.opcao = 5;
        this.mudou = false;
        this.botaoAtivado = 2;
      }
      else if(op == 3){
        this.opcao = 10;
        this.mudou = false;
        this.botaoAtivado = 3;
      }
    },

    mostrarModal(value){
      value.quantidadeDeSessoes = this.quantidadeDePomodoros;
      value.quantidadeDePausas = this.quantidadeDePausasCurtas + this.quantidadeDePausasLongas;
      this.$refs.modalRef.mostrarModal(value)
    },
    fecharModal(value){
      this.opcao = parseFloat(value.minutos)+parseFloat(value.segundos)/60;
    },

    atualizaDados(value){
      this.quantidadeDePomodoros = value.qTP;
      this.quantidadeDePausasCurtas = value.qPC;
      this.quantidadeDePausasLongas = value.qPL;
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
  border: 1px solid #4AAE9B;
  border-radius: 2px;
}

#containerTabela{
  padding-top: 50px;
}
</style>
