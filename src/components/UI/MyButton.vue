<template>
    <div class="btn-wrapper">
        <button class="btn">
            <slot></slot>
        </button>
    </div>
</template>

<script>
export default {

}
</script>

<style scoped>
.btn-wrapper {
    display: flex;
    align-items: end;
}
.btn {
    height: max-content;
    margin-bottom: 11px;
    background: none;
    border: none;
    color: #777;
    white-space: nowrap;
    cursor: pointer;
}
.btn:hover, .btn:focus {
    margin-bottom: 10px;
    color: #eee;
    border-bottom: 1px solid #eee;
}
</style>