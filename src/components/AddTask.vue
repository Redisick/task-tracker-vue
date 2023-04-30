<template>
    <div class="form--cover">
        <form @submit="onSubmit" class="form">
        <div class="form__control">
            <label>Задача</label>
            <textarea type="text" v-model="text" name="text" placeholder="..." class="input--text"></textarea>
        </div>
        <input type="submit" value="Сохранить" class="btn green" />       
        </form>
    </div>  
</template>

<script>

export default {
    name: 'AddTask',
    props: {
        task: Object,
    },
    data() {
        return {
            id: (this.task ? this.task.id : ""),
            text: (this.task ? this.task.text : "") ,
            done: false
        }
    },
    methods: {
        onSubmit(e) {
            e.preventDefault();

            if (!this.text) {
                alert('Пожалуйста, добавьте название');
                return;
            }

            const newTask = {
                text: this.text,
                done: this.done
            }

            if (this.task.id) {
                this.$emit('add-task', newTask, this.task.id);
            }
            else {
                this.$emit('add-task', newTask);
            }      
            this.text = '';
        }
    }
}
</script>