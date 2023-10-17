<script setup>
defineProps({
    field: {
        type: Object,
        default: () => {},
        required: true
    },
    submitted: {
        type: Boolean,
        default: false
    }
})
</script>
<template>
    <div v-if="field.$invalid && submitted" style="margin-bottom: 16px;">
        <span v-for="(error, index) of field.$silentErrors" :key="index">
            <span class="p-error">{{ error.$message }}</span>
        </span>
    </div>
</template>
<style lang="scss" scoped>
.p-error{
    color: red;
}
</style>
