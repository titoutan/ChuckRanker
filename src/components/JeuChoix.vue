<template>
  <p class="phrase" ref="phrase1" @click="choisir(1)" :class="{choisi: choix===1, paschoisi: choix===2, jeu: choix===0}">{{ phrase1.phrase }} <span v-if="choix !== 0">({{ phrase1.vote }})</span></p>
  <p class="phrase" ref="phrase2" @click="choisir(2)" :class="{choisi: choix===2, paschoisi: choix===1, jeu: choix===0}">{{ phrase2.phrase }} <span v-if="choix !== 0">({{ phrase2.vote }})</span></p>
  <p v-if="choix !== 0"><span class="highlight">{{ stats }}%</span> des gens sont d'accord avec toi</p>
  <button @click="continuer()" v-if="choix !== 0">Continuer</button>
</template>

<script>
const url = "https://chuckapi.alwaysdata.net/chuckapi/v2/"
export default {
  name: 'JeuChoix',
  data() {
    return {
      phrases: null,
      phrase1: "",
      phrase2: "",
      choix: 0
    }
  },
  computed: {
    stats() {
      switch (this.choix) {
        case 1:
          return Math.round(this.phrase1.vote/(this.phrase1.vote+this.phrase2.vote)*100)
        case 2:
          return Math.round(this.phrase2.vote/(this.phrase1.vote+this.phrase2.vote)*100)
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
      } while(this.phrase1 === this.phrase2)
    },
    genererPhrase2() {
      do {
        this.phrase2 = this.phrases.data[Math.floor(Math.random() * this.phrases.data.length)];
      } while(this.phrase1 === this.phrase2)
    },
    choisir(n) {
      if (this.choix === 0) {
        this.choix = n
      }
      let requestOptions;
      switch (n) {
        case 1:
          this.phrase1.vote+=1;
          requestOptions = {
            method: "PATCH", // Méthode HTTP
            headers: { 'Content-Type': 'application/json' }, // Type de contenu
            body: JSON.stringify(this.phrase1) // Corps de la requête
          };
          fetch(`${url}${this.phrase1.id}`,requestOptions);
          break;
        case 2:
          this.phrase2.vote+=1;
          requestOptions = {
            method: "PATCH", // Méthode HTTP
            headers: { 'Content-Type': 'application/json' }, // Type de contenu
            body: JSON.stringify(this.phrase2) // Corps de la requête
          };
          fetch(`${url}${this.phrase2.id}`,requestOptions);
          break;
      }
    },
    continuer() {
      switch (this.choix) {
        case 1:
          this.genererPhrase2()
          break;
        case 2:
          this.genererPhrase1()
          break;
      }
      this.choix = 0;
    }
  }
}
</script>

<style>
.phrase {
  border: 2px solid #42b983;
  border-radius: 20px;
  padding: 30px;
  font-size: 1.8em;
  transition: all 0.3s;
}
.jeu:hover {
  background-color: #42b983;
}
.paschoisi {
  border: 2px solid #498067;
}
.choisi {
  background-color: #42b983;
  border: 2px solid #42b983;
}
button {
  font-size: 1.9em;
  padding: 50px;
  background-color:#43da96;
  border:none;
  border-radius: 70px;
  box-shadow: 4px 5px 5px black;
}
</style>