<template>
<div :class="['calculator', theme]">
  <button @click="toggleTheme">Сменить тему</button>
  <div class="calculator" @keydown="handleKeyPress" tabindex="0">
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
      theme: 'light', // Добавлено состояние темы
    };
  },
  methods: {
    toggleTheme() {
    this.theme = this.theme === 'light' ? 'dark' : 'light';
  },
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
    handleKeyPress(event) {
      const key = event.key;

      if (this.numbers.includes(Number(key))) {
        this.appendToInput(Number(key));
      } else if (this.operators.includes(key)) {
        this.appendToInput(key);
      } else if (key === 'Enter') {
        this.calculate();
      } else if (key === 'Backspace') {
        this.clear();
      }
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
  background-color: var(--bg-color);
}

.input {
  width: calc(100% - 20px);
  padding: 10px;
  margin-bottom: 20px;
  font-size: 24px;
  text-align: right;
  border: 1px solid #000;
  background-color: var(--input-bg-color);
}

.button {
  padding: 20px;
  font-size: 18px;
  background-color: var(--button-bg-color);
  color: var(--button-text-color);
  border: none;
  border-radius: 5px;
  cursor: pointer;
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

/* Темная тема */
.dark {
  --bg-color: #333;
  --input-bg-color: #555;
  --button-bg-color: #000;
  --button-text-color: white;
}

.light {
  --bg-color: #fff;
  --input-bg-color: #f0f0f0;
  --button-bg-color: #000;
  --button-text-color: white;
}
</style>
