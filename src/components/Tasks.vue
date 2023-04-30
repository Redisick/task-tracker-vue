<template>
    <div class="tasks">
        <Task :key="task.id" v-for="task in tasks"
        @update-task="$emit('update-task', task.id)"
        @toggle-done="$emit('toggle-done', task.id)"
        @delete-task="$emit('delete-task', task.id)" :task="task"/>
    </div>
</template>

<script>
import Task from "./Task";

export default {
    name: 'Tasks',
    props: {
        tasks: Array
    },
    components: {
        Task,
    },
    emits: ['update-task', 'delete-task', 'toggle-done']
}
</script>