<template>
  <form class="col-12 container" @submit.prevent="addContact()">
    <input
      v-model="newName"
      type="text"
      class="col-6 form-control"
      placeholder="Write full name and email"
      required
    />
    <b-button type="submit" variant="primary" class="ml-3 col-6" @submit.prevent="addContact()">Add contact</b-button>
  </form>
</template>

<script>
export default {
  name: "CreateContact",
  data() {
    return {
      newName: "",
    };
  },
  methods: {
    addContact() {
      if (this.newName.length > 0) {
        this.$emit("on-form-submit", this.newName);
      }
      this.newName = "";
    }
  }
};
</script>
<style scoped>
.container{
  display: flex;
}
</style>
