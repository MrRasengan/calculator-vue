<template>
  <div class="calculator">
    <h1>Калькулятор</h1>
    <input v-model="input" placeholder="0" readonly />
    <h2 v-if="result !== null">Результат: {{ result }}</h2>
    <div class="buttons">
      <button v-for="num in numbers" :key="num" @click="appendToInput(num)">{{ num }}</button>
      <button v-for="op in operators" :key="op" @click="appendToInput(op)">{{ op }}</button>
      <button @click="calculate">=</button>
      <button @click="clear">C</button>
    </div>
  </div>
</template>

<script>
import { evaluate } from 'mathjs';

export default {
  data() {
    return {
      input: '',
      result: null,
      numbers: [1, 2, 3, 4, 5, 6, 7, 8, 9, 0],
      operators: ['+', '-', '*', '/', '.'],
    };
  },
  methods: {
    appendToInput(value) {
      if (this.isValidInput(value)) {
        this.input += value.toString();
        this.result = null; // Сброс результата при вводе нового значения
      }
    },
    isValidInput(value) {
      const lastChar = this.input[this.input.length - 1];
      const operators = ['+', '-', '*', '/'];
      return !(operators.includes(value) && operators.includes(lastChar));
    },
    calculate() {
      try {
        this.result = evaluate(this.input);
        this.input = ''; // Сброс ввода после вычисления
      } catch (error) {
        alert('Ошибка в выражении');
        this.result = null;
      }
    },
    clear() {
      this.input = '';
      this.result = null;
    },
  },
};
</script>


<style scoped>
.calculator {
  max-width: 400px;
  margin: auto;
  padding: 20px;
  border: 1px solid #000;
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
  background-color: #fff;
}

input {
  width: calc(100% - 20px);
  padding: 10px;
  margin-bottom: 20px;
  font-size: 24px;
  text-align: right;
  border: 1px solid #000;
  background-color: #f0f0f0;
}

.buttons {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 10px;
}

button {
  padding: 20px;
  font-size: 18px;
  background-color: #000;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #444; /* Темнее при наведении */
}

h2 {
  margin-top: 20px;
}
</style>
