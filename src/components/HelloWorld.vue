<template>
  <div class="hello">
    <h1>
      Live placeholder test
    </h1>
    <label for="sum" class="ctl-label color1">Some text:</label>
    <live-placeholder>
      <input id="sum" type="text" value="" placeholder="###-###-###">
    </live-placeholder>
    <div class="hint-msg color1">от 50 000 до 1 000 000 ₽</div>
  </div>
</template>

<script>
import LivePlaceholder from './Live-placeholder'
export default {
  name: 'HelloWorld',
  components: { LivePlaceholder },
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
