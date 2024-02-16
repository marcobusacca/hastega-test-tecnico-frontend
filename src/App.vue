<script>
import { store } from './store';

import AppHeader from './components/AppHeader.vue';
import AppFooter from './components/AppFooter.vue';

import Login from './pages/Login.vue';
import BookIndex from './pages/BookIndex.vue';
import UserDetails from './pages/UserDetails.vue';

export default {

  components: {
    AppHeader,
    AppFooter,
    Login,
    BookIndex,
    UserDetails,
  },
  data() {
    return {
      store,
    }
  },
}
</script>

<template>
  <!-- COMPONENT: APP HEADER -->
  <app-header />
  <main>
    <!-- PAGE: LOGIN -->
    <login v-if="!Object.keys(this.store.loggedUser).length" />
    <!-- PAGE: BOOK INDEX -->
    <book-index v-if="Object.keys(this.store.loggedUser).length > 0 && !this.store.userDetails" />
    <!-- PAGE: USER DETAILS -->
    <user-details v-if="Object.keys(this.store.loggedUser).length > 0 && this.store.userDetails" />
  </main>
  <!-- COMPONENT: APP FOOTER -->
  <app-footer />
</template>

<style lang="scss">
// IMPORTO GENERALS.SCSS
@use './styles/generals.scss' as *;
</style>
