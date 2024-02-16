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
            bookActive: {},
            creatingBook: false,
            updatingBook: false,
            confirmDeleteBookId: null,
            confirmDeleteBookTitle: "",
            showConfirmBanner: false,
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
                this.store.books = response.data;

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
            this.store.newBooks.forEach(book => {
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
        confirmDelete(id, title) {
            // SALVO ID E TITOLO DEL LIBRO DA CANCELLARE
            this.confirmDeleteBookId = id;
            this.confirmDeleteBookTitle = title;
        },
        cancelDelete() {
            // SVUOTO LE VARIABILI DI APPOGGIO
            this.confirmDeleteBookId = null;
            this.confirmDeleteBookTitle = "";
        },
        proceedDelete() {
            // RICHIAMO LA FUNZIONE PER ELIMINARE IL LIBRO
            this.bookDelete(this.confirmDeleteBookId);
            // SVUOTO LE VARIABILI DI APPOGGIO
            this.cancelDelete();
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

                // SETTO "SHOW_CONFIRM_BANNER" A TRUE
                this.showConfirmBanner = true;

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
            // SETTO "SHOW_CONFIRM_BANNER" A "FALSE"
            this.showConfirmBanner = false;
            // AVVIO LA CHIAMATA API PER OTTENERE I LIBRI
            this.getBooksByUserId(this.store.loggedUser.id);
        },
        formSubmitted() {
            this.closePage();
            this.showConfirmBanner = true;
        },
        createNewBooksArray() {
            // MAPPO L'ARRAY DEI LIBRI DELL'UTENTE
            this.store.newBooks = this.store.books.map(book => ({
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
    <div class="container-fluid py-5"
        v-if="!this.store.loading && !Object.keys(this.bookActive).length && !this.creatingBook">
        <div class="container" v-if="this.showConfirmBanner">
            <div class="row">
                <!-- CONFIRM BANNER -->
                <div class="col-12">
                    <div class="alert alert-success">
                        <span>Operazione effettuata</span>
                    </div>
                </div>
            </div>
        </div>
        <div class="container bg-white border rounded-5 shadow">
            <div class="row justify-content-center p-4">
                <!-- BOTTONE: CREATE BOOK -->
                <div class="col-12 text-center mb-5">
                    <button class="btn btn-success mx-2" @click="bookCreate">Aggiungi Libro</button>
                </div>
                <!-- TABELLA CON LISTA LIBRI ASSOCIATI ALL'UTENTE -->
                <div class="col-12 my-3">
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
                            <tr v-if="!this.store.books.length">
                                <td colspan="4" class="py-5">Nessun libro trovato</td>
                            </tr>
                            <tr v-for="book in this.store.newBooks" :key="book.id" v-else>
                                <!-- DATA DI INSERIMENTO -->
                                <td v-text="book.formattedCreatedAt"></td>
                                <!-- TITOLO -->
                                <td v-text="book.title"></td>
                                <!-- AUTORE -->
                                <td v-text="book.author"></td>
                                <!-- STRUMENTI: SHOW, EDIT, DELETE -->
                                <td>
                                    <!-- BUTTON SHOW -->
                                    <button class="btn btn-info mx-1" @click="bookShow(book.id)">
                                        <i class="fas fa-eye"></i>
                                    </button>
                                    <!-- BUTTON EDIT -->
                                    <button class="btn btn-warning mx-1" @click="bookUpdate(book.id)">
                                        <i class="fas fa-edit"></i>
                                    </button>
                                    <!-- BUTTON DELETE -->
                                    <button class="btn btn-danger mx-1" data-bs-toggle="modal"
                                        data-bs-target="#deleteBookModal" @click="confirmDelete(book.id, book.title)">
                                        <i class="fas fa-trash"></i>
                                    </button>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
        <!-- MODALE DI CONFERMA ELIMINAZIONE LIBRO -->
        <div class="modal fade" id="deleteBookModal" tabindex="-1" aria-labelledby="deleteBookModalLabel"
            aria-hidden="true">
            <div class="modal-dialog">
                <div class="modal-content">
                    <div class="modal-header">
                        <!-- TITOLO DEL LIBRO DA ELIMINARE -->
                        <h1 class="modal-title fs-5" id="deleteBookModalLabel" v-text="this.confirmDeleteBookTitle"></h1>
                        <!-- BOTTONE DI ANNULLAMENTO -->
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"
                            @click="cancelDelete"></button>
                    </div>
                    <div class="modal-body">
                        Sei sicuro di voler eliminare questo libro?
                    </div>
                    <div class="modal-footer">
                        <!-- BOTTONE DI ANNULLAMENTO -->
                        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal"
                            @click="cancelDelete">Annulla</button>
                        <!-- BOTTONE DI CONFERMA ELIMINAZIONE -->
                        <button type="button" class="btn btn-danger" data-bs-dismiss="modal"
                            @click="proceedDelete">Elimina</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <!-- PAGE: BOOK SHOW -->
    <book-show v-if="Object.keys(this.bookActive).length > 0 && !this.updatingBook" :book="this.bookActive"
        @close-page="closePage" />
    <!-- PAGE: BOOK FORM -->
    <book-form v-if="this.creatingBook || this.updatingBook" :book="this.bookActive" @close-page="closePage"
        @form-submitted="formSubmitted" />
</template>

<style lang="scss" scoped></style>