<template>
  <h1>Bienvenue sur Chuck Ranker</h1>
  <h2>Le classement des phrases de Chuck Norris</h2>
  <img src="../assets/chuckn_approved.png" alt="Chuck Norris image">
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
img {
  margin: 0 auto;
  width: 8vh;
  height: 8vh;
}
h1 {
  font-size:3em;
  margin-top:0;
}
h2 {
  font-size:1.9em;
  margin-top:0;
}
ul {
  list-style-type: none;
  padding: 0;
}
</style>
