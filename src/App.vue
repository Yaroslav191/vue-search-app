<template>
   <div id="app">
      <SearchForm />
   </div>
</template>

<script>
import SearchForm from "./components/SearchForm.vue";

export default {
   name: "App",
   components: {
      SearchForm,
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
   margin-top: 60px;
}
</style>
