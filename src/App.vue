<template>
  <div :class="$style.test">
    <video width="100px" height="100px" ref="video" autoplay></video>
    <p>{{text}}</p>
  </div>
</template>

<script>
import QrCode from 'qrcode-reader';

export default {
  data() {
    return {
      text: ''
    }
  },
  mounted() {
    const qrCode = new QrCode();
    qrCode.callback = function(error, result) {
      if (error) {
        this.text = error;
      } else {
        console.log(result);
        this.text = error;
      }
    }

    const video = this.$refs.video;
    navigator.mediaDevices.getUserMedia({ video: true })
        .then(function(stream) {
          video.srcObject = stream;
          video.play();
        });

    video.addEventListener('play', function() {
      const canvas = document.createElement('canvas');
      canvas.width = 1000;
      canvas.height = 1000;
      const ctx = canvas.getContext('2d');
      setInterval(function() {
        ctx.drawImage(video, 0, 0, canvas.width, canvas.height);
        const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
        qrCode.decode(imageData);
      }, 500);
    });
  }
}
</script>

<style lang="css" module>
  .test {
    width: 1000px;
    height: 1000px;
  }

  .test video {
    width: 1000px;
    height: 1000px;
  }
</style>
