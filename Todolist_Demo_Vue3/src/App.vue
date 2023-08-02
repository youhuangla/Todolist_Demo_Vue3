<template>
  <div id="app">
    <h2>{{ title }}</h2>
    <input type="text" v-model="title" @keydown.enter="addTodo">
  </div>
  <ul>
    <li v-for="todo in todos" :key="todo.title">
      <input type="checkbox" v-model="todo.done">
      <span :class="{ done: todo.done }">{{ todo.title }}</span>
    </li>
  </ul>
  <div>
    <input type="checkbox" v-model="allDone"> Select All
    <span>{{ active }} / {{ all }}</span>
  </div>
</template>

<script setup>
import { computed, ref } from 'vue';


// 定义一个响应式数据
const title = ref('');
const todos = ref([
  { title: 'Eat', done: false },
  { title: 'Sleep', done: false },
  { title: 'Code', done: false }
]);
const addTodo = () => {
  todos.value.push({ title: title.value, done: false });
  title.value = '';
};
const active = computed(() => {
  return todos.value.filter(v => !v.done).length;
});
const all = computed(() => {
  return todos.value.length;
});
const allDone = computed({
  get() {
    return active.value === 0;
  },
  set(value) {
    todos.value.forEach(v => v.done = value);
  }
});
</script>

<style>
/* 在这里定义组件的样式 */
.done {
  color: gray;
  text-decoration: line-through;
}
</style>
