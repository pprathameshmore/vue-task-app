<template>
    <div :key="task.id" v-for="task in tasks">
        <Task @toggle-remainder="$emit('toggle-remainder', task.id)" @delete-task="$emit('delete-task', task.id)" :task="task" />
    </div>
</template>

<script>
import Task from "./Task.vue"

export default {
    name: "Tasks",
    props: {
        tasks: Array
    },
    components: {
        Task
    },
    emits: ["delete-task", "toggle-remainder"]
}

</script>