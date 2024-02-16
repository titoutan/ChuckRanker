<template>
  <h1>Bienvenue sur Chuck Ranker</h1>
  <img src="../assets/ChuckNorris.jpg" alt="Chuck Norris image">
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
  height: 12vh;
}
h1 {
  font-size:3em;
  margin-top:0;
  margin-bottom:0.7em;
  text-align: center;
}
ul {
  list-style-type: none;
  padding: 0;
}
</style>
