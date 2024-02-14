<template>
<div class="main">
  <div class="container">
  <div class="itemlogo">
      <div class="logocontainer">
          <img src="./assets/logo.png" alt="Logo de Las Condes">
          <h3>MONITOREO DE CÁMARAS</h3>
          <button @click="reloadPage">Refrescar</button>
      </div>
  </div>
  <div class="item" v-for="(item,index) in urls" :key="index">
   
    <HLSVideoPlayer
    :front=item.front
    :patente="item.patente"
    :inside=item.inside
    />
  
    </div>
  </div>
</div>

</template>

<script>
import HLSVideoPlayer from './components/HLSVideoPlayer.vue';
import axios from "axios";

export default {
  components: {
    HLSVideoPlayer
  },
  data(){
    return {
      urls:[]
      
    }
  },
  created() {
    this.fetch();
 
  },
  methods: {
    async fetch() {//busca la lista de buses de las condes***
      try {
        
        const resultado = await axios.get("https://masgps-bi.wit.la/buses/camaras/camarasLasCondes.php");
        this.urls = resultado.data.list;
        console.log(this.urls);
       
      } catch (error) {
        console.log(error);
      }
    },

  reloadPage() {
    location.reload();
  }

  }
  
};
</script>

<style scoped>
.container {
  display: flex;
  justify-content:space-evenly;
  margin: 8px;
  flex-wrap: wrap;
  margin: auto;
}

.item {
    box-sizing: border-box;
    text-align: center;
    align-content: center;
    margin-top: 30px;
    margin-bottom: 5px;
    padding: 4px;
    flex-basis: 18%;
    background-color: rgb(33, 74, 113);
    box-sizing: border-box;
    border-top-left-radius: 10px;
    border-top-right-radius: 10px;
}

.itemlogo {
  box-sizing: border-box;
  text-align: center;
  align-content: center;
  margin-top: 5px;
  margin-bottom: 5px;
  padding: 4px;
  flex-basis: 18%;
}

.logocontainer {
    position: relative;
    top: 20%;
}

.logocontainer h3 {
    display: block;
    color: white;
    font-family: "Orbitron", sans-serif;
    font-optical-sizing: auto;
    font-style: normal;
    font-weight: 600;
    font-size: 1.8rem;
    text-transform: capitalize;
}

.logocontainer img {
    display: block;
    max-width: 100%;
    margin: auto;
}

button {
  font-size: 1.6rem;
  font-weight: bold;
  background-color: rgb(72, 140, 204);
  color: white;
  padding: 1rem;
  padding-top: 0.5rem;
  padding-bottom: 0.5rem;
  border: 2px solid black;
  border-radius: 10px;
}

button:hover {
  background-color: rgb(37, 90, 139);
}

.camaras {
width: 95%;
text-align: center;
display:inline-block;
    width: 25%;
    padding: 5px;
    margin: 2px;
}
</style>



