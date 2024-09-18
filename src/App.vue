<template>
  <div id="app">
    <Calculator />
  </div>
</template>

<script>
import Calculator from './components/CalculatorApp.vue';

export default {
  components: {
    Calculator,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
}
</style>
