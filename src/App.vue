<template>
  <HelloWorld />
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
export default {
  components: { HelloWorld },
};
</script>

<style>
#app {
  height: 100%;
}

html,
body {
  margin: 0;
  padding: 0;
}

/* use viewport-relative units to cover page fully */
body {
  height: 100vh;
  width: 100vw;
  background-color: dimgray;
}

/* include border and padding in element width and height */
* {
  box-sizing: border-box;
}
</style>
