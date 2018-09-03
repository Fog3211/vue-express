<template>
  <div id="app">

    <app-header :poiInfo="poiInfo"></app-header>

    <app-nav></app-nav>

    <router-view></router-view>

  </div>
</template>

<script>
import Header from "./components/header/header";
import nav from "./components/nav/nav";
export default {
    name: "App",
    components: {
        "app-header": Header,
        "app-nav": nav
    },
    data() {
      return {
        poiInfo:{}
      }
    },
     created(){
    // axios

    // fetch
    fetch("/api/goods")
      .then(res => {
        return res.json()
      })
      .then(response =>{
        // console.log(response)
        if(response.code == 0){
          // console.log(response.data)
          this.poiInfo = response.data.poi_info
          // console.log(this.poiInfo)
        }
      })
  }
};
</script>

<style>
</style>
