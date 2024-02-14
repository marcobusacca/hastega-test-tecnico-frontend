<script>
import { store } from './store';
import axios from 'axios';

import AppHeader from './components/AppHeader.vue';

export default {

  components: {
    AppHeader,
  },
  data() {
    return {
      store,
      users: [],
    }
  },
  mounted() {
    this.getUsers();
  },
  methods: {
    getUsers() {
      // FACCIO PARTIRE IL LOADING
      this.store.loading = true;

      // FACCIO LA CHIAMATA API PER OTTENERE GLI USERS
      axios.get(`${store.baseUrl}/api/users`).then((response) => {

        // SALVO I DATI DENTRO L'ARRAY USERS
        this.users = response.data;

        // FERMO IL LOADING
        this.store.loading = false;

      }).catch((error) => {

        // STAMPO IN CONSOLE L'ERRORE
        console.error("Errore nella richiesta API: ", error);

        // FERMO IL LOADING
        this.store.loading = false;
      });
    }
  }
}
</script>

<template>
  <app-header />
</template>

<style lang="scss">
// IMPORTO GENERALS.SCSS
@use './styles/generals.scss' as *;
</style>
