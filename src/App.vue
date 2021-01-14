<template>
  <div id="app">
    <h1>Hey, Draw Some Stuff</h1>
    <div>
      <artboard :tool="tool"
                :color="color"
                :width="width"
                :height="height"
                :data="from_data"
                @updated-board="updateBoardData" />
      <controls @change-tool="(payload) => tool = payload"
                @change-color="(payload) => color = payload" />
    </div>
  </div>
</template>

<script>
// @todo: better event management!
import Artboard from './components/Artboard';
import Controls from './components/ArtboardControls';
export default {
  name: 'App',
  components: {
    Artboard,
    Controls,
  },
  data() {
    return {
      width: document.body.offsetWidth,
      height: document.documentElement.scrollHeight,
      tool: 'draw',
      color: '#000',
      board_data: {
        drawn: [],
        on_board: [],
      },
      from_data: [],
    }
  },
  methods: {
    updateBoardData(payload) {
      this.board_data.drawn = payload.drawn;
      this.board_data.on_board = payload.on_board;
    },
  }
}
</script>

<style>
* {
  box-sizing: border-box;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
