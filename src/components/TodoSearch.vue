<template>
        <my-input 
            v-model.trim="inputValue" 
            placeholder="Search"
            class="input-search"
        />
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
.input-search {
    font-size: 18px;
}
</style>