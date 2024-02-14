<script>
import axios from 'axios';

import AppHeader from './components/AppHeader.vue';
import AppLoader from './components/AppLoader.vue';

import Login from './pages/Login.vue';
import BookIndex from './pages/BookIndex.vue';

export default {

  components: {
    AppHeader,
    AppLoader,
    Login,
    BookIndex,
  },
  data() {
    return {
      baseUrl: 'http://127.0.0.1:8080',
      loading: false,
      users: [],
      loggedUser: {},
      books: [],
    }
  },
  mounted() {
    this.getUsers();
  },
  methods: {
    getUsers() {
      // FACCIO PARTIRE IL LOADING
      this.loading = true;

      // FACCIO LA CHIAMATA API PER OTTENERE GLI USERS
      axios.get(`${this.baseUrl}/api/users`).then((response) => {

        // SALVO I DATI DENTRO L'ARRAY USERS
        this.users = response.data;

        // FERMO IL LOADING
        this.loading = false;

      }).catch((error) => {

        // STAMPO IN CONSOLE L'ERRORE
        console.error("Errore nella richiesta API getUsers: ", error);

        // FERMO IL LOADING
        this.loading = false;
      });
    },
    getBooksByUserId(userId) {
      // FACCIO PARTIRE IL LOADING
      this.loading = true;

      // FACCIO LA CHIAMATA API PER OTTENERE I BOOKS COLLEGATI ALLO USER LOGGATO
      axios.get(`${this.baseUrl}/api/books?user_id=${userId}`).then((response) => {

        // SALVO I DATI DENTRO L'ARRAY BOOKS
        this.books = response.data;

        // FERMO IL LOADING
        this.loading = false;

      }).catch((error) => {

        // STAMPO IN CONSOLE L'ERRORE
        console.error("Errore nella richiesta API getBooksByUserId: ", error);

        // FERMO IL LOADING
        this.loading = false;
      });
    },
    LoginCompleted(loggedUserId) {

      // CICLO L'ARRAY USERS
      this.users.forEach(user => {
        // SALVO LO USER LOGGATO DENTRO "LOGGED_USER"
        if (user.id === loggedUserId) {
          this.loggedUser = user;
        }
      });

      // RICHIAMO LA FUNZIONE "GET_BOOKS_BY_USER_ID"
      this.getBooksByUserId(loggedUserId);
    },
    logout() {
      this.loggedUser = {};
      this.books = [];
    }
  }
}
</script>

<template>
  <!-- COMPONENT: APP HEADER -->
  <app-header />
  <main>
    <!-- COMPONENT: APP LOADER -->
    <app-loader v-if="this.loading" />
    <!-- PAGE: LOGIN -->
    <login v-if="this.loading === false && !Object.keys(this.loggedUser).length" :users="this.users"
      @login-completed="LoginCompleted" />
    <!-- PAGE: BOOK INDEX -->
    <book-index v-if="this.loading === false && this.books.length !== 0" :books="this.books" :loggedUser="this.loggedUser"
      @logout="logout" />
  </main>
</template>

<style lang="scss">
// IMPORTO GENERALS.SCSS
@use './styles/generals.scss' as *;
</style>
