<template>
    <header class="header blur">
        <h1><a href="#top">{{ title }}</a></h1>
        <Button 
        @btn-click="$emit('toggle-add-task')" 
        :text='showAddTask ? "Отмена" : "Добавить" ' 
        :color='showAddTask ? "" : "green" '
        />
    </header>
</template>

<script>
import Button from './Button';

export default {
    name: 'Header',
    props: {
        title: String,
        showAddTask: Boolean
    },
    components: {
        Button
    }
}
</script>