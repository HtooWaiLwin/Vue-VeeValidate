<script setup>
defineOptions({
    inheritAttrs: false,
});

defineProps({
    label: {
        type: String,
        requierd: true,
    },

    error: {
        type: String,
        default: "",
    },
});

const model = defineModel();
</script>

<template>
    <div class="form-field">
        <label>{{ label }}</label>
        <input v-bind="$attrs" v-model="model" />
        <span v-if="error" class="error"> {{ error }} </span>
    </div>
</template>

<style scoped>
.form-field label {
    display: inline-block;
    width: 30%;
}

.form-field + .form-field {
    margin: 10px 0;
}

.form-field span {
    padding-left: 10px;
    color: red;
}
</style>
