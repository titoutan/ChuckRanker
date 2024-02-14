<template>
  <HelloWorld msg="Welcome to Your Vue.js App"/>
  <FormAjout id="FormAjout"/>
  <JeuChoix/>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import JeuChoix from './components/JeuChoix.vue'
import FormAjout from './components/FormAjout.vue'

export default {
  name: 'App',
  components: {
    HelloWorld,
    JeuChoix,
    FormAjout
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display:flex;
  flex-direction: column;
}
h3 {
  margin-top: 50px;
  font-size:2em;
}
</style>
