<template>
  <HelloWorld/>
  <FormAjout id="FormAjout"/>
  <div class="buttons">
    <button @click="this.state='liste'" :class="{'selected':this.state==='liste'}">
      Classement
    </button>
    <button @click="this.state='jeu'" :class="{'selected':this.state==='jeu'}">
      Jeu
    </button>
  </div>
  <ListePhrase v-if="state === 'liste'"/>
  <JeuChoix v-if="state === 'jeu'"/>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import JeuChoix from './components/JeuChoix.vue'
import FormAjout from './components/FormAjout.vue'
import ListePhrase from './components/ListePhrase.vue'

export default {
  name: 'App',
  components: {
    HelloWorld,
    JeuChoix,
    FormAjout,
    ListePhrase
  },
  data() {
    return{
      state:"liste"
    }
  } 
}
</script>

<style>
:root {
  --main-green:#2e8b57;
  --main-background-color: #060906;
}
* {
  background-color: var(--main-background-color);
  color: #e4e6eb;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display:flex;
  flex-direction: column;
  width:60%;
  min-width: 800px;
  margin-left: auto;
  margin-right:auto;
}
h3 {
  margin-top: 50px;
  font-size:2em;
}
</style>
<style scoped>
  .buttons {
    margin-top: 2em;
    font-size: 1.3em;
  }
  .buttons button {
    color: var(--main-green);
    border:unset;
    padding:0;
    margin: 0 15px;
    font-size: 1.5em;
  }
  .buttons .selected {
    border-bottom:1px solid var(--main-green);
  }
</style>
