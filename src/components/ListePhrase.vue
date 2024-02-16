<template>
  <h2>Le classement des phrases de Chuck Norris</h2>
  <ul v-if="this.phrases.length > 0"  class="slide-fade">
    <li v-for="i of this.phrases" :key="i.id" :class="{show: this.show[this.phrases.findIndex((elt) => elt.id === i.id)]}">
      <div class="classement">
        <span class="rank">
          {{ this.phrases.findIndex((elt) => elt.id === i.id)+1 }}
        </span>
        <div class="like">
          <i class="fa-regular fa-thumbs-up"></i>
          {{ i.vote == null ? 0 : i.vote }}
        </div>
      </div>
      <div class="corps">
        <p class="phrase">
          {{ i.phrase }}
        </p>
        <i @click="signaler(i.id)" class="signalement fa-solid fa-flag fa-lg"></i>  
      </div>
    </li>
  </ul>
</template>

<script>
import Constants from '/src/config.js'
export default {
  name: "ListePhrase",
  data() {
    return {
      phrases: Array,
      show: Array
    }
  },
  methods: {
    async fetchData() {
      console.log(Constants.API_URL)
      this.phrases = (await (await fetch(Constants.API_URL)).json()).data.sort((a,b) => b.vote - a.vote)
      this.show = Array(this.phrases.length)
      console.log(this.show)
      for (let i = 0; i<this.phrases.length; i++) {
        setTimeout(
          () => {
            this.show.unshift(true)
          },
          50*i
        )
      }
    },
  signaler(id) {
    const requestOptions = {
      method: "PATCH",
      headers: { 'Content-Type': 'application/json' }
    };
    fetch(`${Constants.API_URL}/report/${id}`,requestOptions);
    alert("Votre phrase a été signalée")
  }
  },
  async created() {
    this.fetchData()
  }
}
</script>

<style scoped>
h2 {
  font-size:1.9em;
  margin-top:1em;
}
ul {
  list-style-type: none;
  padding:0;
}
li {
  border: 1px solid var(--main-green);
  border-radius: 5px;
  display: flex;
  align-items: center;
  padding: 10px;
  margin: 1.5em 0;
}
.slide-fade li {
  transition: all 0.4s ease-out;
  opacity: 0;
}
.slide-fade li.show {
  opacity: 1;
}
.classement {
  flex: 1;
}
.corps {
  flex: 9;
  display:flex;
}
.rank {
  font-size:1.9em
}
.like {
  font-size:1.1em
}
.phrase {
  text-align: left;
  font-size:1.3em;
  margin-right:5px;
}
.signalement {
  color: #dd4444;
  align-self: flex-end;
  margin-left: auto;
  cursor: pointer;
  margin-right:5px;
  margin-bottom: 15px;
}
</style>