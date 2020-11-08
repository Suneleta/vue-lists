<!--Component of kanban including two lists that communicate with drag and drop-->
<template>
  <div class="container">
    <div class="row">
      <div class="col-12 py-5">
        <h1>{{ listsName }}</h1>
      </div>
    </div>
    <div class="row mb-3">
      <!--START OF::Create contact component-->
      <create-contact 
      @on-form-submit="addContact($event)"
      />
     <!--END OF::Create contact component-->
    </div>
    <div class="row">
         <!--START OF:: Contact card list of saved contacts-->

      <div class="col-12 col-sm-10 col-lg-6">
       <div class="row mb-3">
        <div class="col form-inline">
              <b-form-input 
               type="text"
               v-model.trim="search"
               placeholder="Search first name..."
               @keyup="filteredList"
               />
               <b-button @click="sortLastNames">Sort By Last Name</b-button>
        </div>
    </div>
        <ul class="list-group">
          <draggable
            class="list-group kanban-column"
            :list="contactDataList"
            group="contacts"
          >
          <Contact
            v-for="contactData in filteredList"
            :key="contactData.id"
            :first_name="contactData.first_name"
            :last_name="contactData.last_name"
            :email="contactData.email"
            @on-delete="deleteContact(contactData)"
            @on-edit="editContact(contactData, $event)"
          />
        </draggable>
        </ul>
      </div>
      <!--END OF:: Contact card list of saved contacts-->
      <!--START OF:: Contact card list empty-->
      <div class="col-12 col-sm-10 col-lg-6">
        <div class="row mb-3">
        <div class="col form-inline">
              <b-form-input
               type="text"
               v-model.trim="searchEmpty"
               placeholder="Search first name..."
               @keyup="filteredEmptyList"
               />
               <b-button @click="sortLastNamesEmpty">Sort By Last Name</b-button>
        </div>
    </div>
        <ul class="list-group alert-secondary">
          <draggable
            class="list-group kanban-column"
            :list="emptyList"
            group="contacts"
          >
          <Contact
            v-for="element in filteredEmptyList"
            :key="element.name"
             :first_name="element.first_name"
            :last_name="element.last_name"
            :email="element.email"
            @on-delete="deleteElement(element)"
            @on-edit="editContact(element, $event)"
          />
        </draggable>
        </ul>
      </div>
     <!--END OF:: Contact card list empty-->
    </div>
  </div>
</template>

<script>
import draggable from "vuedraggable";
import Contact from "./Contact.vue";
import CreateContact from "./CreateContact.vue";
export default {
  name: "kanban",
  props: {
    listsName: String,
  },
  data() {
    return {
        contactDataList: [],
        emptyList: [],
        search: "",
        searchEmpty:""
    };
  },
  created(){
     fetch("contacts.json")
        .then(response => response.json())
        .then(data => (this.contactDataList = data));
  },
  methods: {
    sortLastNames: function() {
      function compare(a, b) {
        if (a.last_name < b.last_name)
          return -1;
        if (a.last_name > b.last_name)
          return 1;
        return 0;
      }
      return this.contactDataList.sort(compare);
    },
    sortLastNamesEmpty: function() {
      function compare(a, b) {
        if (a.last_name < b.last_name)
          return -1;
        if (a.last_name > b.last_name)
          return 1;
        return 0;
      }
      return this.emptyList.sort(compare);
    },
    addContact(newName, newLastName, newEmail) {
      this.contactDataList.unshift({ first_name: newName, last_name: newLastName, email: newEmail  });
    },
    deleteContact(deletedContact) {
      this.contactDataList = this.contactDataList.filter(contactData => contactData !== deletedContact);
    },
    deleteElement(deletedElement) {
      this.emptyList = this.emptyList.filter(element => element !== deletedElement);
    },
    editContact(contactData, newContactData) {
      contactData.email = newContactData; //Only email is editable
    },
   
  },
  computed: {
    filteredEmptyList: function(){
      return this.emptyList.filter(element => {
        return element.first_name.match(this.searchEmpty);
     })
    },
    filteredList: function() {
    return this.contactDataList.filter(contactData => {
     return contactData.first_name.match(this.search);
     })
    },

  },
  components: { Contact, CreateContact, draggable },
};
</script>
<style>
/* light stylings for the kanban columns */
.kanban-column {
  min-height: 300px;
}
</style>