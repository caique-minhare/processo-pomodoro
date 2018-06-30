<template>
  <div class="">
    <b-table striped hover :items="items"></b-table>
  </div>

</template>

<script>
var data = new Date()
var diaAtual = data.getDate() + '/' + (data.getMonth() < 10 ? ('0'+data.getMonth()) : data.getMonth()) + '/' + data.getFullYear() ;
const items = [
  { dia: diaAtual, quantidade_de_sessoes: 40, pausas_curtas: 1, pausas_longas: 1 }
]

export default {
  props:['qpc', 'qpl', 'pomodoros'],
  watch:{
    qpc: function( newVal ){
      this.quantidadeDePausasCurtas = newVal;
      alert("QPC A")
    },
    qpl: function( newVal ){
      this.quantidadeDePausasLongas = newVal;
    },
    pomodoros: function( newVal ){
      this.pomodoro = newVal
      alert("QPT")
    }
  },

  data () {
    return {
      items: items,
      quantidadeDePausasCurtas: 0,
      quantidadeDePausasLongas: 0,
      pomodoro: 0
    }
  },
}
</script>
