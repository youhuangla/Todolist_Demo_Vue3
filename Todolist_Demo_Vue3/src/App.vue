<template>
  <div id="app">
    <h2>{{ title }}</h2>
    <input type="text" v-model="title">
  </div>
  <ul>
    <li v-for="todo in todos">
      {{ todo }}
    </li>
  </ul>
</template>

<script setup>
import { ref } from 'vue';

// 定义一个响应式数据
const title = ref('');
const todos = ref(['Eat', 'Sleep', 'Code']);
</script>

<style>
/* 在这里定义组件的样式 */
</style>
