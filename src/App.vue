<template>
  <div id="app" class="container-fluid d-flex" >
  
    <SentryCard />
    
      
      
    <Footer />  

    
  </div>
</template>

<script>
import SentryCard from "./components/SentryCard.vue";
import Footer from "./components/Footer.vue";
// import 'bootstrap/dist/css/bootstrap.css'

// import ChangeColor from "./components/ChangeColor.vue";

export default {
  name: "App",
  components: {
    SentryCard,
    Footer,
    
    // ChangeColor,
  },
  data() {
    return {
   
      
    }
    
  },
  methods: {
    changeColor: function(){
    
    }
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
  /* margin-top: 25px; */
}
body {
  background: linear-gradient(to bottom right,rgb(219,21,21),rgba(240, 236, 21) );
 
   box-sizing: border-box;
   /* background: linear-gradient(to bottom right,rgb(219,21,21),whitesmoke) */
   /* padding-bottom: 5em; */
}
.container-fluid{
  padding: 0;
  margin: 0; 
   flex-direction: column;
    background-image: url("./assets/Surveillance.svg");
  background-repeat: no-repeat;
 min-height: 93vh;
 
 
}
</style>
