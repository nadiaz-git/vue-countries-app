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
}

p, span {
  font-weight: 300;
}

p.light {
  font-weight: 600;
}

p.bold, h5.bold, h4.bold, h3.bold {
  font-weight: 800;
}

.bg-light {
  background-color: hsl(0, 0%, 98%);
}

.bg-light-header {
  background-color: hsl(0, 0%, 100%);
}

.row-margin {
  margin: 50px;
}

.darktheme {
  background-color: hsl(207, 26%, 17%);
  color: hsl(0, 0%, 100%);
}

.lighttheme {
  background-color: hsl(0, 0%, 98%);
  color: hsl(200, 15%, 8%);
}

.darktheme-h {
  background-color: hsl(209, 23%, 22%);
  color: hsl(0, 0%, 100%);
}

.lighttheme-h {
  background-color: hsl(0, 0%, 100%);
  color: hsl(200, 15%, 8%);
}

.cardstyle {
  margin: auto;
  width: 100%;
  max-width: 20rem;
  cursor: pointer;
}

.darkcard {
  background-color: hsl(209, 23%, 22%) !important;
}

.lightcard {
  background-color: hsl(0, 0%, 100%) !important;
}
</style>
