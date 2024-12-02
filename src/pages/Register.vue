<template>
    <div class="d-flex align-items-center justify-content-center vh-100 bg-light">
        <div class="card shadow p-4" style="width: 350px;">
            <h5 class="text-end text-primary fw-bold" style="cursor: pointer; font-size: 0.8rem" @click="this.$router.push('/')">Go back</h5>
            <h2 class="text-center mb-4">Register</h2>
            <form @submit.prevent="register">
                <!-- Username Input -->
                <div class="form-group mb-3">
                    <label for="username" class="form-label">Username</label>
                    <input type="text" id="username" class="form-control" v-model="username"
                        placeholder="Enter your username" />
                    <div v-if="errors.username" class="text-danger mt-1">{{ errors.username }}</div>
                </div>
                <!-- Password Input -->
                <div class="form-group mb-3">
                    <label for="password" class="form-label">Password</label>
                    <input type="password" id="password" class="form-control" v-model="password"
                        placeholder="Enter your password" />
                    <div v-if="errors.password" class="text-danger mt-1">{{ errors.password }}</div>
                </div>
                <!-- Email Input -->
                <div class="form-group mb-3">
                    <label for="email" class="form-label">Email</label>
                    <input type="email" id="email" class="form-control" v-model="email" placeholder="Enter your email" />
                    <div v-if="errors.email" class="text-danger mt-1">{{ errors.email }}</div>
                </div>
                <!-- Error Message -->
                <div v-if="error" class="alert alert-danger text-center mb-3">
                    {{ error }}
                </div>
                <!-- Submit Button -->
                <button type="submit" class="btn btn-primary w-100">Register</button>
            </form>
        </div>
    </div>
</template>

<script>
import axios from "axios";
import { API_URL } from "../../config";

export default {
    name: "Register",
    components: {

    },
    data() {
        return {
            username: "",
            password: "",
            email: "",
            error: null,
            errors: {}, // For form-specific validation messages
        };
    },
    methods: {
        validateForm() {
            const errors = {};

            // Username validation
            if (!this.username.trim()) {
                errors.username = "Username cannot be empty.";
            }

            // Password validation
            if (!this.password || this.password.length <= 5) {
                errors.password = "Password must be longer than 5 characters.";
            }

            // Email validation
            const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
            if (!emailRegex.test(this.email)) {
                errors.email = "Please enter a valid email address.";
            }

            this.errors = errors;
            return Object.keys(errors).length === 0;
        },
        async register() {
            if (!this.validateForm()) {
                return;
            }

            try {
                const response = await axios.post(`${API_URL}/register`, {
                    username: this.username,
                    password: this.password,
                    email: this.email,
                });
                if (response.status === 201) {
                    const message = response.data.msg;
                    window.alert(`${message}, you can log in now.`);
                    this.$router.push("login");
                }
            } catch (error) {
                this.error = error.response?.data?.error || error.response?.data?.msg || "An error occurred. Please try again." 
            }
        },
    },
};
</script>

<style scoped>
.card {
    border-radius: 10px;
}

.alert {
    font-size: 0.9rem;
}
</style>
