<template>
  <div class="calculator">
    <h1 style="color:#000; padding-bottom: 30px;">Calculadora Vue.js</h1>
    <div class="history">{{ history }}</div>
    <div class="display">{{ display }}</div>
    <div class="buttons">
      <button v-for="button in buttons" :key="button" @click="handleClick(button)">
        {{ button }}
      </button>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  name: 'App',
  setup() {
    const display = ref('0');
    const history = ref('');
    const buttons = [
      '7', '8', '9', '+',
      '4', '5', '6', '-',
      '1', '2', '3', '*',
      'C', '0', '=', '/',
    ];
    let leftOperand = null;
    let operator = null;
    let result = null;

    const handleClick = (button) => {
      if (button >= '0' && button <= '9') {
        if (display.value === '0') {
          display.value = button;
        } else {
          display.value += button;
        }
        if (operator) {
          history.value = `${result} ${operator} ${display.value}`;
        } else {
          history.value += ` ${button}`;
        }
      } else if (button === 'C') {
        display.value = '0';
        history.value = '';
        leftOperand = null;
        operator = null;
        result = null;
      } else if (button === '=') {
        const rightOperand = Number(display.value);
        switch (operator) {
          case '+':
            result += rightOperand;
            break;
          case '-':
            result -= rightOperand;
            break;
          case '*':
            result *= rightOperand;
            break;
          case '/':
            result /= rightOperand;
            break;
          default:
            result = rightOperand;
        }
        history.value = `${result} ${operator} ${display.value} = ${result}`;
        display.value = result;
        leftOperand = null;
        operator = null;
      } else {
        const currentNumber = Number(display.value);
        if (operator) {
          switch (operator) {
            case '+':
              result += currentNumber;
              break;
            case '-':
              result -= currentNumber;
              break;
            case '*':
              result *= currentNumber;
              break;
            case '/':
              result /= currentNumber;
              break;
          }
          history.value = `${result} ${operator} ${display.value}`;
        } else {
          result = currentNumber;
          history.value = `${currentNumber} ${button}`;
        }
        leftOperand = currentNumber;
        operator = button;
        display.value = '0';
      }
    };

    return {
      display,
      history,
      buttons,
      handleClick,
    };
  },
};
</script>

<style>
.calculator {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #eee;
}



.history {
  width: 300px;
  height: 50px;
  font-size: 18px;
  text-align: right;
  padding: 10px;
  border: 1px solid #ccc;
  margin-bottom: 20px;
  background-color: #fff;
  color: black;
}

.display {
  width: 300px;
  height: 50px;
  font-size: 24px;
  text-align: right;
  padding: 10px;
  border: 1px solid #ccc;
  margin-bottom: 20px;
  background-color: #fff;
  color: black;
}

.buttons {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-gap: 10px;
}

button {
  width: 70px;
  height: 70px;
  font-size: 24px;
  background-color: #fff;
  border: 1px solid #ccc;
  cursor: pointer;
}
</style>
