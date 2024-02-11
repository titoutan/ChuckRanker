<template>
  <CasePhrase ref="phrase1" @click="choisir(1)" phrase="Ma phrase de chuck" :choisi="choix===1 "/>
  <CasePhrase ref="phrase2" @click="choisir(2)" phrase="Une autre phrase de chuck" :choisi="choix===2"/>
</template>


<script>
import CasePhrase from './Phrase.vue'

export default {
  name: 'JeuChoix',
  components: {
    CasePhrase
  },
  data() {return {
    choix: 0
  }},
  methods: {
    choisir(n) {
      this.choix = n
    }
  }
}
</script>

<style>

</style>