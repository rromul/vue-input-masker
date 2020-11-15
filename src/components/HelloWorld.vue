<template>
  <div class="hello">
    <h1>
      Live placeholder test
    </h1>
    <label for="sum" class="ctl-label color1">Phone number:</label>
    <live-placeholder pattern="+9 (999) 999-99-99" fix-default show-placeholder>
      <input id="sum" type="text" v-model="phone" default-value="+7 ">
    </live-placeholder>
    <div class="hint-msg color1">от 50 000 до 1 000 000 ₽</div>

    <label for="sum2" class="ctl-label color1">Phone number:</label>
    <live-placeholder pattern="+9 (999) 999-99-99" fix-default show-placeholder mask-digit="_">
      <input id="sum2" type="text" v-model="phone" default-value="+7 ">
    </live-placeholder>
    <div class="hint-msg color1">от 50 000 до 1 000 000 ₽</div>

    <label for="date" class="ctl-label color1">Date:</label>
    <live-placeholder :show-placeholder="true" pattern="99.99.9999" mask-digit="_">
      <input id="date" type="text" value="">
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
  },
  data () {
    return {
      phone: "+7 "
    }
  }
}
</script>

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
