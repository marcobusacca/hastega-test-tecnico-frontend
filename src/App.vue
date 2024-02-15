<script>
import { store } from './store';

import AppHeader from './components/AppHeader.vue';

import Login from './pages/Login.vue';
import BookIndex from './pages/BookIndex.vue';

export default {

  components: {
    AppHeader,
    Login,
    BookIndex,
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
    <book-index v-else />
  </main>
</template>

<style lang="scss">
// IMPORTO GENERALS.SCSS
@use './styles/generals.scss' as *;
</style>
