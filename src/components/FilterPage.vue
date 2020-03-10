<template>
  <div class="page-content">

    <div class="controls">
      <Slider id="brightness" name="Brightness" v-bind:disabled="disabled" :update="updateBrightness" message="Slide to adjust image brightness! ☀️" />
      <Slider id="contrast" name="Contrast" v-bind:disabled="disabled" :update="updateContrast" message="Slide to adjust image contrast! 🌓" />
    </div>

    <Uploader v-on:updateDisabled="updateDisabled" v-bind:brightness="brightness" v-bind:contrast="contrast" />
  </div>
</template>

<script>
import Slider from './Slider.vue'
import Uploader from './Uploader.vue'

export default {
  name: 'FilterPage',
  props: {
    msg: String
  },
  components: {
    Slider,
    Uploader
  },
  data: 
    function() {
      return {
        brightness: 0,
        contrast: 0,
        disabled: true
      }
    },
  methods: {
    updateDisabled (value) {
      this.disabled = value
    },
    updateBrightness (e) {
      var val = (e.target.valueAsNumber + 100) / 200
      document.getElementById('brightnessRange').style.backgroundImage = '-webkit-gradient(linear, left top, right top, ' +
        'color-stop(' + val + ', #25A95B), ' + 'color-stop(' + val + ', #c8e9d6)' + ')'

      this.brightness = e.target.valueAsNumber
    },
    updateContrast (e) {
      var val = (e.target.valueAsNumber + 100) / 200
      document.getElementById('contrastRange').style.backgroundImage = '-webkit-gradient(linear, left top, right top, ' +
        'color-stop(' + val + ', #4A90E2), ' + 'color-stop(' + val + ', #c8dbe9)' + ')'

      this.contrast = e.target.valueAsNumber
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.controls {
  margin-left: 20px;
  margin-right: 20px;
}

</style>
