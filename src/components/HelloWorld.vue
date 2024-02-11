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
  width: 10%;
}
h1 {
  font-size:3em
}
h2 {
  font-size:1.9em;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
