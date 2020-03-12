<template>
  <div class="page-content">
    <div class="page-content__sliders">
      <Slider
        id="brightness"
        name="Brightness"
        v-bind:disabled="disabled"
        :update="updateBrightness"
        message="Slide to adjust image brightness! ☀️"
        v-bind:colorStop="brightnessColorStop"
        highlightColor="#25A95B"
        trackColor="#c8e9d6"
      />

      <Slider
        id="contrast"
        name="Contrast"
        v-bind:disabled="disabled"
        :update="updateContrast"
        message="Slide to adjust image contrast! 🌓"
        v-bind:colorStop="contrastColorStop"
        highlightColor="#4A90E2"
        trackColor="#c8dbe9"
      />
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
        disabled: true,
        brightnessColorStop: "0.50",
        contrastColorStop: "0.50"
      }
    },
  methods: {
    updateDisabled (value) {
      this.disabled = value;
    },
    updateBrightness (e) {
      this.brightnessColorStop = "" + ((e.target.valueAsNumber + 100) / 200) + "";
      this.brightness = e.target.valueAsNumber;
    },
    updateContrast (e) {
      this.contrastColorStop = "" + ((e.target.valueAsNumber + 100) / 200) + "";
      this.contrast = e.target.valueAsNumber;
    },
  }
}
</script>

<style scoped>

.page-content__sliders {
  margin-left: 20px;
  margin-right: 20px;
}

</style>
