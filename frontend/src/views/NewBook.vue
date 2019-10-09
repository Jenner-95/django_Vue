<template>
 <div class="dashboard">
   <h1 class="subheading grey--text">Nuevo Libro</h1>   
   <v-container class="my-5">

     <v-card flat class="pa-3">
       <v-row align="center" justify="center">
         <v-col cols="8" md="8">
            <form @submit="onSubmit" id="edit-form">
                <v-text-field outlined v-model.trim="form.title" :error-messages="titleErrors" :counter="50" label="Titulo" required @input="$v.title.$touch()" @blur="$v.title.$touch()"></v-text-field>
                <v-textarea outlined v-model.trim="form.description" :error-messages="descriptionErrors" :counter="500" label="Descripcion" textarea rows="10" auto-grow required @input="$v.description.$touch()" @blur="$v.description.$touch()"></v-textarea>
            
                <v-btn type="submit" color="primary" class="mr-8" form="edit-form">crear</v-btn>
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
  import { validationMixin } from 'vuelidate'
  import { required, maxLength, email } from 'vuelidate/lib/validators'
  import axios from 'axios'
  import swal from 'sweetalert'

export default {
mixins: [validationMixin],

    validations: {
      title: { required, maxLength: maxLength(50) },
      description: { required, maxLength: maxLength(500) },
    },

    data() {
        return {
        form: {
            title: '',
            description: '',
        }
      

      }
    },

    computed: {
      titleErrors () {
        const errors = []
        if (!this.$v.title.$dirty) return errors
        !this.$v.title.maxLength && errors.push('Title must be at most 50 characters long')
        !this.$v.title.required && errors.push('Title is required.')
        return errors
      },
      descriptionErrors () {
        const errors = []
        if (!this.$v.description.$dirty) return errors
        !this.$v.description.maxLength && errors.push('Description must be at most 500 characters long')
        !this.$v.description.required && errors.push('Description is required.')
        return errors
      },

    },

    methods: {
      clear () {
        this.$v.$reset()
        this.form.title = ''
        this.form.description = ''
      },
      onSubmit (evt) {
        evt.preventDefault()  
        this.$v.$touch()

        const path = 'http://localhost:8000/api/v1.0/books/'

          axios.post(path, this.form).then((response) => {

              this.form.title = response.data.title
              this.form.description = response.data.description
              
              swal("Libro creado correctamente!", "", "success"),

              location.href = '/books'

          })
          .catch((error) => {
              swal("No se ha creado ningun registro!", "", "error")
          })
        
      },
    },
    created(){
    
  }
  }
</script>
