<template>
  <button @click="toggleVisible()">Ajoutez votre phrase</button>
  <div v-if="visible" class="backdrop" @click="toggleVisible()"></div>
  <form v-if="visible" action="" @submit.prevent="handleSubmit()">
    <h3>Ajoutez votre phrase</h3>
    <textarea autofocus v-model="phrase" type="text" name="" id="" rows="5" autocomplete="off" placeholder="Votre phrase de Chuck Norris..."></textarea>
    <input type="submit" value="Ajouter">
  </form>
</template>

<script>
import Constants from '/src/config.js'
export default {
  name: "FormAjout",
  data() {
    return {
      phrase:"",
      visible: false
    }
  },
  methods: {
    toggleVisible() {
      this.visible = !this.visible
    },
    handleSubmit() {
      if (this.phrase == "" || this.phrase.split().length < 2) {
        alert("veuillez écrire une phrase");
      } 
      else {
        const requestOptions = {
          method: "POST",
          headers: { 'Content-Type': 'application/json' }, 
          body: JSON.stringify({"phrase":this.phrase})
        };
        fetch(`${Constants.API_URL}`,requestOptions);
        this.toggleVisible()
        this.phrase="";
        alert("Votre phrase a été ajoutée")
      }
    }
  }
}
</script>

<style scoped>
button {
  font-size: 1.3em;
  border: 2px solid var(--main-green);
  background-color: unset;
  border-radius: 25px;
  padding: 10px;
  box-shadow: none;
  margin: 30px auto 0;
  width: 30vw;
}
button:hover {
  /* box-shadow: inset 0 0 0 2em var(--main-green); */
	transform: scale(1);
	animation: pulse 1s;
}

@keyframes pulse {
  0% {
    box-shadow: 0 0 0 0px var(--main-green);
  }
  100% {
    box-shadow: 0 0 0 20px rgba(0,0,0, 70%);
  }
}
.backdrop {
  position: absolute;
  top:0;
  left:0;
  background-color:rgba(0,0,0,70%);
  width: 100vw;
  height: 100vh;
}
form {
  display:flex;
  flex-direction: column;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 2;
  background-color: var(--main-background-color);
  padding: 25px;
  border: 2px solid var(--main-green);
  border-radius: 20px;
}
h3 {
  margin-top:0;
}
textarea {
  font-size:1.7em;
  border-radius: 20px;
  padding:15px;
  border: 1px solid darkgrey;
  resize: none;
}
input {
  margin-top: 15px;
  font-size:1.7em;
  border-radius: 20px;
  padding:15px;
  border: 1px solid darkgrey;
  transition: all 350ms;
}
input:hover, input:focus {
  border-color: var(--main-green);
  background-color: var(--main-green);
}
</style>