<template>
    <my-component />
</template>

<script>
import MyComponent from "./components/MyComponent";

export default {
  name: "App",
  components: {
    MyComponent,
  },
};
</script>

<style>
  @import url("https://fonts.googleapis.com/css2?family=Roboto+Condensed:wght@400;700&display=swap");
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
    font-family: "Roboto Condensed", sans-serif;
  }

  ul,
  li,
  dl {
    list-style-type: none;
    margin: 0;
    padding: 0;
  }

  #app {
    max-width: 1440px;
    margin: 0 auto;
  }
</style>
