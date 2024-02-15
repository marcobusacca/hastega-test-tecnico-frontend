<script>
import { store } from '../store';
import axios from 'axios';

import AppLoader from '../components/AppLoader.vue';

import BookShow from './BookShow.vue';
import BookForm from './BookForm.vue';

export default {
    components: {
        AppLoader,
        BookShow,
        BookForm,
    },
    data() {
        return {
            store,
            books: [],
            newBooks: [],
            bookActive: {},
            creatingBook: false,
            updatingBook: false,
        }
    },
    mounted() {
        this.getBooksByUserId(this.store.loggedUser.id);
    },
    methods: {
        getBooksByUserId(userId) {
            // FACCIO PARTIRE IL LOADING
            this.store.loading = true;

            // FACCIO LA CHIAMATA API PER OTTENERE I BOOKS COLLEGATI ALLO USER LOGGATO
            axios.get(`${this.store.baseUrl}/api/books?user_id=${userId}`).then((response) => {

                // SALVO I DATI DENTRO L'ARRAY BOOKS
                this.books = response.data;

                // AVVIO LA FUNZIONE "CREATE_NEW_BOOKS_ARRAY"
                this.createNewBooksArray();

                // FERMO IL LOADING
                this.store.loading = false;

            }).catch((error) => {

                // STAMPO IN CONSOLE L'ERRORE
                console.error("Errore nella richiesta API getBooksByUserId: ", error);
            });
        },
        bookShow(id) {
            // CICLO L'ARRAY "NEW_BOOKS" (OPPURE BOOKS) PER TROVARE L'ELEMENTO CLICCATO
            this.newBooks.forEach(book => {
                if (book.id === id) {
                    this.bookActive = book;
                }
            });
        },
        bookCreate() {
            this.creatingBook = true;
        },
        bookUpdate(id) {
            this.bookShow(id);
            this.updatingBook = true;
        },
        bookDelete(id) {
            // FACCIO PARTIRE IL LOADING
            this.store.loading = true;

            // FACCIO LA CHIAMATA API DELETE PER CANCELLARE IL LIBRO
            axios.delete(`${this.store.baseUrl}/api/books/${id}`).then((response) => {

                // STAMPO IN CONSOLE LA RISPOSTA
                console.log(response);

                // FERMO IL LOADING
                this.store.loading = false;

                // AVVIO LA CHIAMATA API PER OTTENERE I LIBRI
                this.getBooksByUserId(this.store.loggedUser.id);

            }).catch((error) => {

                // STAMPO IN CONSOLE L'ERRORE
                console.error("Errore nella richiesta API getBooksByUserId: ", error);
            });
        },
        closePage() {
            // SVUOTO L'ELEMENTO CLICCATO IN PRECEDENZA
            this.bookActive = {};
            // SETTO "CREATING_BOOK" A "FALSE"
            this.creatingBook = false;
            // SETTO "UPDATING_BOOK" A "FALSE"
            this.updatingBook = false;
            // AVVIO LA CHIAMATA API PER OTTENERE I LIBRI
            this.getBooksByUserId(this.store.loggedUser.id);
        },
        logout() {
            this.books = [];
            this.newBooks = [];
            this.store.loggedUser = {};
        },
        createNewBooksArray() {
            // MAPPO L'ARRAY DEI LIBRI DELL'UTENTE
            this.newBooks = this.books.map(book => ({
                // IN QUESTO MODO MANTENGO TUTTE LE PROPRIETA GIA ESISTENTI
                ...book,
                // AGGIUNGO LA PROPRIETA "FORMATTED_CREATED_AT"
                formattedCreatedAt: this.formatItalianDate(book.createdAt),
            }))
        },
        formatItalianDate(originalDate) {
            const options = { day: "numeric", month: "numeric", year: "numeric" };
            const date = new Date(originalDate);
            return date.toLocaleDateString("it-IT", options);
        },
    },
}
</script>

<template>
    <!-- COMPONENT: APP LOADER -->
    <app-loader v-if="this.store.loading" />
    <!-- PAGE: BOOK INDEX -->
    <div class="container border rounded-5 shadow my-5"
        v-if="!this.store.loading && !Object.keys(this.bookActive).length && !this.creatingBook">
        <div class="row justify-content-center p-4">
            <!-- NOME UTENTE E EMAIL -->
            <div class="col-12">
                <span
                    v-text="`${this.store.loggedUser.name} ${this.store.loggedUser.surname} - ${this.store.loggedUser.email}`"
                    class="fw-bold"></span>
            </div>
            <!-- BOTTONE: CREATE BOOK, BOTTONE: LOGOUT -->
            <div class="col-12 text-center py-5">
                <button class="btn btn-success mx-2" @click="bookCreate">Aggiungi Libro</button>
                <button class=" btn btn-danger mx-2" @click="logout">Logout</button>
            </div>
            <!-- TABELLA CON LISTA LIBRI ASSOCIATI ALL'UTENTE -->
            <div class="col-12 py-3">
                <table class="table table-hover text-center">
                    <thead>
                        <tr>
                            <th scope="col">Data Inserimento</th>
                            <th scope="col">Titolo</th>
                            <th scope="col">Autore</th>
                            <th scope="col">Strumenti</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-if="!this.books.length">
                            <td colspan="4" class="py-5">Nessun libro trovato</td>
                        </tr>
                        <tr v-for="book in newBooks" :key="book.id" v-else>
                            <!-- DATA DI INSERIMENTO -->
                            <td v-text="book.formattedCreatedAt"></td>
                            <!-- TITOLO -->
                            <td v-text="book.title"></td>
                            <!-- AUTORE -->
                            <td v-text="book.author"></td>
                            <!-- STRUMENTI: SHOW, EDIT, DELETE -->
                            <td>
                                <button class="btn btn-info mx-1" @click="bookShow(book.id)">
                                    <i class="fas fa-eye"></i>
                                </button>
                                <button class="btn btn-warning mx-1" @click="bookUpdate(book.id)">
                                    <i class="fas fa-edit"></i>
                                </button>
                                <button class="btn btn-danger mx-1" @click="bookDelete(book.id)">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
    <!-- PAGE: BOOK SHOW -->
    <book-show v-if="Object.keys(this.bookActive).length > 0 && !this.updatingBook" :book="this.bookActive"
        @close-page="closePage" />
    <!-- PAGE: BOOK FORM -->
    <book-form v-if="this.creatingBook || this.updatingBook" :book="this.bookActive" @close-page="closePage" />
</template>

<style lang="scss" scoped></style>