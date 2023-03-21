<template>
  <div class="range-slider">
    <span ref="rangeSlider__bullet" class="range-slider__label">0</span>
    <input
      ref="rangeSlider__line"
      class="range-slider__range"
      type="range"
      min="0"
      max="10"
      v-model="sliderValue"
    />
  </div>
</template>

<script>
export default {
  name: "ratingSlider",

  data() {
    return {
      sliderValue: 0,
    };
  },

  watch: {
    sliderValue() {
      this.showSliderValue();
    },
  },

  methods: {
    showSliderValue() {
      let rangeSlider = this.$refs.rangeSlider__line
      let rangeBullet = this.$refs.rangeSlider__bullet

      rangeBullet.innerHTML = rangeSlider.value;

      let calc = ((rangeSlider.value / rangeSlider.max) * 92.5) + "%"

      rangeBullet.style.left = calc;
      
      this.$emit('returnValue', this.sliderValue)
    },
  },
};
</script>

<style lang="scss">
.range-slider__range {
  margin: 24px 0 32px 0;
  width: 100%;
  -webkit-appearance: none;

  &::-webkit-slider-runnable-track {
    width: 100%;
    height: 5px;

    cursor: pointer;
    box-shadow: none;
    background: #a3a2a2;
    border-radius: 5px;
  }

  &::-webkit-slider-thumb {
    box-shadow: none;
    border: 0px solid #ffffff;
    box-shadow: 0px 10px 10px rgba(0, 0, 0, 0.25);
    height: 30px;
    width: 30px;
    border-radius: 22px;
    background: rgb(49, 49, 49);
    cursor: pointer;
    -webkit-appearance: none;
    margin-top: -14px;
  }
}

.range-slider__label {
  top: 41px;
  position: relative;
  transform-origin: center center;

  pointer-events: none;

  display: flex;
  justify-content: center;
  align-items: center;

  width: 30px;
  height: 30px;

  box-sizing: border-box;
  left: attr(value);
  color: #ffffff;
}
</style>
