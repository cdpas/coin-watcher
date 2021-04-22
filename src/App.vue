<template>
  <v-app>
     <v-app-bar 
     app 
     style="max-width: 100vw; text-align:center; background-color:#006d77"
     >
       <div style="width: 10vw"></div>
       <div style="width: 80vw">
        <h1 style="color:white">Coin Watcher</h1> 
       </div>
      <div  style="width: 10vw">
      </div>
  </v-app-bar>
    <v-main>
      <Home/>
    </v-main>
    <p 
    class="footer"
    >
    by
    <a href="https://cdpas.net">cdpas</a>
    </p>
  </v-app>
</template>
<script>
import Home from './components/home';
export default {
  name: 'App',
  components: {
    Home
  },
  data: () => ({
    //
  }),
};
</script>
<style lang="scss">
@import './assets/css/style.css';
</style>