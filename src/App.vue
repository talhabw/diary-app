<template>
  <div id="app">
    <Login />
    <Diary />
  </div>
</template>

<script>
import Login from "./components/Login";
import Diary from "./components/Diary";
export default {
  name: "App",
  data() {
    return {
      login: false
    };
  },
  components: {
    Login,
    Diary
  }
};
</script>

<style>
</style>
