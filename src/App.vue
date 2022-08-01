<template>
  <div class="flex flex-col max-w-md items-center">
    <InputField v-model="value" />
    <div>Total: {{ total }}</div>
    <div>Value: {{ value }}</div>
    <div class="flex ">
      <div class="grid grid-cols-4 gap-4 mx-4">
        <div v-for="calculatorKey in calculatorKeys" :key="calculatorKey" @click="pressed(calculatorKey)"
          class="bg-red-400 p-6 cursor-pointer">{{ calculatorKey }}</div>
      </div>
    </div>
  </div>

</template>

<script>
import InputField from './components/InputField.vue'

export default {
  name: 'App',
  components: {
    InputField
  },
  data() {
    return {
      value: 0,
      total: 0,
      calculatorKeys: ['(', ')', '%', 'AC', 7, 8, 9, '/', 4, 5, 6, 'x', 1, 2, 3, '-', 0, '.', '=', '+'],
    }
  },
  methods: {

    pressed(calculatorKey) {
      switch (calculatorKey) {
        case '+':
          console.log(calculatorKey)
          this.sum();
          break;
        case '-':
          console.log(calculatorKey)
          this.sub();
          break;
        case 'x':
          console.log(calculatorKey)
          this.mul();
          break;
        default:
      }
    },

    sum() {
      this.total += this.value;
    },
    sub() {
      this.total -= this.value;
    },
    mul() {
      this.total *= this.value;
    }
  }
}
</script>

<style>
</style>
