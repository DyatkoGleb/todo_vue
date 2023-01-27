<template>
    <div class="todo-item">
        <div v-if="!creating" class="todo-item__btn-add" @click="creatingNewTodo">
            <svg width="101px" height="101px" viewBox="0 0 16 16" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
                <rect width="16" height="16" id="icon-bound" fill="none" />
                <polygon points="14,7 9,7 9,2 7,2 7,7 2,7 2,9 7,9 7,14 9,14 9,9 14,9" />
            </svg>
        </div>

        <my-form-wrapper v-else>
            <create-edit-todo-form :titleError="titleError" :descriptionError="descriptionError" @newTodo="createTodo"/>
        </my-form-wrapper>
    </div>
</template>

<script>
import MyFormWrapper from '@/components/UI/MyFormWrapper'
import CreateEditTodoForm from '@/components/CreateEditTodoForm'

export default {
    data() {
        return {
            creating: false,
            title: '',
            description: '',
            titleError: false,
            descriptionError: false
        }
    },
    components: { MyFormWrapper, CreateEditTodoForm },
    methods: {
        creatingNewTodo() {
            this.creating = true
        },
        createTodo(data) {
            let newTodo = { 
                title: data.titleValue, 
                description: data.textareaValue
            }
            

            if (!newTodo.title) { 
                this.titleError = true
                return
            }
            this.titleError = false

            if (!newTodo.description) { 
                this.descriptionError = true
                return
            }
            this.descriptionError = false


            this.$emit('createTodo', newTodo)


            this.title = ''
            this.description = ''
            this.creating = false
        }
    }
}
</script>

<style scoped>
.todo-item {
    width: 100%;
    margin: 10px 0;
    border-radius: 12px;
    transition: .1s;
    cursor: pointer;
}
.todo-item__btn-add {
    width: 100%;
    text-align: center;
    transition: .1s;
    border: 1px solid #282828;
    border-radius: 12px;
    fill: #282828;
}
.todo-item__btn-add:hover {
    fill: #333;
    border: 1px solid #333;
}
</style>