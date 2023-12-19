<template>
<div class="main">
  <div class="titulo">
    Monitoreo de Camaras de la Municipalidad de Las Condes /by WIT
  </div>
  <hr>
  <div class="camaras" v-for="(item,index) in urls" :key="index">
   
    <HLSVideoPlayer
    :front=item.front
    :patente="item.patente"
    :inside=item.inside
    />
  
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

  }
  
};
</script>

<style scoped>
.main{
  text-align: center;
}

.camaras {
width: 95%;
text-align: center;
display:inline-block;
    width: 25%;
    padding: 5px;
    margin: 2px;
}
.titulo{
  font-size: xx-large;
  text-align: center;
  margin-top: 20px;
}
</style>



