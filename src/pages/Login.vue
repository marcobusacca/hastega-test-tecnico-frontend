<script>
import { store } from '../store';
import axios from 'axios';

import AppLoader from '../components/AppLoader.vue';

export default {
    components: {
        AppLoader,
    },
    data() {
        return {
            store,
            users: [],
            loggedUserId: "",
            error: false,
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
            axios.get(`${this.store.baseUrl}/api/users`).then((response) => {

                // SALVO I DATI DENTRO L'ARRAY USERS
                this.users = response.data;

                // FERMO IL LOADING
                this.store.loading = false;

            }).catch((error) => {

                // STAMPO IN CONSOLE L'ERRORE
                console.error("Errore nella richiesta API getUsers: ", error);
            });
        },
        checkLogin() {
            if (this.loggedUserId !== "") {
                // CICLO L'ARRAY USERS
                this.users.forEach(user => {
                    // SALVO LO USER LOGGATO DENTRO "LOGGED_USER"
                    if (user.id === this.loggedUserId) {
                        this.store.loggedUser = user;
                    }
                });
            } else {
                this.error = true;
            }
        },
    },
}
</script>

<template>
    <!-- COMPONENT: APP LOADER -->
    <app-loader v-if="this.store.loading" />
    <!-- PAGE: LOGIN -->
    <div class="container border rounded-5 shadow my-5" v-else>
        <div class="row py-5">
            <!-- TITOLO PAGINA LOGIN -->
            <div class="col-12 pb-5 px-4">
                <h3>Scegli l'account con cui entrare</h3>
            </div>
            <!-- CONTAINER FORM DI LOGIN -->
            <div class="col-12">
                <div class="row">
                    <!-- MENU A TENDINA PER SELEZIONE ACCOUNT -->
                    <div class="col-12 form-check text-center py-5">
                        <select class="form-select mb-5" v-model="this.loggedUserId">
                            <option value="" selected hidden>Seleziona l'account</option>
                            <option v-for="user in users" :key="user.id" :value="user.id" v-text="user.email"></option>
                        </select>
                        <span v-if="error" class="text-danger fw-bold">Devi selezionare l'account</span>
                    </div>
                    <!-- BOTTONE DI INVIO PROCEDURA DI LOGIN -->
                    <div class="col-12 text-center">
                        <button type="submit" class="btn btn-primary text-center" @click="checkLogin">Entra</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<style lang="scss" scoped></style>