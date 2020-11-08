<template>
  <li class="d-flex align-items-center list-group-item">
    <button
      class="btn border-0 flex-grow-1 text-left shadow-none"
      v-if="!isEditing"
    >
       <span>{{first_name}}</span>
       <span class="p-2">{{last_name}}</span>
       <span>{{email}}</span>
    </button>
    <form v-else class="flex-grow-1" @submit.prevent="finishEditing()">
      <input
        type="text"
        class="form-control"
        v-model="newContact"
        @blur="finishEditing()"
        ref="newContact"
      />
    </form>
    <button
      @click="startEditing()"
      class="btn btn-outline-primary border-0 ml-2"
    >
      <span class="fa fa-edit"></span>
    </button>
    <button @click="$emit('on-delete')" class="btn btn-outline-danger border-0">
      <span class="fa fa-trash"></span>
    </button>
  </li>
</template>

<script>
export default {
    name: "Contact",
  data() {
    return {
      isEditing: false,
      newContactName: "",
      newContactLastName: ""
    };
  },
  props: {
    first_name: String,
    last_name: String,
    email: String
  },
  methods: {
    startEditing() {
      if (this.isEditing) {
        this.finishEditing();
      } else {
        this.newContact = this.email;
        this.isEditing = true;
        this.$nextTick(() => this.$refs.newContact.focus());
      }
    },
    finishEditing() {
      this.isEditing = false;
      this.$emit("on-edit", this.newContact);
    }
  }
};
</script>
