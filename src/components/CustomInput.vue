<script setup>
    const email = defineModel()

    defineProps(['error'])
</script>

<template>
    <input v-model="email" type="text" v-bind="$attrs">
    <span v-if="error">{{ error }}</span>
</template>