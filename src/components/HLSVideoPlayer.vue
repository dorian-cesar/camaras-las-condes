<template>
  <div class="item">
    <div class="bus">
      <h4>{{ patente }}</h4>
      <video ref="videoPlayer" class="video-js vjs-default-skin" controls width="345"></video>
      <video ref="videoPlayer2" class="video-js vjs-default-skin" controls width="345"></video>
    </div>
  </div>
</template>

<script>
import videojs from 'video.js';
import 'video.js/dist/video-js.css';
import 'videojs-contrib-hls';

export default {
  props: {
    patente: String,
    front: String,
    inside: String
  },

  mounted() {
    // Inicializamos ambos reproductores
    this.initializePlayer(this.$refs.videoPlayer, this.inside);
    this.initializePlayer(this.$refs.videoPlayer2, this.front);
  },

  beforeDestroy() {
    // Dispose para liberar recursos cuando se destruya el complemento
    if (this.player) {
      this.player.dispose();
    }
    if (this.player2) {
      this.player2.dispose();
    }
  },

  methods: {
    initializePlayer(videoElement, source) {
      const player = videojs(videoElement, {
        techOrder: ['html5'],
        autoplay: false,
        controls: true,
        sources: [{
          src: source,
          type: 'application/x-mpegURL'
        }]
      }, () => {
        console.log('Reproductor listo');
      });

      // Si hay errores, reintentar
      player.on('error', () => {
        console.log('Ocurrio un error, reintentando...');
        this.retryLoad(player, source);
      });
    },

    retryLoad(player, source) {
      const maxRetryAttempts = 3;
      let retryCount = 0;
      const retryDelay = 3000; // 3 seconds

      function loadVideo() {
        player.src([source, 'application/x-mpegURL']);
        player.load();
      }

      function retry() {
        if (retryCount < maxRetryAttempts) {
          retryCount++;
          setTimeout(loadVideo, retryDelay);
        } else {
          console.error('Fallo al recargar, reintentando...');
        }
      }

      retry();
    }
  }
};
</script>

<style scoped>
/* Estilos opcionales para el componente de video */
.bus h4 {
  display: inline;
  margin: 0;
  color: white;
}
.bus video {
  display: block;
  box-sizing: border-box;
}

.item {
    box-sizing: border-box;
    text-align: center;
    align-content: center;
    margin-top: 30px;
    margin-bottom: 5px;
    padding: 4px;
    flex-basis: 18%;
    background-color: #240A3B;
    box-sizing: border-box;
    border-top-left-radius: 10px;
    border-top-right-radius: 10px;
}
</style>
