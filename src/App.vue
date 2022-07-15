<script setup>
import {ref, onMounted, computed, watch, camelize} from "vue";

const todos = ref([])
const name = ref('')
const input_content = ref('')
const input_category = ref(null)
const todos_asc = computed(() => todos.value.sort((a, b) => {
  return b.createAt - a.createAt
}))
const addTodo = () => {
  if (input_content.value.trim() === '' || input_category.value === null) {
    return
  }
  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    createAt: new Date().getTime()
  })
  input_content.value = ''
  input_category.value = null
}
const removeTodo = todo =>{
  todos.value = todos.value.filter(t => t !== todo)
}
watch(todos, newVal => {
  localStorage.setItem('todos', JSON.stringify(newVal))
}, {deep: true})
watch(name, (newVal) => {
  localStorage.setItem('name', newVal)
})
onMounted(() => {
  name.value = localStorage.getItem('name') || ''
  todos.value = JSON.parse(localStorage.getItem('todos')) || []
})
</script>

<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        what`s up , <input type="text" placeholder="name here" v-model="name">
      </h2>
    </section>
    <section class="create-todo">
      <h3>
        CREATE TODO LIST
      </h3>
      <form @submit.prevent="addTodo">
        <h4>
          chi to barnaet ast ?!
        </h4>
        <input type="text"
               placeholder="barname azafe kon "
               v-model="input_content"/>

        <h4>
          pick category
        </h4>
        <div class="options">
          <label>
            <input
                type="radio"
                name="category"
                id="category1"
                value="business"
                v-model="input_category"/>
            <span class="bubble business"></span>
            <div> business</div>
          </label>
          <label>
            <input
                type="radio"
                name="category"
                id="category1"
                value="personal"
                v-model="input_category"/>
            <span class="bubble personal"></span>
            <div> personal</div>
          </label>
        </div>
        <input type="submit" value="Add todo"/>
      </form>
    </section>
    <section class="todo-list">
      <h3> todo list</h3>
      <div class="list">
        <div v-for=" todos in todos_asc" :class="`todo-item ${todos.done && 'done'}`">
          <label>
            <input type="checkbox" v-model=" todos.done "/>
            <span :class="`bubble ${todos.category}`"></span>
          </label>
          <div class="todo-content">
            <input type="text" v-model="todos.content">
          </div>
          <div class="action">
            <button class="delete" @click="removeTodo(todos)">
              delete
            </button>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>