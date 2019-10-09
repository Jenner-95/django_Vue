<template>
 <div class="dashboard">
   <div style="margin-left:60px">
    <h1 class="subheading grey--text" style="margin-left:450px">Listado de Libros</h1>
    <v-btn color="secondary" class="mr-8" :to="{ name: 'NewBook' }">nuevo libro</v-btn>
   </div>
   <v-container  style="background:#263238;color:white" class="my-5" v-for="book in books" :key="book.title">

     <v-card flat class="pa-3">
       <v-row>
         <v-col cols="4" md="4">
           <div class="caption grey--text">Titulo</div>
           
            <div>{{ book.title }}</div>  
         </v-col>
         <v-col cols="4" md="4">
           <div class="caption grey--text">Descripcion</div>
            <div>{{ book.description }}</div>  
         </v-col>
         <v-col cols="4" md="4">
           <div class="caption grey--text"></div>
              <v-btn class="ma-1" color="primary" v-bind:to="{ name:'EditBook', params: {bookId: book.id}}" dark>Editar
                <v-icon dark right>mdi-checkbox-marked-circle</v-icon>
              </v-btn>
              <v-btn class="ma-2" color="red" v-bind:to="{ name:'DeleteBook', params: {bookId: book.id}}" dark>Eliminar
                <v-icon dark right>mdi-cancel</v-icon>
              </v-btn>
         </v-col>
       </v-row>
     </v-card>

   </v-container>  

 </div>

</template>



<script>
import axios from 'axios';

export default {
  data() {
    return {
      headers:[
        { value: 'title', text: 'Titulo' },
        { value: 'description', text: 'Descripcion'},
        { value: 'action', text: 'Accion' }
      ],
      books: []

    }
  },
  methods: {
    getBooks (){

      const path = 'http://localhost:8000/api/v1.0/books/'
      axios.get(path).then((response) => {
        this.books = response.data
      })
      .catch((error) => {
        console.log(error)
      })
    }
  },

  created(){
    this.getBooks()
  }  
}
</script>
