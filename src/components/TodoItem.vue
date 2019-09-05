<template>
	<div class="view">
		<input
			type="checkbox" class="toggle"
			:value="todo.completed"
			@input="onInput">
		<label>{{todo.title}}</label>
		<button
			class="destroy"
			@click="removeTodo">
		</button>
	</div>
</template>

<script>
export default {
	name: 'TodoItem',
	props: {
		todo: Object
	},
	methods: {
		removeTodo() {
			this.$emit('remove-todo', this.todo);
		},
		onInput() {
			this.$emit('done', this.todo, !this.todo.completed);
		}
	}
}
</script>
