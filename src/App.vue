<script setup>
import { toTypedSchema } from "@vee-validate/yup";
import { useForm } from "vee-validate";
import { number, object, ref, string, boolean } from "yup";
import BaseInput from "./components/BaseInput.vue";

const config = (state) => ({
    validateOnModelUpdate: state.errors.length > 0,
});

const schema = toTypedSchema(
    object({
        fullName: string()
            .required("Name is required")
            .min(2, "Name must contain at least 2 characters"),

        email: string()
            .required("Email is required")
            .email("Enter a valid email address"),

        confirmEmail: string()
            .required("Confirm your email")
            .oneOf([ref("email")], "Email must match"),

        age: number()
            .transform((value, originalValue) =>
                originalValue === "" ? undefined : value,
            )
            .typeError("Age must be a number")
            .required("Age is required")
            .min(16, "You must be at least 16"),

        password: string()
            .required("Password is required")
            .min(8, "Minum 8 characters")
            .matches(/[A-Z]/, "Include an uppercase letter")
            .matches(/[a-z]/, "Include a lowercase letter")
            .matches(/[0-9]/, "Include a number"),

        confirmPassword: string()
            .required("Confirm your password")
            .oneOf([ref("password")], "Passwords must match"),

        terms: boolean()
            .oneOf([true], "You must accept the terms")
            .required("You must accept the terms"),

        address: object({
            city: string().required("City is required"),

            country: string().required("Country is required"),
        }),

        hasAccommodation: boolean().default(false),

        accommodationDetails: string().when("hasAccommodation", {
            is: true,
            then: (schema) =>
                schema.required("Please provide accommodation details"),
            otherwise: (shcema) => shcema.notRequired(),
        }),
    }),
);

const {
    defineField,
    errors,
    values,
    meta,
    resetForm,
    handleSubmit,
    isSubmitting,
} = useForm({
    validationSchema: schema,
});

const [fullName, fullNameAttrs] = defineField("fullName", config);
const [email, emailAttrs] = defineField("email", config);
const [password, passwordAttrs] = defineField("password", config);
const [confirmPassword, confirmPasswordAttrs] = defineField(
    "confirmPassword",
    config,
);
const [age, ageAttrs] = defineField("age", config);
const [terms, termsAttrs] = defineField("terms", config);
const [city, cityAttrs] = defineField("address.city", config);
const [country, countryAttrs] = defineField("address.country", config);
const [confirmEmail, confirmEmailAttrs] = defineField("confirmEmail", config);
const [hasAccommodation, hasAccommodationAttrs] = defineField(
    "hasAccommodation",
    config,
);
const [accommodationDetails, accommodationDetailsAttrs] = defineField(
    "accommodationDetails",
    config,
);

const onSubmit = handleSubmit(async (formValues) => {
    console.log("Valid from:", formValues);
    await new Promise((resolve) => setTimeout(resolve, 1000));

    alert("Registration successful!");
});
</script>

<template>
    <main>
        <h1>Student Registration</h1>
        <form @submit="onSubmit">
            <BaseInput
                v-model="fullName"
                v-bind="fullNameAttrs"
                label="Full Name"
                type="text"
                name="fullName"
                :error="errors.fullName"
            ></BaseInput>
            <BaseInput
                v-model="email"
                v-bind="emailAttrs"
                label="Email"
                type="email"
                name="email"
                :error="errors.email"
            ></BaseInput>
            <BaseInput
                v-model="confirmEmail"
                v-bind="confirmEmailAttrs"
                label="Confirm Email"
                type="email"
                name="confirmEmail"
                :error="errors.confirmEmail"
            ></BaseInput>
            <BaseInput
                v-model="age"
                v-bind="ageAttrs"
                label="Age"
                type="number"
                name="age"
                :error="errors.age"
            ></BaseInput>
            <BaseInput
                v-model="password"
                v-bind="passwordAttrs"
                label="Password"
                type="password"
                name="password"
                :error="errors.password"
            ></BaseInput>
            <BaseInput
                v-model="confirmPassword"
                v-bind="confirmPasswordAttrs"
                label="Confirm Password"
                type="password"
                name="confirmPassword"
                :error="errors.confirmPassword"
            ></BaseInput>
            <BaseInput
                v-model="city"
                v-bind="cityAttrs"
                label="City"
                type="text"
                :error="errors['address.city']"
            >
            </BaseInput>
            <BaseInput
                v-model="country"
                v-bind="countryAttrs"
                label="Country"
                type="text"
                :error="errors['address.country']"
            >
            </BaseInput>
            <BaseInput
                v-model="terms"
                v-bind="termsAttrs"
                label="Terms"
                type="checkbox"
                name="terms"
                :error="errors.terms"
            ></BaseInput>
            <label>
                <input
                    type="checkbox"
                    v-model="hasAccommodation"
                    v-bind="hasAccommodationAttrs"
                />
                I need accommodation
            </label>
            <BaseInput
                v-if="hasAccommodation"
                v-model="accommodationDetails"
                v-bind="accommodationDetailsAttrs"
                label="Accommodation Details"
                :error="errors.accommodationDetails"
            ></BaseInput>

            <br /><br />
            <button type="submit" :disabled="isSubmitting">
                {{ isSubmitting ? "Submitting..." : "Register" }}
            </button>
            <button type="button" @click="resetForm()">Reset</button>
        </form>

        <h3>Current values</h3>
        <pre>{{ JSON.stringify(values, null, 2) }}</pre>

        <h3>Form State</h3>
        <pre>{{
            JSON.stringify(
                {
                    valid: meta.valid,
                    dirty: meta.dirty,
                    touched: meta.touched,
                },
                null,
                2,
            )
        }}</pre>
    </main>
</template>

<style scoped>
form {
    background: #dbd7d7aa;
    padding: 25px;
}
</style>
