<template>
  <h3>Choissez la phrase que vous préférez</h3>
  <div id="phrases">
    <p class="phrase" ref="phrase1" @click="choisir(1)" :class="{choisi: choix===1, paschoisi: choix===2, jeu: choix===0, degage: degager1}">{{ phrase1.phrase }} <span v-if="choix !== 0">({{ phrase1.vote==null? 0 : phrase1.vote }})</span></p>
    <p class="phrase" ref="phrase2" @click="choisir(2)" :class="{choisi: choix===2, paschoisi: choix===1, jeu: choix===0, degage: degager2}">{{ phrase2.phrase }} <span v-if="choix !== 0">({{ phrase2.vote==null? 0 : phrase2.vote }})</span></p>
  </div>
  <p v-if="choix !== 0"><span class="highlight">{{ stats }}%</span> des gens sont de votre avis !</p>
  <button @click="continuer()" v-if="choix !== 0">Continuer</button>
</template>

<script>
const url = "http://localhost/chuckapi/v2/"
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
      console.log('fetch')
      this.phrases = await (await fetch(url)).json()
    },
    genererPhrase1() {
      do {
        this.phrase1 = this.phrases.data[Math.floor(Math.random() * this.phrases.data.length)];
      } while(this.phrase1 === this.phrase2)
      this.degager1 = false
      this.inserer1 = true

    },
    genererPhrase2() {
      do {
        this.phrase2 = this.phrases.data[Math.floor(Math.random() * this.phrases.data.length)];
      } while(this.phrase1 === this.phrase2)
      this.degager2 = false
      this.inserer2 = true
    },
    async voter(phrase) {
      phrase.vote = parseInt(phrase.vote==null? 0 : phrase.vote)+1;
      const requestOptions = {
        method: "PATCH",
        headers: { 'Content-Type': 'application/json' }, 
        body: JSON.stringify(phrase)
      };
      await fetch(`${url}${phrase.id}`,requestOptions);
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
      console.log(document.getElementsByClassName("paschoisi")[0])
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

<style>
h3 {
  margin-top: 50px;
  font-size:2em;
}
#phrases {
  margin: 20px auto;
  width: 50%;
}
.phrase {
  border: 2px solid #42b983;
  border-radius: 20px;
  padding: 30px;
  font-size: 1.8em;
  transition: all 0.3s;
  cursor: pointer;
  transition-duration: 300ms;
  transition-property: transform;
}
.jeu:hover {
  background-color: #42b983;
  transform: translateY(-5px);
}
.paschoisi {
  border: 2px solid #498067;
}
.choisi {
  background-color: #42b983;
  border: 2px solid #42b983;
}
.highlight {
  color: #42b983;
  font-weight: 700;
}
button {
  font-size: 1.9em;
  padding: 50px;
  background-color:#43da96;
  border:none;
  border-radius: 70px;
  box-shadow: 4px 5px 5px black;
  cursor: pointer;
}
.degage {
  transform: translateX(-150%);
}
</style>  