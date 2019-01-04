<template>
  <div id="app">
<Header />
<GithubExplorer />
  </div>
</template>

<script>
import GithubExplorer from "./components/GithubExplorer.vue";
import Header from "./components/Header.vue";
export default {
  name: "app",
  components: {
    GithubExplorer,
    Header
  }
};
</script>

<style lang="less">
@import '../node_modules/bootstrap/dist/css/bootstrap.min.css';
@import '../node_modules/@fortawesome/fontawesome-free/css/all.min.css';
#app {
  background: #eeeeee;
  min-height: 100vh;
}
</style>
