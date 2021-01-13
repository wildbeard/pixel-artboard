<template>
  <div id="app">
    <h1>Hey, Draw Some Stuff</h1>
    <div>
      <artboard :tool="'draw'"
                :color="'#000'"
                :width="width"
                :height="height" />
      <controls />
    </div>
  </div>
</template>

<script>
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
    }
  },
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
