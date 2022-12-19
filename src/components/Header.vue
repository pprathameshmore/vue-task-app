<template>
    <header>
        <h1>{{ title }}</h1>
        <Button @btn-click="$emit('toggle-add-task')" :color="showAddTask ? 'red' : 'green'"
            :text="showAddTask ? 'Close' : 'Add Task'" />
    </header>
</template>


<script>
import Button from "./Button.vue"

export default {
    name: "Header",
    props: ["title", "showAddTask"],
    components: {
        Button
    }
}

</script>

<style scoped>
header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
}
</style>