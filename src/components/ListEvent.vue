<template>
  <p class="homeText">
    Tapahtumat

    <b-table :items="this.content"></b-table>

  </p>
</template>
<script>
  import axios from 'axios'
  export default {
  name: 'ListEvent',
  data() {
    return {
      content: Array,
    };
  },

    methods: {

      async deleteNote(id) {
        try {
          await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
            method: "DELETE"
          });
          this.notes = this.notes.filter(note => note.id !== id);
        } catch (error) {
          console.error(error);
        }
    },
      async deleteAll()
      {
        try {
          await fetch(`https://jsonplaceholder.typicode.com/users/`, {
            method: "DELETE"
          });
          this.notes = this.notes.filter();
        } catch (error) {
          console.error(error);
        }
      }
    },
  mounted() {
    axios
    .get('http://localhost:3001/notes')
    .then(response => {
      this.content = response.data;
      console.log(this.content)
    })
  }
};

</script>
<style scoped>
.homeText{
  font-size: 35px;
  color: red;
  text-align: center;
  position: relative;
  top:30px;
  text-shadow: 2px 2px 2px gray;
}
</style>