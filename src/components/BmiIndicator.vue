<style src="../assets/styles.scss" lang="scss" scoped></style>

<template>
  <div class="bmi-result">
    <div class="pet-name">{{ name }}'s BMI is</div>
    <div class="bmi-score">{{ bmi }}</div>
    <div class="bmi-scale">
      <div class="scale-area" :class="{underweight: bmi < 30}"></div>
      <div class="scale-area" :class="{normal: bmi <= 90 && bmi >= 30}"></div>
      <div class="scale-area" :class="{overweight: bmi > 90}"></div>
    </div>
    <div class="bmi-description">
      <div class="description-name" :class="{active: bmi < 30}">Underweight</div>
      <div class="description-name" :class="{active: bmi <= 90 && bmi >= 30}">Normal</div>
      <div class="description-name" :class="{active: bmi > 90}">Overweight</div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['bmi', 'name'],
};
</script>
