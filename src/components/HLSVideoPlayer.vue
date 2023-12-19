<template>
    <div class="bus">
        <p>{{ patente }}</p>
        <video ref="videoPlayer" class="video-js vjs-default-skin" controls width="400" ></video>
        <video ref="videoPlayer2" class="video-js vjs-default-skin" controls width="400" ></video>
    </div>
  </template>
  
  <script>
  import videojs from 'video.js';
  import 'video.js/dist/video-js.css';
  import 'videojs-contrib-hls';
  
  export default {

    props:{

      patente:String,
      front:String,
      inside:String
    },


    mounted() {
      // Referencia al elemento de video
      const videoElement = this.$refs.videoPlayer;
  
      // Inicializar el reproductor de video con video.js
      this.player = videojs(videoElement, {
        techOrder: ['html5', 'hlsjs'], // Prioriza HLS.js si está disponible
        autoplay: false, // Autoplay del video (true/false)
        controls: true, // Mostrar controles del video (true/false)
        sources: [{
          src: this.inside,
          type: 'application/x-mpegURL' // Tipo del video
        }]
      }, function onPlayerReady() {
        console.log('Reproductor listo');
      });

      // Referencia al elemento de video
      const videoElement2 = this.$refs.videoPlayer2;
  
      // Inicializar el reproductor de video con video.js
      this.player = videojs(videoElement2, {
        techOrder: ['html5', 'hlsjs'], // Prioriza HLS.js si está disponible
        autoplay: false, // Autoplay del video (true/false)
        controls: true, // Mostrar controles del video (true/false)
        sources: [{
          src: this.front,
          type: 'application/x-mpegURL' // Tipo del video
        }]
      }, function onPlayerReady() {
        console.log('Reproductor listo');
      });
    },
    beforeDestroy() {
      // Destruir el reproductor de video al desmontar el componente
      if (this.player) {
        this.player.dispose();
      }
      if (this.player2) {
        this.player2.dispose();
      }
    }
  };
  </script>
  
  <style>
  /* Estilos opcionales para el componente de video */

  </style>
  