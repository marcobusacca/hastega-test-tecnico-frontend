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
            this.store.books = [];
            this.store.newBooks = [];
            this.store.loggedUser = {};
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
                    <div class="col-6 d-flex justify-content-center align-items-center">
                        <img src="../../public/book-vault-logo.png" alt="">
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