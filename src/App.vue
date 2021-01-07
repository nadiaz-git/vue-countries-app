<template>
  <div id="app">
    <CountryList></CountryList>
  </div>
</template>

<script>
import CountryList from './components/CountryList.vue';

export default {
  name: 'App',
  components: {
      CountryList
  }
}
</script>

<style>
#app {
  font-family: 'Nunito Sans', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 30px;
}

p, span {
  font-weight: 300;
}

p.light {
  font-weight: 600;
}

p.bold {
  font-weight: 800;
}

  .bg-light {
    background-color: hsl(0, 0%, 98%);
  }

  .bg-light-header {
    background-color: hsl(0, 0%, 100%);
  }
</style>
