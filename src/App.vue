<template>
  <div id="app">
    <Board></Board>
  </div>
</template>

<script>
import Board from './components/Board.vue'
export default {
  name: 'App',
  components:{Board}
}
</script>

<style>
:root{
    --line_color:#e9ebf0;
    --body_color:#dfe2eb;
    --item_color:rgb(253, 253, 253);
    --text_color:#4A4A4A;
    --hover_color:#dcedc1;
    --button_color:#eeeeee;
}
#app{
  font-family: Helvetica;
  display: flex;
  justify-content: center;
  background-color: var(--body_color);
  height: 860px;
}
</style>
