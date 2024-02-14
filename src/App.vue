<script>
import axios from 'axios';

import AppHeader from './components/AppHeader.vue';

import Login from './pages/Login.vue';

export default {

  components: {
    AppHeader,
    Login,
  },
  data() {
    return {
      baseUrl: 'http://127.0.0.1:8080',
      loading: false,
      users: [],
      loggedUserId: null,
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
        console.error("Errore nella richiesta API: ", error);

        // FERMO IL LOADING
        this.loading = false;
      });
    },
    LoginCompleted(loggedUserId) {
      this.loggedUserId = loggedUserId;
    }
  }
}
</script>

<template>
  <app-header />
  <main>
    <login v-if="this.loading === false && this.loggedUserId === null" :users="this.users" @get-books="LoginCompleted" />
  </main>
</template>

<style lang="scss">
// IMPORTO GENERALS.SCSS
@use './styles/generals.scss' as *;
</style>
