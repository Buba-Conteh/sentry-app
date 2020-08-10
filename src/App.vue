<template>
  <div id="app" class="container-fluid d-flex ">
    <SentryCard />

    <Footer />  
  </div>
</template>

<script>
import SentryCard from "./components/SentryCard.vue";
import Footer from "./components/Footer.vue";

export default {
  name: "App",
  components: {
    SentryCard,
    Footer,
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
body {
  background-color: rgb(219, 21, 21);
  background-image: url("./assets/Surveillance.svg");
  background-repeat: no-repeat;
   box-sizing: border-box;
   /* padding-bottom: 5em; */
}
.container-fluid{
  padding: 0;
  margin: 0; 
   flex-direction: column;
 min-height: 90.8vh;
 
}
</style>
