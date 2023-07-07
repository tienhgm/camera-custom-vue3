<template>
  <div class="parent">
    <video id="video" autoplay muted playsinline></video>
    <canvas id="canvas"></canvas>
    <div class="bottom-camera"></div>
    <img src="~/assets/imgs/temp.png" class="img-temp" />
    <div class="capture" id="snap" @click="onCapture()"></div>
  </div>
</template>

<script>
export default {
  name: "Test",
  data() {
    return {
      video: "",
    };
  },
  async mounted() {
    try {
      const video = document.querySelector("#video");
      await this.setupCamera();
      video.play();
    } catch (error) {
      alert(error);
    }
  },
  methods: {
    async setupCamera() {
      const video = document.querySelector("#video");
      const stream = await navigator.mediaDevices.getUserMedia({
        video: { facingMode: "environment" },
      });
      if (stream) {
        video.srcObject = stream;
        return new Promise((resolve) => {
          video.onloadedmetadata = () => {
            resolve(video);
          };
        });
      }
    },
    handleSuccess(stream) {},
    onCapture() {
      const canvas = document.getElementById("canvas");
      var ctx = canvas.getContext("2d");
      const video = document.querySelector("#video");
      ctx.drawImage(video, 0, 0, canvas.width, canvas.height);
      const dataURL = canvas.toDataURL();
      console.log(dataURL);
    },
  },
};
</script>

<style>
* {
  box-sizing: border-box;
  padding: 0;
  margin: 0;
}
.parent {
  position: relative;
  overflow: hidden;
  width: 100%;
  height: 100vh;
}
#video {
  width: 100%;
  /* height: inherit; */
  /* transform: rotate(90deg); */
  height: calc(100%- 150px);
  transform-origin: bottom left;
  /* width: 100vh;
  height: 100vw; */
  /* margin-top: -100vw; */
  object-fit: cover;
}
.img-temp {
  position: absolute;
  /* width: 50%; */
  height: 30%;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%) rotate(90deg);
}
img {
  display: block;
  width: 100%;
  height: 100%;
}
#canvas {
  position: absolute;
  z-index: 9;
  width: 60px;
  height: 60px;
  object-fit: cover;
  bottom: 2rem;
  left: 5%;
}
.bottom-camera {
  height: 150px;
  position: absolute;
  width: inherit;
  bottom: 0;
  background-color: rgba(145, 24, 24, 0.769);
}
.capture {
  cursor: pointer;
  bottom: 2rem;
  left: 50%;
  transform: translateX(-50%);
  position: absolute;
  background: #fff;
  border-radius: 50%;
  width: 60px;
  height: 60px;
}
</style>
