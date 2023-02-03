<template>
    <div class="form">
        <div class="todo-item__inputs">
            <my-input 
                class="todo-item__input-title"
                placeholder="Title"
                v-focus
                v-model.trim="inputValue" 
                :inputValue="title" 
                :inputError="titleError" 
            ></my-input>
            <my-textarea 
                placeholder="Description"
                v-model.trim="textareaValue" 
                :textareaValue="description" 
                :descriptionError="descriptionError"
            ></my-textarea>
        </div>

        <my-button @click="newTodo">Add</my-button>
    </div>
</template>

<script>
import MyInput from '@/components/UI/MyInput'
import MyTextarea from '@/components/UI/MyTextarea'
import MyButton from '@/components/UI/MyButton'

export default {
    components: { MyInput, MyTextarea, MyButton },
    props: { 
        title: { type: String }, 
        description: { type: String },
        titleError: { type: Boolean },
        descriptionError: { type: Boolean },
    },
    data() {
        return {
            inputValue: this.title,
            textareaValue: this.description
        }
    },
    methods: {
        newTodo() {
            const newTodo = {
                titleValue: this.inputValue,
                textareaValue: this.textareaValue
            }

            this.$emit('newTodo', newTodo)
        }
    }
}
</script>

<style scoped>
.form {
    display: flex;
    justify-content: space-between;
}
.todo-item__inputs {
    width: 100%;
    margin-top: 2px;
    margin-right: 20px;
}
.todo-item__input-title {
    margin-bottom: 13px;
}
</style>