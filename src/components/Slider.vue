<template>
  <div class="controls__card" v-bind:id="id">
    <div class="controls__header">{{ name }}</div>
    <div class="controls__slider">
      <input type="range" min="-100" max="100" value="0" class="slider" v-bind:id="id + 'Range'" @input="update" v-bind:disabled="disabled" />
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
    disabled: Boolean
  },
}
</script>

<style scoped>

.controls__card {
  box-sizing: border-box;
  margin-top: 7px;
  padding: 6px 20px 6px 20px;
  border-radius: 5px;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.10);
}

.controls__header {
  font-weight: 500;
  font-size: 18px;
  line-height: 25px;
}

.controls__instruction {
  font-size: 13px;
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

#brightness .controls__header {
  color: #25A95B;
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

#contrast .controls__header {
  color: #4A90E2;
}

#contrast .controls__slider .slider::-webkit-slider-thumb {
  background: #4A90E2;
}

#contrast .controls__slider .slider::-moz-range-thumb {
  background: #4A90E2;
} 
</style>