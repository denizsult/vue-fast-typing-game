<template>
  <fast />
</template>

<script>
import fast from "./components/fast.vue";

export default {
  name: "App",
  components: {
    fast,
  },
};
</script>

<style>
@import url("/assets/font.ttf");

html {
  background: gainsboro;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  color: #2c3e50;
}
</style>
