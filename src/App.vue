<template>
  <div id="app">

    <site_header :start-search="startSearch(param)"/>
    <!--  -->

    <site_main/>
    <!--  -->

    <site_footer/>

  </div>
</template>

<script>
import site_header from "./components/site_header.vue";
import site_main from "./components/site_main.vue";
import site_footer from "./components/site_footer.vue";

export default {
  name: 'App',
    components: {
      site_header,
      site_main,
      site_footer
    },
    data() {
      return {
        userQuery: "",
      }
    },
    methods: {
      startSearch(param){
        console.log(param);
      }
    }
}
</script>

<style lang="scss">

</style>
