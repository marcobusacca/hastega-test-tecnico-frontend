<script>
import { store } from '../store';

export default {
    data() {
        return {
            store,
        }
    },
    methods: {
        logout() {
            this.store.loggedUser = {};
            this.store.userDetails = false;
            this.store.books = [];
            this.store.newBooks = [];
        },
    },
}
</script>

<template>
    <header>
        <!-- Header Navbar -->
        <nav class="bg-white border shadow py-3 h-100">
            <div class="container-fluid">
                <div class="row">
                    <div class="d-flex justify-content-center align-items-center"
                        :class="!Object.keys(this.store.loggedUser).length ? 'col-12' : 'col-6'">
                        <img src="/book-vault-logo.png" alt="">
                        <h1 class="app-title">BookVault</h1>
                    </div>
                    <div class="col-6 d-flex justify-content-center align-items-center"
                        v-if="Object.keys(this.store.loggedUser).length > 0">
                        <div class="dropdown">
                            <a class="btn bg-black text-white dropdown-toggle" href="#" role="button"
                                data-bs-toggle="dropdown" aria-expanded="false">
                                {{ this.store.loggedUser.name }}
                            </a>
                            <ul class="dropdown-menu">
                                <li class="dropdown-item" @click="this.store.userDetails = true">Profilo</li>
                                <li class="dropdown-item" @click="logout">Logout</li>
                            </ul>
                        </div>
                    </div>
                </div>
            </div>
        </nav>
    </header>
</template>

<style lang="scss" scoped>
header {
    .app-title {
        font-weight: 700;
    }
}
</style>