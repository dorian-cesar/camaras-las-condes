<template>
<div class="main">
  <div class="itemlogo">
    <div class="logocontainer">
      <a href="https://www.wit.la"><img class="witlogo" src="./assets/logo-wit.png" alt="Logo de WIT.la"></a>
      <img src="./assets/logo.png" alt="Logo de Las Condes">
      <h3>Monitoreo en Línea</h3>
      <button @click="fetch">Refrescar</button>
    </div>
    <div class="tablecont">
    <StatusTable :urldata="urls"/>
    </div>
  </div>
  <div class="container">
    <template v-for="(item,index) in urls" :key="index">
      <template v-if="item.estado === 'active'">
      <HLSVideoPlayer
      :front=item.front
      :patente="item.patente"
      :inside=item.inside
      />
      </template>
    </template>
  </div>
</div>

</template>

<script>
import HLSVideoPlayer from './components/HLSVideoPlayer.vue';
import StatusTable from './components/StatusTable.vue';
import axios from "axios";

export default {
  components: {
    HLSVideoPlayer,
    StatusTable
  },
  data(){
    return {
      urls:[]
      
    }
  },
  created() {
    this.fetch();
    // Revisar cada 10 segundos si es que las camaras han cambiado
    setInterval(this.checkNew, 10000);
 
  },
  methods: {
    async checkNew() {
      try {
        // Si el array de urls existe (validar para evitar errores)
        if(this.urls!=null){
          const resultado = await axios.get("https://masgps-bi.wit.la/buses/camaras/camarasLasCondes.php");
          if(this.urls.length!=resultado.data.list.length){
            // Si el resultado es distinto al anterior, actualizar el
            // array de camaras, lo que provoca que tanto la tabla
            // como los reproductores se regeneren
            this.urls = resultado.data.list;
          }
        }
      } catch (error) {
        console.log(error);
      }
    },
    async fetch() {//busca la lista de buses de las condes***
      try {
        
        const resultado = await axios.get("https://masgps-bi.wit.la/buses/camaras/camarasLasCondes.php");
        this.urls = resultado.data.list;
        console.log(this.urls);
       
      } catch (error) {
        console.log(error);
      }
    },
  }
};
</script>

<style scoped>
.main {
  display: flex;
  height: 100vh;
  justify-content:space-between;
  margin: auto;
  vertical-align: middle;
  align-items: baseline;
}
.container {
  display: flex;
  flex: 1 1;
  justify-content: space-evenly;
  margin: 8px;
  flex-wrap: wrap;
  margin: auto;
  margin-left: 20px;
  vertical-align: top;
  margin-right: 20px;
  align-items: flex-start;
}

.itemlogo {
  box-sizing: border-box;
  text-align: center;
  align-content: center;
  margin-top: 5px;
  margin-bottom: 5px;
  padding: 4px;
  flex: 0 0 330px;
}



.tablecont {
  max-width: 80%;
  max-height: 60vh;
  overflow-y: auto;
  margin: auto;
  margin-top: 20px;
}

.logocontainer {
    position: relative;
    top: 20%;
}

.logocontainer h3 {
    display: block;
    color: white;
    font-family: "Sriracha", cursive;
    font-optical-sizing: auto;
    font-style: normal;
    font-weight: 400;
    font-size: 1.8rem;
    text-transform: capitalize;
    text-shadow: 0px 0px 2px black;
}

.logocontainer img {
    display: block;
    max-width: 100%;
    margin: auto;
}

.logocontainer .witlogo {
    display: block;
    max-width: 30%;
    margin-bottom: 30px;
}

button {
  font-size: 1.6rem;
  font-weight: bold;
  background-color: #240A3B;
  color: white;
  padding: 1rem;
  padding-top: 0.5rem;
  padding-bottom: 0.5rem;
  border: 2px solid black;
  border-radius: 10px;
  cursor: pointer;
}

button:hover {
  background-color: #360461;
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



