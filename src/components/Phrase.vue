<template>
  <p :class="{choisi: choisi}"> {{ phrase }}</p>
</template>

<script>
export default {
  name: "CasePhrase",
  props: {
    phrase: String,
    choisi: Boolean
  }
}
</script>

<style scoped>
p {
  border: 2px solid #42b983;
  border-radius: 20px;
  padding: 30px;
  font-size: 1.8em;
  transition: all 0.3s;
}
p:hover {
  background-color: #42b983;
}
.choisi {
  background-color: #42b983;
}
</style>