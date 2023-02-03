<template>
    <select 
        class="select"
        :value="modelValue"   
        @change="changeOption" 
    >
        <option disable value="">Choose filter</option>
        <option 
            v-for="option in options"
            :key="option.value"
            :value="option.value"
        >
            {{ option.name }}
        </option>
    </select>
</template>

<script>
export default {
    props: {
        modelValue: {
            type: String
        },
        options: {
            type: Array,
        }
    },
    methods: {
        changeOption(event) {
            this.$emit('update:modelValue', event.target.value)
        }
    }
}
</script>

<style scoped>
.select {
    margin-left: 10px;
    padding: 3px;
    background: #181818;
    border: 1px solid #282828;
}
.select {
    background: #181818;
}
</style>