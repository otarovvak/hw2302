<template>
  <div>
    <input type="text" v-model="currentInput">
    <button @click="appendNumber('7')">7</button>
    <button @click="appendNumber('8')">8</button>
    <button @click="appendNumber('9')">9</button>
    <br>
    <button @click="appendNumber('4')">4</button>
    <button @click="appendNumber('5')">5</button>
    <button @click="appendNumber('6')">6</button>
    <br>
    <button @click="appendNumber('1')">1</button>
    <button @click="appendNumber('2')">2</button>
    <button @click="appendNumber('3')">3</button>
    <br>
    <button @click="appendNumber('0')">0</button>
    <button @click="clearScreen">C</button>
    <br>
    <button @click="appendOperator('+')">+</button>
    <button @click="appendOperator('-')">-</button>
    <button @click="appendOperator('*')">*</button>
    <button @click="appendOperator('/')">/</button>
    <button @click="calculate">=</button>
    <p style="text-align: right;">{{ result }}</p>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const currentInput = ref('');
const previousInput = ref('');
const operator = ref('');
const result = ref('');

const appendNumber = (number: string) => {
  currentInput.value += number;
};

const appendOperator = (op: string) => {
  if (currentInput.value !== '') { 
    previousInput.value = currentInput.value; 
    currentInput.value = '';
    operator.value = op;
  } else if (op === '-') {
    currentInput.value += '-';
  }
};

const clearScreen = () => {
  currentInput.value = '';
  previousInput.value = '';
  operator.value = '';
  result.value = '';
};

const calculate = () => {
  if (previousInput.value === '' || operator.value === '') {
    return; // Early return if no operation
  }

  let tempResult = 0;
  const previous = parseFloat(previousInput.value);
  const current = parseFloat(currentInput.value);
  let operation = previousInput.value + ' ' + operator.value + ' ' + currentInput.value + ' = ';
  switch (operator.value) {
    case '+':
      tempResult = previous + current;
      break;
    case '-':
      tempResult = previous - current;
      break;
    case '*':
      tempResult = previous * current;
      break;
    case '/':
      if (current === 0) {
        result.value = 'Error: Division by zero';
        return;
      } else {
        tempResult = previous / current;
      }
      break;
    default:
      break;
  }
  if (!isNaN(tempResult)) {
    result.value = operation + tempResult.toString();
    currentInput.value = tempResult.toString();
  }
  operator.value = '';
  previousInput.value = '';
};
</script>
