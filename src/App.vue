<script setup>
import { ref, onMounted, computed, watch } from 'vue'
const todos = ref([])
const name = ref('')
const input_content = ref('')
const input_category = ref(null)


const addTodo = () => {
  if (input_content.value.trim() == '' || input_category.value==null) {
    return;
  } else {
    todos.value.push({
      content: input_content.value,
      category: input_category.value,
      done: false,
      createdAt: new Date().getTime()
    })
    input_content.value = '';
    
  }
 
}
watch(todos, newVal => {
 localStorage.setItem('todos',JSON.stringify(newVal))
},{deep:true})
watch(name, (newVal) => {
  localStorage.setItem('name', newVal);
})
onMounted(() => {
  name.value = localStorage.getItem('name') || '',
    todos.value = JSON.parse(localStorage.getItem('todos')) || []
})
const removeToDo=(todo) => {
  todos.value = todos.value.filter(t => t != todo);
}
</script>

<template>
 <main class="app">
  <section class="greeting">
    <h2 class="title">
      What's up, <input type="text" v-model="name" placeholder="Name here...">
    </h2>
  </section>
  <section class="create-todo">
    <h3>CREATE A TODO</h3>
    <form @submit.prevent="addTodo()">
      <h4>What's on your todo list?</h4>
      <input type="text" placeholder="ed:make a video" v-model="input_content">
     <h4>Make a category</h4>
     <div class="options">
      <label >
        <input type="radio" name="category"  value="business" v-model="input_category">
        <span class="bubble business"></span>
        <div>Business</div>
      </label>
      <label >
        <input type="radio" name="personal" value="personal" v-model="input_category">
        <span class="bubble personal"></span>
        <div>Personal</div>
      </label>
     </div>
     <input type="submit" value="Add todo">
    </form>
  </section>
  <section class="todo-list">
    <h3>TODO List</h3>
    <div class="list" id="todo-list">
      <div v-for="(todo,index) in todos" :class="['todo-item',(todo.done? 'done' :'') ]" :key="index">
       <label>
        <input type="checkbox"  v-model="todo.done" >
        <span :class="['bubble', (todo.category === 'business' ? 'business' : 'personal')]"></span>
       </label>
       <div class="todo-content">
       <input type="text" v-model="todo.content">
       </div>
       <div class="actions">
        <button class="delete" @click="removeToDo(todo)">Delete</button>
       </div>
      </div>
    </div>
  </section>
 </main>
</template>
