<template>
    <div class='task blur'>

        <div @click="$emit('toggle-done', task.id)" :class="[task.done ? 'done' : '', 'task__check' ]"></div>
        <h3 class="task__text">
            {{task.text}}           
        </h3>   
        <div class="task__icons">
            <i @click="$emit('update-task', task.id)" class="fas fa-pen"></i>
            <i @click="$emit('delete-task', task.id)" class="fas fa-circle-xmark"></i>
        </div> 
    </div>
</template>

<script>
export default {
    name: 'Task',
    props: {
        task: Object
    }
}
</script>