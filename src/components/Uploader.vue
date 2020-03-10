<template>
  <div class="upload-card">
    <div class="upload__photo" ref="photoCard">
      <img :src="image" />
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
      <div class="upload__button">
        <img src="../assets/Triangle.svg" />
        <button type="button" @click="$refs.fileInput.click()" class="trigger">Upload</button>
      </div>
    </div>

    <canvas id="imageCanvas" ref="imageCanvas" style="display: none"></canvas>
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
      filename: '',
      image: '',
      originalImageData: '',
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
    this.image = "data:image/gif;base64,R0lGODlhAQABAAAAACwAAAAAAQABAAA=";
  },
  methods: {
    onFileChange(e) {
      var self = this;

      var  files = e.target.files;

      var reader = new FileReader();
      this.createImage(files[0]);

      reader.onload = () => {
        var img = new Image();
        img.onload = function() {
          self.drawCanvasImage(img)
        }
        img.src = event.target.result;
      };

      reader.readAsDataURL(files[0]);
    },
    createImage(file) {
      var reader = new FileReader();
      var vm = this;

      reader.onload = (e) => {
        vm.image = e.target.result;
        vm.filename = file.name;
      };
      reader.readAsDataURL(file);
    },
    drawCanvasImage(img) {
      var canvas = this.$refs.imageCanvas;
      var container = this.$refs.photoCard;
      var yOffset = 0

      canvas.width = container.clientWidth;


      canvas.height = canvas.width * img.height / img.width;

      if (canvas.height > container.clientHeight) {
        var clientHeightMidpoint = container.clientHeight / 2
        var imageHeightMidpoint = canvas.height / 2

        yOffset = (imageHeightMidpoint - clientHeightMidpoint) / 2
      }

      var ctx = canvas.getContext('2d');
      ctx.drawImage(img, 0, -yOffset, canvas.width, canvas.height);

      var imgData = ctx.getImageData(0, 0, canvas.width, canvas.height);

      ctx.putImageData(imgData, 0, 0);
      
      this.originalImageData = imgData
      this.image = canvas.toDataURL("image/png");

      // console.log("i'm updating disabled", this.updateDisabled)

      // this.updateDisabled(false)
      this.$emit('updateDisabled', false)

    },

    applyFilters () {
      var canvas = this.$refs.imageCanvas;
      var ctx = canvas.getContext('2d');
      
      var imgData = ctx.getImageData(0, 0, canvas.width, canvas.height);

      this.applyBrightness(imgData, this.originalImageData.data, this.brightness);
      this.applyContrast(imgData, this.contrast);

      ctx.putImageData(imgData, 0, 0);
      this.image = canvas.toDataURL("image/png");
    },

    applyBrightness(data, originalData, brightness) {
      for (var i = 0; i < data.data.length; i+= 4) {
        data.data[i] = originalData[i] + 255 * (brightness / 100);
        data.data[i+1] = originalData[i+1] + 255 * (brightness / 100);
        data.data[i+2] = originalData[i+2] + 255 * (brightness / 100);
      }
    },

    applyContrast(data, contrast) {
      var factor = (259.0 * (contrast + 255.0)) / (255.0 * (259.0 - contrast));

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

<style scoped>

.upload-card {
  display: flex;
  flex-direction: column;
  height: 268px;
  margin-left: 20px;
  margin-right: 20px;
  margin-top: 29px;
  margin-bottom: 29px;
  padding-bottom: 0px;
  box-sizing: border-box;
  text-transform: uppercase;
  background-color: #E7E7E7;
  border-radius: 5px;
}

.upload__photo {
  height: 221px;

}

.upload__photo img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  width: calc(100vw - 40px);
  border-top-left-radius: 5px;
  border-top-right-radius: 5px;
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
  justify-content: space-between;
  margin-bottom: 0px;
  background-color: #fff;
}

.upload__name-wrapper {
  border: 1px solid #DCDEE4;
  border-radius: 5px;
  height: 30px;
  display: flex;
  flex-direction: row;
  width: 61.79%;
  align-items: center;
  justify-content: center;
  font-size: 11px;
  font-weight: 500;
  line-height: 30px;
  text-transform: uppercase;
}

.upload__label {
  letter-spacing: 1.1px;
  width: 60px;
  border-right: 1px solid #DCDEE4;
  height: 100%;
  background-color: #F6F8FA;
  color: #8392A6;
  border-top-left-radius: 3px;
  border-bottom-left-radius: 3px;
}

.upload__name {
  width: 71.01%;
  height: 100%;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  padding-left: 10px;
  padding-right: 10px;
  color: #25A95B;
}

.upload__button {
  border: 1px solid #DCDEE4;
  border-radius: 5px;
  height: 30px;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  background-color: #F6F8FA;
  padding-left: 9px;
}

.upload__button button {
   -webkit-appearance: none;
  font-size: 12px;
  letter-spacing: 0.55px;
  font-weight: 500;
  text-transform: uppercase;
  color: #4A90E2;
  background-color: transparent;
  border: 0px;
}
</style>