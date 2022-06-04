<template>
  <input type="text" v-model="newLabel" @keydown.enter="addTodo" />
  <button @click="addTodo">新增</button>
  <span class="empty-tip" v-if="showTip">不能为空！</span>
  <ul>
    <transition-group tag="ul" name="slide-list">
      <li v-for="(item, index) in list" :key="item.label">
        <input type="checkbox" v-model="item.done" />
        {{ item.label }}
        <button @click="deleteTodo(index)">删除</button>
      </li>
    </transition-group>
  </ul>
  <div>统计：{{ doneLength }} / {{ allLength }}</div>
  <div>全选：<input type="checkbox" v-model="ifAllDone" /></div>
</template>
<script setup>
import { computed, ref } from 'vue'
let newLabel = ref('')
let showTip = ref(false)
let list = ref([
  { label: 'todo1', done: false },
  { label: 'todo2', done: true },
  { label: 'todo3', done: false },
])
const addTodo = () => {
  newLabel.value = newLabel.value.trim()
  if (!newLabel.value) {
    showTip.value = true
    setTimeout(() => {
      showTip.value = false
    }, 3000)
  } else {
    showTip.value = false
    list.value.push({ label: newLabel.value, done: false })
    newLabel.value = ''
  }
}
let allLength = computed(() => list.value.length)
let doneLength = computed(() => {
  return list.value.filter((item) => item.done).length
})
const deleteTodo = (i) => {
  list.value.splice(i, 1)
}
let ifAllDone = computed({
  get() {
    return doneLength.value === allLength.value
  },
  set(val) {
    list.value.forEach((item) => {
      item.done = val
    })
  },
})
</script>
<style>
ul {
  /* padding-left: 20px; */
}
li {
  list-style: none;
  margin: 20px;
}
input[type='text'] {
  height: 24px;
  outline: none;
}
input {
  cursor: pointer;
}
button {
  margin: 0 12px;
  cursor: pointer;
}
.empty-tip {
  color: rgb(209, 50, 50);
}
.slide-list-enter-from,
.slide-list-leave-to {
  transform: translateX(30px);
  opacity: 0;
}
.slide-list-enter-active,
.slide-list-leave-active {
  transition: all 0.3s ease;
}
.slide-list-move {
  transition: transform 0.3s ease;
}
</style>