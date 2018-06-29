<template>
  <div id="temporizador">
    <div id="relogio">
      <span id="minutos">{{minutos}}</span>
      <span id="separador">:</span>
      <span id="segundos">{{segundos}}</span>
    </div>
    <div id="containerDeBotoesAcaoAtual">
  			<button class="botao redondo" type="button" name="button" id="comecarTimer" v-if="!estadoTimer" @click="rodarTimer">
          Play
        </button>
        <button class="botao redondo" type="button" name="button" id="pararTimer" v-if="estadoTimer" @click="pararTimer">Parar a fera</button>
        <button class="botao redondo" type="button" name="button" id="resetarTimer" v-if="botaoResetar" @click="resetarTimer">Resetar Timer</button>
    </div>
    <div>
      {{quantidadePomodoro}}
      {{quantidadeTotalPomodoros}}
    </div>
  </div>
</template>

<script type="text/javascript">
  export default{
    name: 'Temporizador' ,
    props: ['opcao'],
    watch: {
      opcao: function(newVal){
        this.tempoTotal = newVal*60;
      }
    },
    data(){
      return{
        estadoTimer: null,
  			tempoTotal: 25*60,
  			botaoResetar: false,
        quantidadePomodoro: 0,
        quantidadeTotalPomodoros: 0
      }
    },

    methods:{
      rodarTimer(){
        if(this.quantidadePomodoro == 4){
          alert("Estudos mostram que após 4 seções é benéfico realizar uma pausa longa")
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
        clearInterval(this.timer);
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
        this.quantidadeTotalPomodoros += 1;
        this.quantidadePomodoro +=1;

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
  font-size: 100px;
}

.botao{
  border: none;
  padding: 20px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  margin: 4px 2px;
}

.redondo{
  border-radius: 50%;
}
</style>
