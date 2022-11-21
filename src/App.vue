<script setup>
import { ref, onMounted, computed, watch } from 'vue'
const todos = ref([])
const name = ref('')
const input_content = ref('')
const input_category = ref(null)
const todos_asc = computed(() => todos.value.sort((a,b) =>{
	return a.createdAt - b.createdAt
}))
watch(name, (newVal) => {
	localStorage.setItem('name', newVal)
})
watch(todos, (newVal) => {
	localStorage.setItem('todos', JSON.stringify(newVal))
}, {
	deep: true
})
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
				Get Stuff Done!<input type="text">
			</h2>
		</section>

		<section class="create-todo">
			<h3>What to do?</h3>

			<form id="new-todo-form" @submit.prevent="addTodo">
				<input 
					type="text" 
					name="content" 
					id="content" 
					v-model="input_content"/>
				
				<div class="options">
				</div>

				<input type="submit" value="Get it Done!"/>
			</form>
		</section>

		<section class="todo-list">
			<div class="list" id="todo-list">

				<div v-for="todo in todos_asc" :class="`todo-item ${todo.done && 'done'}`">


					<div class="todo-content">
						<input type="text" v-model="todo.content"/>
					</div>

					<div class="actions">
						<button class="delete" @click="removeTodo(todo)">Done</button>
					</div>
				</div>

			</div>
		</section>

	</main>
</template>