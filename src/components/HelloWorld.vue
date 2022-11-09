<template>
  <div class="hello">
    <h1>{{ msg }}</h1>


    <b>{{ firstNumber - 2 }}</b>
    <br>
    <b>{{ firstNumber * 2 }}</b>
    <br>
    <b>{{ firstNumber - 111 }}</b>
    <b>ID: {{ _uid }}</b>

    <button v-on:click="firstNumber = firstNumber + 1">Dodaj</button>
  </div>
</template>

<script>
export default {
  data: function () {
    return {
      firstNumber: 12,
    }
  },
  name: 'HelloWorld',
  props: {
    msg: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
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
