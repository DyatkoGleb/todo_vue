<template>
    <div class="form-wrapper">
        <slot></slot>
    </div> 
</template>

<script>
export default {

}
</script>

<style scoped>
.form-wrapper{
    align-items: flex-end;
    width: 100%;
    padding: 20px;
    border: 1px solid #282828;
    border-radius: 12px;
    cursor: pointer;
}
</style>