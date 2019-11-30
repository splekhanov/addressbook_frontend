<template>
    <div id="contacts-form">
        <form @submit.prevent="handleSubmit">
            <label>Name</label>
            <input
                   ref="first"
                   type="text"
                   id="contactName"
                   :class="{ 'has-error': submitting && invalidName }"
                   v-model="contact.name"
                   @focus="clearStatus"
                   @keypress="clearStatus"
            />
            <label>Email</label>
            <input
                    type="text"
                    id="contactEmail"
                    :class="{ 'has-error': submitting && invalidEmail }"
                    v-model="contact.email"
                    @focus="clearStatus"
            />
            <p v-if="error && submitting" class="error-message">
                ❗Please fill out all required fields
            </p>
            <p v-if="success" class="success-message">
                ✅ Contact successfully added
            </p>
            <button>Add Contact</button>
        </form>
    </div>
</template>

<script>
    export default {
        name: 'contacts-form',
        data() {
            return {
                submitting: false,
                error: false,
                success: false,
                contact: {
                    name: '',
                    email: '',
                },
            }
        },

        methods: {
            handleSubmit(){
                this.submitting = true;
                this.clearStatus();

                if (this.invalidName || this.invalidEmail) {
                    this.error = true;
                    return
                }

                if(this.contact.name !== '' && this.contact.email !== ''){
                    this.$emit('add:contact', this.contact);
                    this.$refs.first.focus();
                }

                this.error = false;
                this.success = true;
                this.clearForm();
            },

            clearForm(){
                this.contact.name = '';
                this.contact.email = '';
                this.submitting = false;
            },

            clearStatus() {
                this.success = false;
                this.error = false;
            }
        },

        computed: {
            invalidName() {
                return this.contact.name === '';
            },

            invalidEmail() {
                return this.contact.email === ''
            },
        },
    }
</script>

<style scoped>
    form {
        margin-bottom: 2rem;
    }

    [class*='-message'] {
        font-weight: 500;
    }

    .error-message {
        color: #d33c40;
    }

    .success-message {
        color: #32a95d;
    }
</style>