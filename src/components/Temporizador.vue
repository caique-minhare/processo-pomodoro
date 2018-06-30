<template>
  <div id="temporizador">
    <div id="relogio">
      <span id="minutos">{{minutos}}</span>
      <span id="separador">:</span>
      <span id="segundos">{{segundos}}</span>
    </div>
    <div id="containerDeBotoesAcaoAtual">
      <b-container>
        <b-row>
          <b-col>
            <b-button class="botao redondo" v-if="!estadoTimer" @click="rodarTimer">Começar sessão </b-button>
          </b-col>
        </b-row>
        <b-row >
          <b-col>
<b-button class="botao" v-if="estadoTimer" @click="pararTimer">Pausar sessão</b-button>
          </b-col>
          <b-col>
            <b-button class="botao" v-if="botaoResetar" @click="resetarTimer">Resetar sessão</b-button>
          </b-col>
        </b-row>
      </b-container>
    </div>
  </div>
</template>

<script type="text/javascript">
  export default{
    name: 'Temporizador' ,
    props: ['opcao', 'botaoAtivado'],
    watch: {
      opcao: function(newVal){
        this.tempoTotal = newVal*60;
      },
      botaoAtivado: function(newVal){
        this.estadoAtivado = newVal;
      }
    },
    data(){
      return{
        estadoTimer: null,
  			tempoTotal: 25*60,
  			botaoResetar: false,
        quantidadePomodoro: 0,
        quantidadeTotalPomodoros: 0,
        quantidadeDePausasCurtas: 0,
        quantidadeDePausasLongas:0,
        estadoAtivado: null
      }
    },

    methods:{
      rodarTimer(){
        if(this.quantidadePomodoro == 4){
          alert("Estudos mostram que após 4 sessões é benéfico realizar uma pausa longa")
          this.quantidadePomodoro = 0
        }else{
          this.estadoTimer = setInterval(
            () => this.contagemRegressiva(), 1000
  				);
  				this.botaoResetar = true;
        }
			},

      pararTimer(){
        clearInterval(this.estadoTimer);
        this.estadoTimer = null;
        this.botaoResetar = false;
      },

      resetarTimer(){
        this.tempoTotal = (25*60);
        this.pararTimer();
        this.estadoTimer = null;
        this.botaoResetar = false;
      },

      renderizadorTimer(tempo){
        if( tempo < 10 ){
          return '0' + tempo;
        }else{
          return '' + tempo;
        }
		},

    exibirNotificacao(){
      if (!("Notification" in window)) {
        alert("Este navegador não fornece suporte a notificações de desktop");
      }

      // Verificação para ver se as permissões já estão garantidas
      else if (Notification.permission === "granted") {
        // Se sim, criamos uma notificação
        var notification = new Notification("Sua seção de Pomodoro acaba de terminar! Descanse \n uma pausa curta :D");
      }

      // Pedir permissão ao usuário
      else if (Notification.permission !== 'denied') {
        Notification.requestPermission(function (permission) {
          // Aceite de permissão, criar notificação
          if (permission === "granted") {
            var notification = new Notification("Sua seção de Pomodoro acaba de terminar! Descanse uma pausa curta :D");
          }
        });
      }
    },

    contagemRegressiva(){

      if(this.tempoTotal == 0){
        var audio = new Audio('http://soundbible.com/mp3/Elevator Ding-SoundBible.com-685385892.mp3');
        audio.play();
        this.exibirNotificacao();
        this.resetarTimer();
        // Verificação do estado ativado ( Se é um pomodoro ou uma pausa )
        if( this.estadoAtivado == 2){
          this.quantidadeDePausasCurtas +=1;
        }
        else if(this.estadoAtivado == 3){
          this.quantidadeDePausasLongas += 1;
        }
        else{
          this.quantidadeTotalPomodoros += 1;
          this.quantidadePomodoro +=1;
        }

        let qTP = this.quantidadeTotalPomodoros;
        let qPC = this.quantidadeDePausasCurtas;
        let qPL = this.quantidadeDePausasLongas;
        this.$emit('termino', {qTP,qPC,qPL});
      }
      else{
        this.tempoTotal--;
      }
		}
  },
  computed:{
    minutos: function(){
      const minutos = Math.floor(this.tempoTotal/60);
      return this.renderizadorTimer(minutos);
    },

    segundos: function(){
      const segundos = this.tempoTotal - (this.minutos * 60);
      return this.renderizadorTimer(segundos);
    }
  }
}
</script>

<style>
#relogio{
  font-size: 10vh;
}

#containerDeBotoesAcaoAtual{
  padding-bottom: 10px;
}

.botao{
  color: white;
  background: #4AAE9B;
  border: 1px solid #4AAE9B;
  border-radius: 2px;
}

</style>
