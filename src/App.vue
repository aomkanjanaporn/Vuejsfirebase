<template>
  <div id="app"  class="containner row row justify-content-md-center">
    <div class=" col-md-8 text-center">
      <img src="./assets/logo.png" >
      <div class="page-header ">
        <h1>Vue.js 2 & Firebase Sample Application</h1>
      </div>
      <br>
      <br>
      <br>
      <div class="row">
        <div class="card col-md-5">
          <br>
          <br>
            <h3 >Add Book</h3>
            <hr>
          <div class="card-body">
            <form id="form"  v-on:submit.prevent="addBook">
              <div class="form-group form-inline row">
                <label for="bookTitle" class="col-md-2 card-title">Title :</label>
                <input type="text" id="bookTitle" class="form-control col-md-9" v-model="newBook.title" required>
              </div>
              <div class="form-group form-inline row">
                <label for="bookAuthor" class="col-md-2 card-title">Author :</label>
                <input type="text" id="bookAuthor" class="form-control col-md-9" v-model="newBook.author" required>
              </div>
              <div class="form-group form-inline row">
                <label for="bookUrl" class="col-md-2 card-title">URL :</label>
                <input type="text" id="bookUrl" class="form-control col-md-9" v-model="newBook.url" required>
              </div>
              <br>
              <input type="submit" class="btn btn-primary" value="Add Book"> 
            </form>

          </div>
        </div>
        <div class="col-md-1">
          <br>
        </div>
        <div class=" col-md-6">
            <h3>Books List</h3>
            <table class="table table-striped ">
              <thead>
                <tr>
                  <th>Title</th>
                  <th>Author</th>
                  <th>Delete</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="book in books">
                  <td>
                  <a v-bind:href="book.url">{{ book.title }}</a> 
                  </td>
                  <td>
                    {{ book.author }}
                  </td>
                  <td>
                    <span  v-on:click="removeBook(book)"> <button class="btn btn-danger">delete</button> </span>
                  </td>
                </tr>
              </tbody>
            </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'
import Firebase from 'firebase'
import toastr from 'toastr'

// config firebase
let config = {
    apiKey: "AIzaSyAewcs2Wu1pUxRYXLLNdHJeD5uQSseRsVY",
    authDomain: "vuejsfirebase-654bb.firebaseapp.com",
    databaseURL: "https://vuejsfirebase-654bb.firebaseio.com",
    projectId: "vuejsfirebase-654bb",
    storageBucket: "vuejsfirebase-654bb.appspot.com",
    messagingSenderId: "418733241836"
}
let app = Firebase.initializeApp(config);
let db = app.database();

let booksRef = db.ref('books');

export default {
  name: 'app',
  firebase: {
    books: booksRef
  },
  data(){
    return{
      newBook: {
        title:'',
        author:'',
        url:''
      }
    }
  },
  methods:{
    addBook:function(){
      booksRef.push(this.newBook);
      this.newBook.title = '';
      this.newBook.author = '';
      this.newBook.url = '';

    },
    removeBook: function(book){
      booksRef.child(book['.key']).remove();
      toastr.success("Book remove");
    }
  }
}
</script>

<style>

</style>
