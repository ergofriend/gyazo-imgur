<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>
      View source code on
      <a href="https://github.com/ErgoFriend/gyazo-imgur" target="_blank" rel="noopener">GitHub</a>.
    </p>
    <p> Qiita article
      <a href="https://qiita.com/ErgoFriend/items/acb61dda0121acbed531" target="_blank" rel="noopener">https://qiita.com/ErgoFriend/items/acb61dda0121acbed531</a>
     </p>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  }
};
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
