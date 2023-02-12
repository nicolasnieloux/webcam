<template>
  <div class="web-camera-container">
    <div class="camera-button">
      <button type="button" class="button is-rounded"
              :class="{ 'is-primary' : !isCameraOpen, 'is-danger' : isCameraOpen}" @click="toggleCamera"
      >
        <span v-if="!isCameraOpen">open camera</span>
        <span v-else>close camera</span>
      </button>
    </div>

    <div class="camera-loading" v-show="isCameraOpen && isLoading">

      <ul class="loader-circle">
        <li></li>
        <li></li>
        <li></li>
      </ul>
    </div>
    <div v-if="isCameraOpen" v-show="!isLoading" class="camera-box" :class="{'flash' : isShotPhoto}">
      <div class="camera-shutter" :class="{'flash' : isShotPhoto}"></div>
      <video v-show="!isPhotoTaken" ref="camera" :width="450" :height="337.5" autoplay></video>
      <!--      <canvas v-show="isPhotoTaken" id="photoTaken" ref="canvas" :width="450" :height="337.5"></canvas>-->
    </div>
  </div>
</template>


<script>

export default {
  name: 'App',

  data() {

    return {
      isCameraOpen: false,
      isPhotoTaken: false,
      isShotPhoto: false,
      isLoading: false,
      link: '#'

    }
  },

  methods: {
    toggleCamera() {
      if (this.isCameraOpen) {
        this.isCameraOpen = false;
        this.isPhotoTaken = false;
        this.isShotPhoto = false;
        this.stopCameraStream();
      } else {
        this.isCameraOpen = true;
        this.CreateCameraElement();
      }
    },
    CreateCameraElement() {
      this.isLoading = true;
      const constraints = (window.constraints = {
        audio: false,
        video: true
      });
      navigator.mediaDevices.getUserMedia(constraints)
          .then(stream => {
            this.isLoading = false;
            this.$refs.camera.srcObject = stream
          }).catch(error => {
        this.isLoading = false;
        alert("erorr roorr" + error);
      });
    },
    stopCameraStream() {
      let tracks = this.$refs.camera.srcObject.getTracks();

      tracks.forEach(track => {
        track.stop();
      });
    },


  }


}


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
</style>
