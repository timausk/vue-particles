<template>
  <div id="app">
    <particle-canvas ref="particleCanvas"></particle-canvas>
  </div>

</template>


<script>
import CanvasParticles from './components/CanvasParticles.vue';
export default {
  name: 'app',
  data () {
    return {
      title: 'A Vue.js particles component'
    }
  },
  components: {
    'particle-canvas': CanvasParticles
  }
}
</script>

<style>
@import './assets/main.css';
</style>
