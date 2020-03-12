<template>
  <div class="controls__card" v-bind:id="id">
    <div class="controls__header" :style="'color: ' + this.highlightColor">{{ name }}</div>
    <div class="controls__slider">
      <input
        type="range"
        min="-100"
        max="100"
        value="0"
        class="slider"
        v-bind:id="id + 'Range'"
        @input="update"
        v-bind:disabled="disabled"
        v-bind:style="cssVars" />
    </div>
    <div class="controls__instruction">{{ message }}</div>
  </div>
</template>

<script>
export default {
  name: 'Slider',
  props: {
    id: String,
    name: String,
    message: String,
    update: { type: Function },
    disabled: Boolean,
    colorStop: String,
    highlightColor: String,
    trackColor: String
  },
  computed: {
    cssVars() {
      return {
        '--highlight-color': this.highlightColor,
        '--track-color': this.trackColor,
        'background-image': '-webkit-gradient(linear, left top, right top, color-stop(' + this.colorStop + ', ' + this.highlightColor + '), color-stop(' + this.colorStop + ', ' + this.trackColor + '))'
      }
    }
  }
}
</script>

<style lang="scss" scoped>

.controls__card {
  box-sizing: border-box;
  margin-top: 7px;
  padding: 6px 20px 6px 20px;
  border-radius: 5px;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.10);

  .controls__header {
    font-weight: 500;
    font-size: 18px;
    line-height: 25px;
  }

  .controls__instruction {
    font-size: 13px;
  }

  .controls__slider {
    .slider {
      -webkit-appearance: none;  /* Override default CSS styles */
      appearance: none;
      width: 100%;
      height: 5px;
      border-radius: 5px;
      margin: 10px 0 10px 0;
      background-image: -webkit-gradient(
        linear,
        left top,
        right top,
        color-stop(0.5, var(--highlight-color)),
        color-stop(0.5, var(--track-color))
      );

      &::-webkit-slider-thumb {
        -webkit-appearance: none;
        appearance: none;
        width: 15px;
        height: 15px;
        border-radius: 50%; 
        cursor: pointer;
        border: 3px solid #FFFFFF;
        box-sizing: content-box;
        background: var(--highlight-color);
      }

      &::-moz-range-thumb {
        background: var(--highlight-color);
      }
    }
  }
}
</style>