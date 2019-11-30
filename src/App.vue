<template>
  <div id="app" class="medium-container">
    <h1>Contacts</h1>

    <contacts-form @add:contact="addContact"/>
    <contacts-table
            :contacts="contacts"
            @delete:contact="deleteContact"
            @edit:contact="editContact"
    />
  </div>
</template>

<script>
  import ContactsTable from '@/components/ContactsTable.vue'
  import ContactsForm from "./components/ContactsForm";

  export default {
    name: 'app',
    components: {
      ContactsTable,
      ContactsForm,
    },
    data() {
      return {
        contacts: [
          {
            id: 1,
            name: 'Richard Hendricks',
            email: 'richard@piedpiper.com',
          },
          {
            id: 2,
            name: 'Bertram Gilfoyle',
            email: 'gilfoyle@piedpiper.com',
          },
          {
            id: 3,
            name: 'Dinesh Chugtai',
            email: 'dinesh@piedpiper.com',
          },
        ],
      }
    },

    methods: {

      addContact(contact) {
        const lastId =
                this.contacts.length > 0
                        ? this.contacts[this.contacts.length - 1].id
                        : 0;
        const id = lastId + 1;
        const newContact = { ...contact, id };
        this.contacts = [...this.contacts, newContact]
      },

      deleteContact(id) {
        this.contacts = this.contacts.filter(
                contact => contact.id !== id
        )
      },

      editContact(id, updatedContact) {
        this.contacts = this.contacts.map(contact =>
                contact.id === id ? updatedContact : contact
        )
      }
    }
  }
</script>

<style>
  button {
    background: #009435;
    border: 1px solid #009435;
    margin-right: 10px;
  }

  .small-container {
    max-width: 680px;
  }
</style>
