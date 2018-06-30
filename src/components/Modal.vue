<template>
  <b-modal ref="modalRef" :title="tituloModal"  :hide-footer="true">
      <div v-if="tipoModal == 0">
        Adicione o tempo customizado para seu pomodoro.
        <br>
        <input v-model="minutos" type="number" name="minutos" value="" placeholder="Minutos">
        <span>:</span>
        <input v-model="segundos" type="number" name="segundos" value="" placeholder="Segundos">
              <b-btn class="mt-3" variant="outline-danger" block @click="setarTempo">Setar tempo</b-btn>
      </div>
      <div v-if="tipoModal == 1">
        Quantidade de sessões: {{quantSessoes}}
        <br>
        Tempo de atividade: {{ parseFloat(quantSessoes*25) }}

      </div>
      <div class="d-block text-center">

      </div>
  </b-modal>
</template>

<script>
export default {
  name: 'Modal' ,
  props: ['titulo'],
  watch:{
    titulo: function(newVal){
      this.tituloModal = newVal;
    }
  },
  data() {
    return {
      showModal: false,
      minutos: 0,
      segundos: 0,
      tituloModal: null,
      tipoModal: null,
      quantSessoes: null,
      quantPausas: null
    }
  },
  methods:{
    mostrarModal(value){
      this.tituloModal = value.nomeHeader;
      this.tipoModal = value.tipoModal;
      this.quantSessoes = value.quantidadeDeSessoes;
      this.quantPausas = value.quantidadeDePausas;
      this.$refs.modalRef.show();
    },
    fecharModal() {
       this.$emit('close');
     },
     setarTempo(event){
       let minutos = this.minutos;
       let segundos = this.segundos;
       this.$emit('setarTempo', {minutos, segundos});
       this.$refs.modalRef.hide()

     }
  }
}
</script>

<style lang="css">
.modal-backdrop {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background-color: rgba(0, 0, 0, 0.3);
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .modal {
    background: #FFFFFF;
    box-shadow: 0.8px 0.8px 8px 0.2px;
    overflow-x: auto;
    display: flex;
    flex-direction: column;
  }

  .modal-header,
  .modal-footer {
    padding: 15px;
    display: flex;
  }

  .modal-header {
    border-bottom: 1px solid #eeeeee;
    color: #4AAE9B;
    justify-content: space-between;
  }

  .modal-footer {
    border-top: 1px solid #eeeeee;
    justify-content: flex-end;
  }

  .modal-body {
    position: relative;
    padding: 10px 10px;
    color: #2c3e50;
  }

  .btn-close {
    border: none;
    font-size: 20px;
    padding: 20px;
    cursor: pointer;
    font-weight: bold;
    color: #4AAE9B;
    background: transparent;
  }

  .btn-green {
    color: white;
    background: #4AAE9B;
    width: auto;
    height: 10vh;
    border: 1px solid #4AAE9B;
    border-radius: 2px;
  }
</style>
