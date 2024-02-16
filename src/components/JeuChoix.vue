<template>
  <h2>Choissez la phrase que vous préférez</h2>
  <div id="phrases">
    <p class="phrase" ref="phrase1" @click="choisir(1)" :class="{choisi: choix===1, paschoisi: choix===2, jeu: choix===0, degage: degager1}">{{ phrase1.phrase }} <span v-if="choix !== 0">({{ phrase1.vote==null? 0 : phrase1.vote }})</span></p>
    <p class="phrase" ref="phrase2" @click="choisir(2)" :class="{choisi: choix===2, paschoisi: choix===1, jeu: choix===0, degage: degager2}">{{ phrase2.phrase }} <span v-if="choix !== 0">({{ phrase2.vote==null? 0 : phrase2.vote }})</span></p>
  </div>
  <p v-if="choix !== 0"><span class="highlight">{{ stats }}%</span> des gens sont de votre avis !</p>
  <button @click="continuer()" v-if="choix !== 0">Continuer</button>
</template>

<script>
import Constants from '/src/config.js'
const url = Constants.API_URL
export default {
  name: 'JeuChoix',
  data() {
    return {
      phrases: null,
      phrase1: "",
      phrase2: "",
      degager1: true,
      degager2: true,
      choix: 0
    }
  },
  computed: {
    stats() {
      switch (this.choix) {
        case 1:
          return Math.round(parseInt(this.phrase1.vote)/(parseInt(this.phrase1.vote)+parseInt(this.phrase2.vote))*100)
        case 2:
          return Math.round(parseInt(this.phrase2.vote)/(parseInt(this.phrase1.vote)+parseInt(this.phrase2.vote))*100)
      }
      return 0;
    }
  },
  async created() {
    await this.fetch();
    this.genererPhrase1();
    this.genererPhrase2();
  },
  methods: {
    async fetch() {
      this.phrases = await (await fetch(url)).json()
    },
    genererPhrase1() {
      do {
        this.phrase1 = this.phrases.data[Math.floor(Math.random() * this.phrases.data.length)];
      } while(this.phrase1.id === this.phrase2.id)
      if (this.phrase1.vote == null) {
        this.phrase1.vote = 0;
      }
      this.degager1 = false
      this.inserer1 = true

    },
    genererPhrase2() {
      do {
        this.phrase2 = this.phrases.data[Math.floor(Math.random() * this.phrases.data.length)];
      } while(this.phrase1.id === this.phrase2.id)
      if (this.phrase2.vote == null) {
        this.phrase2.vote = 0;
      }
      this.degager2 = false
      this.inserer2 = true
    },
    async voter(phrase) {
      const requestOptions = {
        method: "PATCH",
        headers: { 'Content-Type': 'application/json' }
      };
      await fetch(`${url}upvote/${phrase.id}`,requestOptions);
      await this.fetch();
    },
    choisir(n) {
      if (this.choix === 0) {
        this.choix = n
        switch (n) {
          case 1:
            this.voter(this.phrase1)
            break;
          case 2:
            this.voter(this.phrase2)
            break;
        }
      }
    },
    continuer() {
      document.getElementsByClassName("paschoisi")[0].classList.add("degage")
      switch (this.choix) {
        case 1:
          setTimeout(this.genererPhrase2,
          500);
          this.inserer2 = false;
          this.degager2 = true;
          break;
        case 2:
          setTimeout(this.genererPhrase1,
          500);
          this.inserer1 = false;
          this.degager1 = true;
          break;
      }
      this.choix = 0;
    }
  }
}
</script>

<style scoped>
h2 {
  font-size:2em;
  margin-top:1em; 
}
#phrases {
  margin: 0 auto;
}
.phrase {
  border: 2px solid var(--main-green);
  border-radius: 20px;
  padding: 30px;
  font-size: 1.2em;
  transition: all 0.3s;
  cursor: pointer;
  transition-duration: 300ms;
  transition-property: transform;
}
.jeu:hover {
  background-color: var(--main-green);
  transform: translateY(-5px);
}
.paschoisi {
  border: 2px solid #498067;
}
.choisi {
  background-color: var(--main-green);
  border: 2px solid var(--main-green);
}
.choisi span {
  background-color: var(--main-green);
}
.highlight {
  color: var(--main-green);
  font-weight: 700;
}
button {
  transition: 0.35s;
  font-size: 1.9em;
  padding: 50px;
  border:2px solid var(--main-green);
  background-color: unset;
  border-radius: 70px;
  cursor: pointer;
  width:fit-content;
  margin: 0 auto
}
button:hover {
  box-shadow: inset 0 0 0 3em var(--main-green);
}
.degage {
  transform: translateX(-100vw);
}
</style>  