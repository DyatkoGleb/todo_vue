<template>
        <my-input v-model.trim="inputValue" placeholder="Search"></my-input>
</template>

<script>
import MyInput from './UI/MyInput.vue';
export default {
    components: { MyInput },
    data() {
        return {
            inputValue: ''
        }
    },
    watch: {
        inputValue() {
            this.$emit('update:modelValue', this.inputValue)
        }
    }
}
</script>

<style scoped>

</style>