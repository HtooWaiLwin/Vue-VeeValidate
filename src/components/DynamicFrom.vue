<script setup>
import { Form, Field, ErrorMessage } from 'vee-validate'
const props = defineProps({
    schema: {
        type: Object, 
        required: true
    }
})
</script>

<template>
    <Form>
        <div v-for="{as, name, label, ...attrs} in schema.fields" :key="name">
            <label :for="name">{{ label }}</label>
            <Field :as="as" :name="name" :id="name" v-bind="attrs"></Field>
            <ErrorMessage :name="name"></ErrorMessage>
        </div>
    </Form>
</template>