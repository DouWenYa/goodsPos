<template>
  <div id="app">
    <!--左侧导航-->
   <leftNav></leftNav>
    <!--main-->
  <div class="main">
    <router-view></router-view>
  </div>

  </div>
</template>

<script>
import leftNav from "@/components/common/leftNav";
//import leftNav from "./leftNav";
export default {
  name: "App",
  components: {
    leftNav
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.main{
  height: 100%;
}
</style>
