<template>
  <div id="app">
    <h2>{{ title }}</h2>
    <input type="text" v-model="title" @keydown.enter="addTodo">
  </div>
  <ul v-if="todos.length">
    <li v-for="todo in todos" :key="todo.title">
      <input type="checkbox" v-model="todo.done">
      <span :class="{ done: todo.done }">{{ todo.title }}</span>
    </li>
  </ul>
  <div v-else>
    <p>Nothing to do</p>
  </div>
  <div>
    <input type="checkbox" v-model="allDone"> Select All
    <span>{{ active }} / {{ all }}</span>
  </div>
  <div v-if="active < all">
    <button @click="clear">Clear</button>
  </div>
</template>

<script setup>
import { computed, ref, onMounted, onBeforeUnmount } from 'vue';
const localStorageKey = 'todosData';

// 定义一个响应式数据
const title = ref('');
const todos = ref(JSON.parse(localStorage.getItem(localStorageKey)) || [
  { title: 'Eat', done: false },
  { title: 'Sleep', done: false },
  { title: 'Code', done: false }
]);

const saveDataToLocalStorage = () => {
  localStorage.setItem(localStorageKey, JSON.stringify(todos.value));
};

onMounted(() => {
  const savedData = JSON.parse(localStorage.getItem(localStorageKey));
  if (savedData) {
    todos.value = savedData;
  }
});

onBeforeUnmount(() => {
  saveDataToLocalStorage();
});

const addTodo = () => {
  todos.value.push({ title: title.value, done: false });
  title.value = '';
  saveDataToLocalStorage();
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
    saveDataToLocalStorage();
  }
});
const clear = () => {
  todos.value = todos.value.filter(v => !v.done);
  saveDataToLocalStorage();
};
</script>

<style>
/* 在这里定义组件的样式 */
.done {
  color: gray;
  text-decoration: line-through;
}
</style>
