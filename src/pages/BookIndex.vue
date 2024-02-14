<script>
import BookShow from './BookShow.vue';

export default {
    props: {
        books: Array,
        loggedUser: Object,
    },
    components: {
        BookShow,
    },
    data() {
        return {
            newBooks: [],
            bookActive: {},
        }
    },
    mounted() {
        this.createNewBooksArray();
    },
    methods: {
        createNewBooksArray() {
            // MAPPO L'ARRAY DEI LIBRI DELL'UTENTE
            this.newBooks = this.books.map(book => ({
                // IN QUESTO MODO MANTENGO TUTTE LE PROPRIETA GIA ESISTANTI
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
        bookShow(id) {
            // CICLO L'ARRAY "NEW_BOOKS" (OPPURE BOOKS) PER TROVARE L'ELEMENTO CLICCATO
            this.newBooks.forEach(book => {
                if (book.id === id) {
                    this.bookActive = book;
                }
            });
        },
        closePage() {
            // SVUOTO L'ELEMENTO CLICCATO IN PRECEDENZA
            this.bookActive = {};
        }
    },
}
</script>

<template>
    <div class="container border rounded-5 shadow my-5" v-if="!Object.keys(this.bookActive).length">
        <div class="row justify-content-center p-4">
            <!-- NOME UTENTE  -->
            <div class="col-12">
                <span v-text="`${this.loggedUser.name} ${this.loggedUser.surname}`" class="fw-bold"></span>
            </div>
            <!-- BOTTONE: CREATE BOOK, BOTTONE: LOGOUT -->
            <div class="col-12 text-center py-5">
                <button class="btn btn-success mx-2">Aggiungi Libro</button>
                <button class=" btn btn-danger mx-2" @click="$emit('logout')">Logout</button>
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
                        <tr v-for="book in newBooks" :key="book.id">
                            <!-- DATA DI INSERIMENTO -->
                            <th scope="row" v-text="book.formattedCreatedAt"></th>
                            <!-- TITOLO -->
                            <td v-text="book.title"></td>
                            <!-- AUTORE -->
                            <td v-text="book.author"></td>
                            <!-- STRUMENTI: SHOW, EDIT, DELETE -->
                            <td>
                                <button class="btn btn-info mx-1" @click="bookShow(book.id)">
                                    <i class="fas fa-eye"></i>
                                </button>
                                <button class="btn btn-warning mx-1">
                                    <i class="fas fa-edit"></i>
                                </button>
                                <button class="btn btn-danger mx-1">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
    <book-show v-else :book="this.bookActive" @close-page="closePage" />
</template>

<style lang="scss" scoped></style>