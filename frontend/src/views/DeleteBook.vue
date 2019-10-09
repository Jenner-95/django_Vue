<template>
   <v-container class="my-5">

     <v-card flat class="pa-3">
       <v-row align="center" justify="center">
         <v-col cols="8" md="8">
             <h3>Estas seguro que deseas eliminar este libro?</h3>
             <p>Titulo: {{ this.element.title }}</p>
             <p>Descripcion: {{ this.element.description }}</p>
             
             <v-btn color="error" class="mr-8" @click="deleteBook">Eliminar</v-btn>
             <v-btn color="secondary" :to="{ name: 'ListBook' }">cancelar</v-btn>


         </v-col>
       </v-row>
     </v-card>

   </v-container>  
</template>
<script>
import swal from 'sweetalert'
import axios from 'axios';
export default {
    data(){
        return {
            bookId: this.$route.params.bookId,
            element: {
                title: '',
                description: ''
            }
        }
    },
    methods : {
        getBook(){
            const path = `http://127.0.0.1:8000/api/v1.0/books/${this.bookId}/`
            axios.get(path).then((response)=>{
                this.element.title = response.data.title;
                this.element.description = response.data.description;
            })
            .catch((error) =>{
                console.log(error);
            })
        },
        deleteBook(){
            
            const path = `http://127.0.0.1:8000/api/v1.0/books/${this.bookId}/`
            axios.delete(path).then((response)=>{
                swal("Libro eliminado correctamente", "", "success"),

                location.href = '/books'
            })
            .catch((error) => {
                swal("No es posible eliminar el libro", "", "error")
            })
        }
    },
    created(){
        this.getBook()
    }
}
</script>