<script setup>
import { useField } from "vee-validate";

defineOptions({
    inheritAttrs: false,
});

const props = defineProps({
    name: {
        type: String,
        required: true,
    },
    label: {
        type: String,
        required: true,
    },
    type: {
        type: String,
        default: text,
    },
});

const { value, errorMessage, handleBlur } = useField(() => props.name);
</script>

<template>
    <div class="form-field">
        <label>{{ label }}</label>
        <input
            v-bind="$attrs"
            :type="type"
            v-model="value"
            @blur="handleBlur"
        />
        <p v-if="errorMessage" class="error">
            {{ errorMessage }}
        </p>
    </div>
</template>
