<template>
  <b-table striped hover :items="items"></b-table>
</template>

<script>
var data = new Date()
var diaAtual = data.getDate() + '/' + (data.getMonth() < 10 ? ('0'+data.getMonth()) : data.getMonth()) + '/' + data.getFullYear() ;
const items = [
  { dia: diaAtual, quantidade_de_sessoes: 40, pausas_curtas: 1, pausas_longas: 1 },
  { dia: diaAtual, quantidade_de_sessoes: 21, pausas_curtas: 5, pausas_longas: 0 },
  { dia: diaAtual, quantidade_de_sessoes: 89, pausas_curtas: 4, pausas_longas: 0 },
  { dia: diaAtual, quantidade_de_sessoes: 38, pausas_curtas: 8, pausas_longas: 5 }
]

export default {
  data () {
    return {
      items: items
    }
  }
}
</script>
