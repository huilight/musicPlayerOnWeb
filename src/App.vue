<template>
  <div id="app">
    <MusicControler></MusicControler>
	<!-- <music></music> -->
	<!-- <ProgressBar></ProgressBar> -->
	<!-- <IconTest></IconTest> -->
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
// import music from './components/music.vue'
import MusicControler from './components/MusicControler.vue'
import ProgressBar from './components/ProgressBar.vue'
import IconTest from './components/IconTest.vue'

export default {
  name: 'app',
  components: {
    HelloWorld,
	// music,
	MusicControler,
	ProgressBar,
	IconTest,
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
