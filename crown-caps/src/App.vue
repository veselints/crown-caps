<template>
  <div id="app">
    <router-view ref="view"></router-view>
  </div>
</template>

<script>
    import VueRouter from 'vue-router';

    import AddPlate from './components/AddPlate.vue'
    import Serach from "./components/Search.vue";
    import Match from "./components/Match.vue";


    const routes = [
        { path: '/search', component: Serach },
        { path: '/addplate', component: AddPlate },
        { path: '/match', component: Match },
        { path: '/', component: Serach }
    ];

    const router = new VueRouter({
        routes
    });

  export default {
    name: 'App',
    components: {
    },
    router
  }
</script>

<style>

</style>
