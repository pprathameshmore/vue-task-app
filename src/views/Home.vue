<template>
    <div v-show="showAddTask">
        <AddTask @add-task="addTask" />
    </div>
    <Tasks @toggle-remainder="toggleRemainder" @delete-task="deleteTask" :tasks="tasks" />
</template>

<script>
import Tasks from "../components/Tasks.vue"
import AddTask from "../components/AddTask.vue"

export default {
    name: 'Home',
    components: {
        Tasks,
        AddTask,
    },
    data() {
        return {
            tasks: []
        }
    },
    props: {
        showAddTask: Boolean,
    },
    methods: {
        async deleteTask(id) {
            if (confirm("Are you sure?")) {
                const res = await fetch(`http://localhost:5000/tasks/${id}`, { method: "DELETE" })
                if (res.status === 200) {
                    this.tasks = this.tasks.filter((task) => task.id !== id);
                } else {
                    alert("Error deleting task")
                }
            }
        },
        async toggleRemainder(id) {
            console.log("Clicked", id)

            const taskToToggle = await this.fetchTask(id);

            const updateTask = {
                ...taskToToggle,
                reminder: !taskToToggle.reminder
            }
            const res = await fetch(`http://localhost:5000/tasks/${id}`, {
                method: "PUT", headers: {
                    "Content-type": "application/json",
                },
                body: JSON.stringify(updateTask)
            })

            const data = await res.json()

            this.tasks = this.tasks.map((task) => task.id === id ? { ...task, reminder: data.reminder } : task)
        },
        async addTask(task) {
            const res = await fetch("http://localhost:5000/tasks", {
                method: "POST", headers: {
                    "Content-type": "application/json",
                },
                body: JSON.stringify(task)
            })
            const data = await res.json();
            this.tasks = [...this.tasks, data];
        },
        toggleAddTask() {
            this.showAddTask = !this.showAddTask
        },
        async fetchTasks() {
            const res = await fetch("http://localhost:5000/tasks")
            const data = await res.json()
            return data
        },
        async fetchTask(id) {
            const res = await fetch(`http://localhost:5000/tasks/${id}`)
            const data = await res.json()
            return data
        },
    },
    async created() {
        this.tasks = await this.fetchTasks();
    }
}
</script>