<template>
  <div class="container">
    <h1>New contact</h1>
    <div>
      First Name: <input type="text" v-model="newContactFirstName">
      Last Name: <input type="text" v-model="newContactLastName">
      Phone Number: <input type="text" v-model="newContactPhoneNumber">
      <button v-on:click="createcontact()">Create contact</button>
    </div>
    <h1>All contacts</h1>
    <div v-for="contact in contacts">
      <h2>{{ contact }}</h2>
      <img v-bind:src="contacts.url">
      <button v-on:click="showcontacts(contacts)">Show more</button>
      <div v-if="currentContact === contacts">
        <p>First Name: {{ contact.firstName }}</p>
        <p>Last Name: {{ contact.lastName }}</p>
        <p>Phone Number: {{ contact.phoneNumber }}</p>
      </div>
      <p>First Name: {{ contact.firstName }}</p>
      <p>Last Name: {{ contact.lastName }}</p>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      contacts: [],
      currentContact: "",
      newContactFirstName: "",
      newContactLastName: "",
      newContactPhoneNumber: ""
    };
  },
  created: function() {
    axios.get("/api/contacts").then(response => {
      console.log("i am in contact");
      this.contacts = response.data;
    });
  },
  methods: {
    createPhoto: function() {
      var params = {
        firstName: this.newContactFirstName,
        lastName: this.newContactLastName,
        phoneNumber: this.newContactPhoneNumber
      };
      axios.post("/api/contacts", params).then(response => {
        this.contacts.push(response.data);
        this.newContactFirstName = "";
        this.newContactLastName = "";
        this.newContactPhoneNumber = "";
      });
    }
  }
};
</script>