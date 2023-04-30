<template>
    <select v-model="selected" class="filter" @change="$emit('filter-change', selected)">
        <option value="all-tasks" class="filter__option">Все задачи</option>
        <option value="checked" class="filter__option">Выполненные</option>
        <option value="unchecked" class="filter__option">Невыполненные</option>
    </select>
</template>

<script>
export default {
    name: "Filter",
    data() {
        return {
            selected: 'all-tasks'
        }
    }
}
</script>