<script lang="ts" setup>
import { ref } from "vue";

const buttons = [
  { label: "C", class: "btn dark", value: "clear", clickAction: clear },
  { label: "+/-", class: "btn dark", value: "sign", clickAction: sign },
  { label: "%", class: "btn dark", value: "percent", clickAction: percent },
  { label: "/", class: "btn operator", value: "divide", clickAction: divide },
  { label: "7", class: "btn", value: "7", clickAction: () => append("7") },
  { label: "8", class: "btn", value: "8", clickAction: () => append("8") },
  { label: "9", class: "btn", value: "9", clickAction: () => append("9") },
  {
    label: "x",
    class: "btn operator",
    value: "multiply",
    clickAction: multiply,
  },
  { label: "4", class: "btn", value: "4", clickAction: () => append("4") },
  { label: "5", class: "btn", value: "5", clickAction: () => append("5") },
  { label: "6", class: "btn", value: "6", clickAction: () => append("6") },
  { label: "-", class: "btn operator", value: "minus", clickAction: minus },
  { label: "1", class: "btn", value: "1", clickAction: () => append("1") },
  { label: "2", class: "btn", value: "2", clickAction: () => append("2") },
  { label: "3", class: "btn", value: "3", clickAction: () => append("3") },
  { label: "+", class: "btn operator", value: "plus", clickAction: plus },
  { label: "0", class: "btn zero", value: "0", clickAction: () => append("0") },
  { label: ".", class: "btn", value: "dot", clickAction: dot },
  { label: "=", class: "btn operator", value: "equal", clickAction: equal },
];

let current = ref("0");
let previous = "";
let operator: Function;
let isOperatorClicked = false;

function append(number: String) {
  if (isOperatorClicked) current.value = "";
  isOperatorClicked = false;
  current.value =
    current.value === "0" ? `${number}` : `${current.value}${number}`;
}

function plus() {
  if (previous !== "") equal();
  operator = (a, b) => a + b;
  setPreviousNumber();
}

function minus() {
  if (previous !== "") equal();
  operator = (a, b) => a - b;
  setPreviousNumber();
}

function multiply() {
  if (previous !== "") equal();
  operator = (a, b) => a * b;
  setPreviousNumber();
}

function divide() {
  if (previous !== "") equal();
  operator = (a, b) => a / b;
  setPreviousNumber();
}

function setPreviousNumber() {
  previous = current.value;
  isOperatorClicked = true;
}

function equal() {
  current.value = `${operator(Number(previous), Number(current.value))}`;
  previous = "";
  operator;
  isOperatorClicked = false;
}

function clear() {
  current.value = "0";
  previous = "";
  operator;
  isOperatorClicked = false;
}

function sign() {
  if (!isOperatorClicked) {
    current.value =
      current.value.charAt(0) === "-"
        ? current.value.slice(1)
        : `-${current.value}`;
  }
}

function percent() {
  current.value = `${Number(current.value) / 100}`;
}

function dot() {
  if (current.value.indexOf(".") === -1) {
    append(".");
  }
}
</script>

<template>
  <div class="flexbox">
    <div class="calculator">
      <input height="32px" class="display" v-model="current" />
      <div
        v-for="btn in buttons"
        :key="btn.value"
        :class="btn.class"
        @click="btn.clickAction"
      >
        {{ btn.label }}
      </div>
    </div>
  </div>
</template>

<style scoped>
.flexbox {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100vw;
  height: 100vh;
}

.calculator {
  display: grid;
  margin: 0 auto;
  font-size: 40px;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(50px, auto);
  border: 0.3px solid #868383;
  border-radius: 3rem;
  padding: 0.8em;
  background-color: #222;
}

.display {
  grid-column: 1/5;
  grid-row: 1/3;
  text-align: right;
  padding-right: 20px;
  background-color: #d3d3d3;
  color: #333;
  margin-bottom: 10px;
  border-radius: 16px;
  outline: 0;
  font-size: 2rem;
}

.btn {
  border: 1px solid black;
  background-color: #999;
  cursor: pointer;
  border-radius: 40px;
  margin: 10px;
  color: #222;
  font-weight: bold;
  text-align: center;
  transition: background-color 0.2s ease-in;
}

.btn:hover {
  background-color: #868383;
}
.dark {
  background-color: #333;
  color: #ffffff;
}
.operator {
  background-color: #e08d1f;
}
.zero {
  grid-column: 1/3;
}
</style>
