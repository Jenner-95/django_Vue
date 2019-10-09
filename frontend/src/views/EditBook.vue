<template>
 <div class="dashboard">
   <h1 class="subheading grey--text">Editar Libro</h1>   
   <v-container class="my-5">

     <v-card flat class="pa-3">
       <v-row align="center" justify="center">
         <v-col cols="8" md="8">
            <form @submit="onSubmit" id="edit-form">
                <v-text-field outlined v-model.trim="form.title" :counter="50" label="Titulo" required></v-text-field>
                <v-textarea outlined v-model.trim="form.description" :counter="500" label="Descripcion" textarea rows="10" auto-grow required></v-textarea>
            
                <v-btn type="submit" color="primary" class="mr-8" form="edit-form">editar</v-btn>
                <v-btn color="warning" class="mr-8" @click="clear">limpiar</v-btn>
                <v-btn color="secondary" :to="{ name: 'ListBook' }">cancelar</v-btn>

            </form>
         </v-col>
       </v-row>
     </v-card>

   </v-container>  
 </div>
</template>

<script>
  import axios from 'axios'
  import swal from 'sweetalert'

export default {


    data() {
        return {
        bookId: this.$route.params.bookId,
        form: {
            title: '',
            description: '',
        }
      

      }
    },

    methods: {
      clear () {
        this.$v.$reset()
        this.form.title = ''
        this.form.description = ''
      },
      onSubmit (evt) {
        evt.preventDefault()  

        const path = `http://localhost:8000/api/v1.0/books/${this.bookId}/`

          axios.put(path, this.form).then((response) => {

              this.form.title = response.data.title
              this.form.description = response.data.description
              
              swal("Libro actualizado correctamente!", "", "success")
              location.href = '/books'

          })
          .catch((error) => {
              console.log(error)
          })
        
      },

      getBook (){
          const path = `http://localhost:8000/api/v1.0/books/${this.bookId}/`

          axios.get(path).then((response) => {

              this.form.title = response.data.title
              this.form.description = response.data.description
          })
          .catch((error) => {
              console.log(error)
          })
      },
    },
    created(){
    this.getBook()
  }
  }
</script>
