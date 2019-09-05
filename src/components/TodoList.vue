<template>
	<section class="main" v-show="todos.length" v-cloak>
		<input class="toggle-all" type="checkbox">
		<ul class="todo-list">
			<li v-for="todo in filteredTodos"
				class="todo"
				:key="todo.id"
				:class="{completed: todo.completed}">
				<todo-item
					:todo="todo"
					@remove-todo="removeTodo"
					@done="done">
				</todo-item>
			</li>
		</ul>
	</section>
</template>

<script>
import TodoItem from './TodoItem.vue';
export default {
	name: 'TodoList',
	components: {
		TodoItem
	},
	props: {
		todos: Array,
		filteredTodos: Array
	},
	methods: {
		removeTodo(todo) {
			this.$emit('remove-todo', todo);
		},
		done(todo, completed) {
			this.$emit('done', todo, completed);
		}
	}
}
</script>

<style scoped>
[v-cloak] {
	display: none;
}
</style>
