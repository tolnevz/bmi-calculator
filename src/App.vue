<style src="./assets/styles.scss" lang="scss" scoped></style>

<template>
  <div class="container mt-4">
    <div class="row">
      <div class="col-sm-12">
        <div class="control">
          <div class="control-indicator mb-2">
            <div class="control-title">Pet Name</div>
          </div>
          <div class="mb-3">
            <input type="text" class="form-control" :class="errors.petName ? 'is-invalid' : ''" v-model="petName" />
          </div>
          <div class="invalid-control text-center" v-if="errors.petName">{{ errors.petName }}</div>
        </div>
      </div>

      <div class="col-sm-12 col-md-6">
        <div class="control">
          <div class="control-indicator mb-2">
            <div class="control-title">Weight, kg</div>
            <input type="number" :max="max" :min="min" class="form-control-plaintext" :class="errors.weight ? 'is-invalid' : ''" v-model="weight" />
          </div>
          <div class="d-flex align-items-center justify-content-between">
            <div class="me-3 pb-2">{{ min }}</div>
            <div class="w-100">
              <input type="range" class="form-range" :min="min" :max="max" step="0.1" v-model="weight" />
            </div>
            <div class="ms-3 pb-2">{{ max }}</div>
          </div>
          <div class="invalid-control text-center" v-if="errors.weight">{{ errors.weight }}</div>
        </div>
      </div>

      <div class="col-sm-12 col-md-6">
        <div class="control">
          <div class="control-indicator mb-2">
            <div class="control-title">Height, sm</div>
            <input type="number" :max="max" :min="min" class="form-control-plaintext" :class="errors.height ? 'is-invalid' : ''" v-model="height" />
          </div>
          <div class="d-flex align-items-center justify-content-between">
            <div class="me-3 pb-2">{{ min }}</div>
            <div class="w-100">
              <input type="range" class="form-range" :min="min" :max="max" step="0.1" v-model="height" />
            </div>
            <div class="ms-3 pb-2">{{ max }}</div>
          </div>
          <div class="invalid-control text-center" v-if="errors.height">{{ errors.height }}</div>
        </div>
      </div>

      <div class="col-sm-12">
        <div v-if="bmi">
          <BmiIndicator :bmi="bmi" :name="petName" />
        </div>
        <div class="no-message" v-else>Please enter your pet's name and select weight and height.</div>
      </div>
    </div>
  </div>
</template>

<script>
import BmiIndicator from './components/BmiIndicator.vue';

export default {
  components: {BmiIndicator},
  data() {
    return {
      petName: '',
      bmi: null,
      weight: 0,
      height: 0,
      min: 0,
      max: 100,
      errors: {
        weight: '',
        height: '',
        petName: '',
      },
    };
  },
  mounted() {
    this.caculateBmi();
  },
  watch: {
    weight(newValue, oldWalue) {
      this.errors.weight = this.bmiParamsErrorHandler(newValue, 'Weight');
      this.caculateBmi();
    },
    height(newValue, oldWalue) {
      this.errors.height = this.bmiParamsErrorHandler(newValue, 'Height');
      this.caculateBmi();
    },
    petName(newValue, oldWalue) {
      this.errors.petName = this.bmiParamsErrorHandler(newValue, 'petName');
      this.caculateBmi();
    },
  },
  methods: {
    caculateBmi() {
      if (this.petName && this.weight > this.min && this.height > this.min && this.weight <= this.max && this.height <= this.max) {
        this.bmi = (this.weight / ((this.height * this.height) / 10000)).toFixed(2);
      } else {
        this.bmi = null;
      }
    },
    bmiParamsErrorHandler(value, param) {
      if (param === 'petName') {
        return value ? null : 'Every pet should have a name!';
      }
      return value > this.max ? `${param} must be less or equal ${this.max}` : value < this.min ? `${param} must be more than ${this.min}` : null;
    },
  },
};
</script>
