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
            <input type="range" min="1" max="100" value="50" class="slider" id="myRange" />
          </div>
          <div class="controls__instruction">Slide to adjust image brightness! ☀️</div>
        </div>

        <div class="controls__card" id="contrast">
          <div class="controls__header" :style="{color: '#4A90E2'}">Contrast</div>
          <div class="controls__slider">
            <input type="range" min="1" max="100" value="50" class="slider" id="myRange" />
          </div>
          <div class="controls__instruction">Slide to adjust image contrast! 🌓</div>
        </div>
      </div>

      <div class="upload-card">
        <div class="upload__photo">
          <img :src="image" />

          <canvas id="Canvas" class="upload-canvas"></canvas>
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

  <!--<h1>{{ msg }}</h1>
  <p>
    For a guide and recipes on how to configure / customize this project,<br>
    check out the
    <a href="https://cli.vuejs.org" target="_blank" rel="noopener">vue-cli documentation</a>.
  </p>
  <h3>Installed CLI Plugins</h3>
  <ul>
    <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-babel" target="_blank" rel="noopener">babel</a></li>
    <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-eslint" target="_blank" rel="noopener">eslint</a></li>
  </ul>
  <h3>Essential Links</h3>
  <ul>
    <li><a href="https://vuejs.org" target="_blank" rel="noopener">Core Docs</a></li>
    <li><a href="https://forum.vuejs.org" target="_blank" rel="noopener">Forum</a></li>
    <li><a href="https://chat.vuejs.org" target="_blank" rel="noopener">Community Chat</a></li>
    <li><a href="https://twitter.com/vuejs" target="_blank" rel="noopener">Twitter</a></li>
    <li><a href="https://news.vuejs.org" target="_blank" rel="noopener">News</a></li>
  </ul>
  <h3>Ecosystem</h3>
  <ul>
    <li><a href="https://router.vuejs.org" target="_blank" rel="noopener">vue-router</a></li>
    <li><a href="https://vuex.vuejs.org" target="_blank" rel="noopener">vuex</a></li>
    <li><a href="https://github.com/vuejs/vue-devtools#vue-devtools" target="_blank" rel="noopener">vue-devtools</a></li>
    <li><a href="https://vue-loader.vuejs.org" target="_blank" rel="noopener">vue-loader</a></li>
    <li><a href="https://github.com/vuejs/awesome-vue" target="_blank" rel="noopener">awesome-vue</a></li>
  </ul>-->
  </div>
</template>

<script>
var canvas;
var context;

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data: 
    function() {
      return {
        image: '',
        filename: '',
      }
    },
  methods: {
    onFileChange(e) {
      var files = e.target.files || e.dataTransfer.files;
      if (!files.length)
        return;
      this.createImage(files[0]);
    },
    createImage(file) {
      var reader = new FileReader();
      var vm = this;

      reader.onload = (e) => {
        vm.image = e.target.result;
        vm.filename = file.name;
        // console.log(file)
      };
      reader.readAsDataURL(file);
    },
    removeImage: function () {
      this.image = '';
    }
  }
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
  /* width: 100%; */
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

.controls__slider .slider::-moz-range-thumb {
  width: 15px;
  height: 15px;
  border-radius: 50%;
  cursor: pointer;
  border: 3px solid #FFFFFF;
  box-sizing: content-box;
}

#brightness .controls__slider .slider {
  background: #4CAF50;
}

#brightness .controls__slider .slider::-webkit-slider-thumb {
  background: #4CAF50;
}

#brightness .controls__slider .slider::-moz-range-thumb {
  background: #4CAF50;
}

#contrast .controls__slider .slider {
  background: #4A90E2;
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
  /* object-fit: cover;
  width: calc(100vw - 40px); */
}

.upload__photo img {
  /* width: 100%; */
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
  font-size: 12px;
  letter-spacing: 0.55px;
  font-weight: 500;
  text-transform: uppercase;
  color: #4A90E2;

}

</style>
