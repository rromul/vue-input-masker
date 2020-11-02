<template>
  <div class="hello">
    <h1>
      Live placeholder test
    </h1>
    <label for="sum" class="ctl-label color1">Some text:</label>
    <live-placeholder pattern="+9 (999)-999-99-99">
      <input id="sum" type="text" value="+7 " placeholder="+7 (###)-###-##-##">
    </live-placeholder>
    <div class="hint-msg color1">от 50 000 до 1 000 000 ₽</div>

    <label for="date" class="ctl-label color1">Date:</label>
    <live-placeholder pattern="99.99.9999">
      <input id="date" type="text" value="" placeholder="##.##.####">
    </live-placeholder>
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
.hello {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
