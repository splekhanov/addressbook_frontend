<template>
    <div id="contacts-table">
        <p v-if="contacts.length < 1" class="empty-table">
            No contacts
        </p>
        <table>
            <thead>
                <tr>
                    <th>Name</th>
                    <th>Email</th>
                    <th>Actions</th>
                </tr>
            </thead>
            <tbody>
                <tr
                    :key="contact.id"
                    v-for="contact in contacts"
                >
                    <td v-if="editing === contact.id">
                        <input
                            type="text"
                            v-model="contact.name"
                        >
                    </td>
                    <td v-else>{{contact.name}}</td>


                    <td v-if="editing === contact.id">
                        <input
                                type="text"
                                v-model="contact.email"
                        >
                    </td>
                    <td v-else>{{contact.email}}</td>
                    <td v-if="editing === contact.id">
                        <button @click="editContact(contact)">Save</button>
                        <button @click="cancelEdit(contact)" class="muted-button">Cancel</button>
                    </td>
                    <td v-else>
                        <button @click="editMode(contact)">Edit</button>
                        <button @click="$emit('delete:contact', contact.id)">Delete</button>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
    export default {
        name: "contacts-table",
        props: {
            contacts: Array,
        },

        data() {
            return {
                editing: null,
            }
        },
        methods: {
            editMode(contact) {
                this.cachedContact = Object.assign({}, contact);
                this.editing = contact.id
            },

            editContact(contact) {
                if (contact.name === '' || contact.email === '') return;
                this.$emit('edit:contact', contact.id, contact);
                this.editing = null
            },

            cancelEdit(contact) {
                Object.assign(contact, this.cachedContact);
                this.editing = null;
            }
        }
    }


</script>

<style scoped>

</style>