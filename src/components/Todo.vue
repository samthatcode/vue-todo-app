<script setup>
import { ref, onMounted, computed, watch } from 'vue'

const todos = ref([])
const name = ref('')

const input_content = ref('')
const input_category = ref(null)

const todos_asc = computed(() =>
  todos.value.sort((a, b) => {
    return a.createdAt - b.createdAt
  })
)

watch(name, (newVal) => {
  localStorage.setItem('name', newVal)
})

watch(
  todos,
  (newVal) => {
    localStorage.setItem('todos', JSON.stringify(newVal))
  },
  {
    deep: true
  }
)

const addTodo = () => {
  if (input_content.value.trim() === '' || input_category.value === null) {
    return
  }

  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    editable: false,
    createdAt: new Date().getTime()
  })
}

const removeTodo = (todo) => {
  todos.value = todos.value.filter((t) => t !== todo)
}

onMounted(() => {
  name.value = localStorage.getItem('name') || ''
  todos.value = JSON.parse(localStorage.getItem('todos')) || []
})
</script>

<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        What's up, <input type="text" id="name" placeholder="Name here" v-model="name" />
      </h2>
    </section>

    <section class="create-todo">
      <h3>CREATE A TODO</h3>

      <form id="new-todo-form" @submit.prevent="addTodo">
        <h4>What's on your todo list?</h4>
        <input
          type="text"
          name="content"
          id="content"
          placeholder="Enter a tast to track..."
          v-model="input_content"
        />

        <h4>Pick a category</h4>
        <div class="options">
          <label>
            <input
              type="radio"
              name="category"
              id="category1"
              value="business"
              v-model="input_category"
            />
            <span class="bubble business"></span>
            <div>Business</div>
          </label>

          <label>
            <input
              type="radio"
              name="category"
              id="category2"
              value="personal"
              v-model="input_category"
            />
            <span class="bubble personal"></span>
            <div>Personal</div>
          </label>
        </div>

        <input type="submit" value="Add todo" />
      </form>
    </section>

    <section class="bg-gray-100 py-8">
        <h3 class="text-lg font-bold mb-4">TODO LIST</h3>
        <div class="grid gap-4" id="todo-list">
          <div
            v-for="todo in todos_asc"
            :key="todo.id"
            class="flex flex-col sm:flex-row items-center bg-white rounded-lg shadow-md mb-4 py-2 px-4"
          >
            <label class="flex items-center">
              <input type="checkbox" v-model="todo.done" class="mr-2 text-green-500" />
              <span
                class="h-4 w-4 border border-gray-300 rounded-full mr-4"
                :class="{
                  'bg-blue-500': todo.category === 'business',
                  'bg-pink-500': todo.category === 'personal'
                }"
              ></span>
            </label>
            <div class="flex-grow">
              <input
                type="text"
                v-model="todo.content"
                class="bg-transparent border-b border-gray-300 focus:outline-none mb-2 sm:mb-0 sm:mr-4"
              />
            </div>
            <button @click="removeTodo(todo)" class="text-red-500 cursor-pointer">Delete</button>
          </div>
        </div>
      </section>
        
  </main>
</template>
