<template>
	<section class="main" v-show="todos.length" v-cloak>
		<input id="toggle-all" class="toggle-all" type="checkbox"
			:value="allDone"
			:checked="allDone"
			@change="onInput">
		<label for="toggle-all"></label>
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
		filteredTodos: Array,
		allDone: Boolean
	},
	methods: {
		removeTodo(todo) {
			this.$emit('remove-todo', todo);
		},
		done(todo, completed) {
			this.$emit('done', todo, completed);
		},
		onInput() {
			this.$emit('allDone', !this.allDone);
		}
	}
}
</script>

<style scoped>
[v-cloak] {
	display: none;
}
</style>
