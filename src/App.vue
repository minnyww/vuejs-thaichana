<template>
   <div id="app">
      <ThaiChana />
   </div>
</template>

<script>
import ThaiChana from './components/ThaiChana.vue';

export default {
   name: 'App',
   components: {
      ThaiChana,
   },
};
</script>

<style>
#app {
   font-family: Avenir, Helvetica, Arial, sans-serif;
   -webkit-font-smoothing: antialiased;
   -moz-osx-font-smoothing: grayscale;
   text-align: center;
   color: #2c3e50;
   margin-top: 20px;
   margin-bottom: 20px;
}
</style>
