<template>
  <div class="upload-card">
    <div class="upload__photo" ref="photoCard">
      <!-- <img :src="image" /> -->
      <canvas id="imageCanvas" ref="imageCanvas"></canvas>
    </div>

    <div class="upload__action">
      <input type="file" name="up" @change="onFileChange" ref="fileInput" style="display: none" />
      <div class="upload__name-wrapper">
        <div class="upload__label">
          Name
        </div>
        <div class="upload__name">
          {{filename}}
        </div>
      </div>
      <div class="upload__button-wrapper" @click="$refs.fileInput.click()">
        <img src="../assets/Triangle.svg" />
        <div class="upload__button">Upload</div>
      </div>
    </div>

    
  </div>
</template>

<script>
export default {
  name: 'Uploader',
  props: {
    brightness: Number,
    contrast: Number,
  },
  data: function() {
    return {
      filename: 'sljflsdjfkdsjf dksjfldksjf kjskfjksd',
      image: '',
      originalImageData: '',
      context: null,
      canvasWidth: 0,
      canvasHeight: 0,
      containerHeight: 0
    }
  },
  watch: {
    brightness: function () {
      this.applyFilters()
    },
    contrast: function () {
      this.applyFilters()
    }
  },
  mounted() {
    let canvas = this.$refs.imageCanvas;
    this.context = canvas.getContext('2d');

    const container = this.$refs.photoCard;
    this.canvasWidth = canvas.width = container.clientWidth;
    this.containerHeight = this.canvasHeight = canvas.height = container.clientHeight;

    console.log("this.canvasWidth", this.canvasWidth)
    
  },
  methods: {
    onFileChange(e) {
      const files = e.target.files;

      this.createImage(files[0]);
    },
    createImage(file) {
      let reader = new FileReader();
      let vm = this;

      reader.onload = (e) => {
        let img = new Image();
        img.onload = function() {
          vm.drawCanvasImage(img)
        }
        img.src = event.target.result;

        vm.image = e.target.result;
        vm.filename = file.name;
      };
      reader.readAsDataURL(file);
    },
    drawCanvasImage(img) {
      let canvas = this.$refs.imageCanvas;
      var yOffset = 0;

      var newHeight = canvas.width * img.height / img.width;

      if (newHeight > this.containerHeight) {
        this.canvasHeight = canvas.height = this.containerHeight;
        yOffset = ((newHeight - canvas.height) / 2) * -1
      } else {
        this.canvasHeight = canvas.height = newHeight;
      }

      var ctx = canvas.getContext('2d');

      ctx.drawImage(img, 0, yOffset, this.canvasWidth, newHeight);

      let imgData = ctx.getImageData(0, 0, this.canvasWidth, this.canvasHeight);

      ctx.putImageData(imgData, 0, 0);
      
      this.originalImageData = imgData
      this.$emit('updateDisabled', false)
      this.context = ctx
    },

    applyFilters () {
      let imgData = this.context.getImageData(0, 0, this.canvasWidth, this.canvasHeight);

      this.applyBrightness(imgData, this.originalImageData.data, this.brightness);
      this.applyContrast(imgData, this.contrast);

      this.context.putImageData(imgData, 0, 0);
    },

    applyBrightness(data, originalData, brightness) {
      for (var i = 0; i < data.data.length; i+= 4) {
        data.data[i] = originalData[i] + 255 * (brightness / 100);
        data.data[i+1] = originalData[i+1] + 255 * (brightness / 100);
        data.data[i+2] = originalData[i+2] + 255 * (brightness / 100);
      }
    },

    applyContrast(data, contrast) {
      const factor = (259.0 * (contrast + 255.0)) / (255.0 * (259.0 - contrast));

      for (var i = 0; i < data.data.length; i+= 4) {
        data.data[i] = this.truncateColor(factor * (data.data[i] - 128.0) + 128.0);
        data.data[i+1] = this.truncateColor(factor * (data.data[i+1] - 128.0) + 128.0);
        data.data[i+2] = this.truncateColor(factor * (data.data[i+2] - 128.0) + 128.0);
      }
    },

    truncateColor(value) {
      if (value < 0) {
        value = 0;
      } else if (value > 255) {
        value = 255;
      }

      return value;
    },
  }
  
}
</script>

<style lang="scss" scoped>

.upload-card {
  display: flex;
  flex-direction: column;
  box-sizing: border-box;
  height: 268px;
  margin: 29px 20px 29px 20px;
  padding-bottom: 0px;
  border-radius: 5px;
  background-color: #E7E7E7;
  text-transform: uppercase;

  .upload__photo {
    height: 210px;
    display: flex;
    align-items: center;

    canvas {
      width: 100%;
      object-fit: cover;
      border-top-left-radius: 5px;
      border-top-right-radius: 5px;
    }
  }

  .upload__action {
    display: flex;
    flex-direction: row;
    border: 1px solid #DCDEE4;
    border-top: 0px;
    border-bottom-left-radius: 5px;
    border-bottom-right-radius: 5px;
    padding: 7px;
    padding-top: 20px;
    margin-bottom: 0px;
    background-color: #fff;

    .upload__name-wrapper {
      border: 1px solid #DCDEE4;
      border-radius: 5px;
      height: 30px;
      display: flex;
      flex-direction: row;
      width: calc(100% - 124px);
      margin-right: 24px;
      font-size: 11px;
      font-weight: 500;
      line-height: 30px;
      text-transform: uppercase;
      box-sizing: border-box;

      .upload__label {
        letter-spacing: 1.1px;
        width: 60px;
        border-right: 1px solid #DCDEE4;
        height: 100%;
        background-color: #F6F8FA;
        color: #8392A6;
        border-top-left-radius: 3px;
        border-bottom-left-radius: 3px;
        box-sizing: border-box;
        padding-left: 10px;
        padding-right: 10px;
      }

      .upload__name {
        height: 100%;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
        padding-left: 10px;
        padding-right: 10px;
        color: #25A95B;
        box-sizing: border-box;
      }
    }

    .upload__button-wrapper {
      border: 1px solid #DCDEE4;
      border-radius: 5px;
      height: 30px;
      display: flex;
      flex-direction: row;
      align-items: center;
      justify-content: center;
      background-color: #F6F8FA;
      padding-left: 9px;
      width: 100px;
      box-sizing: border-box;
      
      .upload__button {
        font-size: 12px;
        letter-spacing: 0.55px;
        font-weight: 500;
        text-transform: uppercase;
        color: #4A90E2;
        background-color: transparent;
        border: 0px;
        margin-left: 8.7px;
        margin-right: 7.61px;
      }
    }
  }
}
</style>