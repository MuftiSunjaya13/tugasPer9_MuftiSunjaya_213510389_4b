<script setup>
import { ref, onMounted, computed, watch } from 'vue'
const todos = ref([])
const input_content = ref('')
watch(todos, (newVal) => {
	localStorage.setItem('todos', JSON.stringify(newVal))
}, {
	deep: true
})
const addTodo = () => {
	if (input_content.value.trim() === '' ) {
		return
	}
	todos.value.push({
		content: input_content.value,
		done: false,
		editable: false,
	})
}
const removeTodo = (todo) => {
	todos.value = todos.value.filter((t) => t !== todo)
}
onMounted(() => {
	todos.value = JSON.parse(localStorage.getItem('todos')) || []
})

const filter = ref('all')

const filtered_todos = computed(() => {
  if (filter.value === 'uncompleted') {
    return todos.value.filter(todo => !todo.done)
  } else {
    return todos.value
  }
})
</script>

<template>
	<main class="app">
		<section class="create-todo">
			<h3>BUAT KEGIATAN</h3>

			<form id="new-todo-form" @submit.prevent="addTodo">
				<h4>Kegiatan Apa yang Ingin Anda Lakukan?</h4>
				<input 
					type="text" 
					name="content" 
					id="content" 
					placeholder="contoh: exploring genshin"
					v-model="input_content" />
				<input type="submit" value="Tambah Kegiatan" />
			</form>
		</section>

		<section class="todo-list">
			<h3>LIST KEGIATAN</h3>
			
      <div class="filters">
        <label>
          <button :class="{active: filter === 'all'}" @click="filter = 'all'">Semua</button>
        </label>
        <label>
          <button :class="{active: filter === 'uncompleted'}" @click="filter = 'uncompleted'">Belum Selesai</button>
        </label>
      </div>
      <div class="list" id="todo-list">
        <div v-for="todo in filtered_todos" :class="`todo-item ${todo.done && 'done'}`">
					<label>
						<input type="checkbox" v-model="todo.done" />
					</label>

					<div class="todo-content">
						<input type="text" v-model="todo.content" />
					</div>

					<div class="actions">
						<button class="delete" @click="removeTodo(todo)">Hapus</button>
					</div>
				</div>
      </div>
	</section>

	</main>
</template>

<style>
body {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  font-family: Arial, sans-serif;
}

.app {
  width: 100%;
  max-width: 800px;
  background-color: rgb(0,0,0,0.35);
  padding: 80px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
}

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

input[type="text"],
input[type="submit"],
button {
  display: block;
  width: 100%;
  max-width: 100%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 8px;
  background-color: #dadfe4;
  font-size: 16px;
  line-height: 1.5;
  margin-bottom: 16px;
  cursor: pointer;
}

h2.title {
  margin-bottom: 16px;
}

h3 {
  margin-bottom: 16px;
  text-align: center;
  color: black;
}

h4 {
  margin-bottom: 8px;
  color: black;
}

.todo-list .list {
  display: flex;
  flex-direction: column;
}

.todo-list .todo-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding-top: 16px;
  border-bottom: 1px solid #ccc;
}

.todo-list .todo-item:last-child {
  border-bottom: none;
}

.todo-list .todo-item .todo-content {
  flex: 1;
  margin-left: 8px;
}

.todo-list .todo-item input[type="checkbox"] {
  margin-right: 8px;
}

.todo-list .todo-item.done .todo-content input[type="text"] {
  text-decoration: line-through;
  color: #aaa;
}

.filters {
  display: flex;
  margin-bottom: 16px;
}

.filters label {
  margin-right: 8px;
}

.filters button {
  border: none;
  background-color: transparent;
  cursor: pointer;
  font-size: 16px;
  line-height: 1.5;
  padding: 8px;
  border-radius: 4px;
  color: #130174;
  outline: none;
}

.filters button.active {
  background-color: rgba(0, 0, 255, 0.5);
  color: #fff;
}

.todo-item .actions .delete {
	background-color: rgba(255, 0, 0, 0.5);
}
</style>