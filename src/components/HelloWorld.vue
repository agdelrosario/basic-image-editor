<template>
  <div class="hello">

    <div class="header-bar">
      <div class="header-bar__title">Brightness & Contrast Developer Test</div>
      <div class="header-bar__subtitle">01 Jun, 2018 – 31 Dec, 2019</div>
    </div>

    <div class="page-content">
      <div class="header">
        <div class="header__img"></div>
        <div class="header__avatar">
          <img src="../assets/IMG_0105.jpeg" />
        </div>
      </div>

      <div class="controls">
        <div class="controls__card" id="brightness">
          <div class="controls__header" :style="{color: '#25A95B'}">Brightness</div>
          <div class="controls__slider">
            <input type="range" min="-100" max="100" value="0" class="slider" id="brightnessRange" @input="updateBrightness" />
          </div>
          <div class="controls__instruction">Slide to adjust image brightness! ☀️</div>
        </div>

        <div class="controls__card" id="contrast">
          <div class="controls__header" :style="{color: '#4A90E2'}">Contrast</div>
          <div class="controls__slider">
            <input type="range" min="-100" max="100" value="0" class="slider" id="contrastRange" @input="updateContrast" />
          </div>
          <div class="controls__instruction">Slide to adjust image contrast! 🌓</div>
        </div>

      </div>

      <div class="upload-card">
        <div class="upload__photo" ref="photoCard">
          <!-- <img ref="img"  /> -->
          <img :src="image" />

          <!-- <img src="../assets/pleasure-garden-1200-by-768.jpg" /> -->
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
      </div>
          
    </div>
    <canvas id="imageCanvas" ref="imageCanvas" style="display: none"></canvas>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data: 
    function() {
      return {
        originalImageData: '',
        image: '',
        filename: '',
        brightness: 0,
        contrast: 0,
      }
    },
  methods: {
    onFileChange(e) {
      // var files = e.target.files || e.dataTransfer.files;
      // if (!files.length)
      //   return;
      // this.createImage(files[0]);

      var self = this;

      var  files = e.target.files; //reader,

      var reader = new FileReader();
      this.createImage(files[0]);

      reader.onload = () => {
        var img = new Image();
        img.onload = function() {
          self.drawCanvasImage(img)
          // self.applyFilters(img)
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
    },

    updateBrightness (e) {
      var val = (e.target.valueAsNumber + 100) / 200
      document.getElementById('brightnessRange').style.backgroundImage = '-webkit-gradient(linear, left top, right top, ' +
        'color-stop(' + val + ', #25A95B), ' + 'color-stop(' + val + ', #c8e9d6)' + ')'

      this.brightness = e.target.valueAsNumber
      this.applyFilters()
    },

    updateContrast (e) {
      var val = (e.target.valueAsNumber + 100) / 200
      document.getElementById('contrastRange').style.backgroundImage = '-webkit-gradient(linear, left top, right top, ' +
        'color-stop(' + val + ', #4A90E2), ' + 'color-stop(' + val + ', #c8dbe9)' + ')'

      this.contrast = e.target.valueAsNumber
      this.applyFilters()
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
    }
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.header-bar {
  height: 70px;
  background-color: #7344C0;
  color: #FFFFFF;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.header-bar__title {
  font-weight: 500;
  font-size: 15px;
  line-height: 25px;
}

.header-bar__subtitle {
  font-size: 13px;
  line-height: 20px;
}

.header {
  height: 195px;
  margin-bottom: 20px;
}

.header__img {
  height: 100%;
  width: auto;
  background-size: cover;
  background-image: linear-gradient( rgba(255, 255, 255, 0) 80%, rgba(102, 102, 102, 1) 100%) , url('../assets/pleasure-garden-1200-by-768.jpg');
  background-repeat: no-repeat;
  background-position: center;
}

.header__avatar {
  position: absolute;
  margin-top: -50px;
  width: 100%;
}

.header__avatar img {
  width: 60px;
  height: 60px;
  object-fit: cover;
  border-radius: 50%;
  border: 2px solid #FFFFFF;
}

.controls {
  margin-left: 20px;
  margin-right: 20px;
}

.controls__card {
  box-sizing: border-box;
  margin-top: 7px;
  padding: 6px 20px 6px 20px;
  border-radius: 5px;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.10);
}

.controls__slider .slider {
  -webkit-appearance: none;  /* Override default CSS styles */
  appearance: none;
  width: 100%;
  height: 5px;
  border-radius: 5px;
  margin: 10px 0 10px 0;
  background-color: #000;
}

.controls__slider .slider::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 15px;
  height: 15px;
  border-radius: 50%; 
  cursor: pointer;
  border: 3px solid #FFFFFF;
  box-sizing: content-box;
}

#brightness .controls__slider .slider {
  background-image: -webkit-gradient(
    linear,
    left top,
    right top,
    color-stop(0.5, #25A95B),
    color-stop(0.5, #c8e9d6)
  );
}

#brightness .controls__slider .slider::-webkit-slider-thumb {
  background: #25A95B;
}

#brightness .controls__slider .slider::-moz-range-thumb {
  background: #25A95B;
}

#contrast .controls__slider .slider {
  background-image: -webkit-gradient(
    linear,
    left top,
    right top,
    color-stop(0.5, #4A90E2),
    color-stop(0.5, #c8dbe9)
  );
}

#contrast .controls__slider .slider::-webkit-slider-thumb {
  background: #4A90E2;
}

#contrast .controls__slider .slider::-moz-range-thumb {
  background: #4A90E2;
} 

.controls__header {
  font-weight: 500;
  font-size: 18px;
  line-height: 25px;
}

.controls__instruction {
  font-size: 13px;
}

.upload-card {
  display: flex;
  flex-direction: column;
  height: 268px;
  margin-left: 20px;
  margin-right: 20px;
  margin-top: 29px;
  margin-bottom: 29px;
  box-sizing: border-box;
  text-transform: uppercase;
}

.upload__photo {
  height: 212px;
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
  justify-content: space-between;
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
  /* width: 25.97%; */
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
