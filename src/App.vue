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
    --body_color:#f3f6fd;
    --item_color:rgb(253, 253, 253);
    --text_color:#4A4A4A;
    --hover_color:rgb(227, 236, 250);
}
#app{
  display: flex;
  justify-content: center;
  background-color: var(--body_color);
  height: 1000px;
}
</style>
