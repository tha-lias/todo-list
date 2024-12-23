<template>
	<div id="app">
		<h1>Tarefas</h1>
		<TaskProgress :progress="progress"/>
		<NewTask @taskAdded="addTask" />
		<TaskGrid :tasks="tasks" @taskDeleted="deleteTask" @taskStateChanged="toggleTaskState" />
	</div>
</template>

<script>
import TaskProgress from './components/TaskProgress.vue';
import NewTask from './components/NewTask.vue';
import TaskGrid from './components/TaskGrid.vue';

export default {
	components: { NewTask, TaskGrid, TaskProgress},
	data() {
		return {
			tasks: [
				// { name: 'Lavar a Louça', pending: false },
				// { name: 'Comprar blusa', pending: true },
			]
		}
	},
	computed: {
		progress(){
			const total = this.tasks.length;
			const done = this.tasks.filter(t => !t.pending).length
			return Math.round(done / total * 100) || 0
		}
	},
	methods: {
		addTask(task) {
			const sameName = t => t.name === task.name;
			const reallyNew = this.tasks.filter(sameName).length == 0

			if (task.name === '') return
			if (reallyNew) {
				this.tasks.push({
					id: Date.now(),
					name: task.name,
					pending: task.pending || true
				})
			}
		},

		deleteTask(taskId) {
			const taskIndex = this.tasks.findIndex(task => task.id === taskId);
			if (taskIndex !== -1) {
				this.tasks.splice(taskIndex, 1);
			}
		},


		toggleTaskState(taskId) {
			const task = this.tasks.find(task => task.id === taskId);
			if (task) {
				task.pending = !task.pending;
			}
		}

	}
}
</script>

<style>
body {
	font-family: 'Lato', sans-serif;
	background: linear-gradient(to right, rgb(22, 34, 42), rgb(58, 96, 115));
	color: #FFF;
}

#app {
	display: flex;
	flex: 1;
	flex-direction: column;
	justify-content: center;
	align-items: center;
	height: 100vh;
}

#app h1 {
	margin-bottom: 5px;
	font-weight: 300;
	font-size: 3rem;
}
</style>
