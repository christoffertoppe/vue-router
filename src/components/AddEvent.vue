<template>
    <div id="AddEvent">
    <form class="homeText" @submit.prevent="handleSubmit">
        Tallenna onnettomuus
        <input id="tekstikentta" v-model="notes.content" placeholder="tapahtuma">
        <p v-if="error && submitting" class="error-message">Tekstikenttä on tyhjä</p>
        <p v-if="success" class="success_message"> Tapahtuma lisätty</p>
        <button id="save">TALLENNA</button>
    </form>
    </div>
</template>


<script>


    import axios from 'axios'
    export default {
      name: 'AddEvent',
      data() {
        return {
          notes: {
            id: '',
            content: '',
            date: '',
          },
          submitting: false,
          error: false,
          success:false,
          content: Array,
        };
      },
      computed: {
        emptyField() {
          return this.notes.content === ''
        }
      },
      methods: {

        handleSubmit() {
          this.submitting = true;
          this.clearStatus();
          if(this.emptyField) {
            this.error = true;
          } else {

            let date = new Date();
            this.notes.date = date.toLocaleString('fi-FI', {timeZone: 'Europe/Helsinki'});
            this.$emit('add:event', this.notes.content);
            this.addNote(this.notes);
            this.success = true
          }
        },

        clearStatus() {
          this.success = false;
          this.error = false
        },

        async addNote(note) {
          try {
            const response = await fetch('http://localhost:3001/notes', {
              method: 'POST',
              body: JSON.stringify(note),
              headers: {'Content-type': 'application/json; charset=UTF-8' },
            });
            const data = await response.json();

            this.notes = [...this.notes, data]
          } catch (error) {
            console.log(error)
          }
        },

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
    #save {
        margin-top: 20px;
    }

    #tekstikentta {
        width: 50%;
        margin-left: 25%;
        margin-right: 25%;
        justify-content: center;
    }
</style>