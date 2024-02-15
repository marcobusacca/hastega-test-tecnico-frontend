<script>
import { store } from '../store';
import axios from 'axios';

export default {
    emits: ['closePage'],
    props: {
        book: Object,
    },
    data() {
        return {
            store,
            newBook: {},
            formErrors: {},
        }
    },
    mounted() {
        this.setNewBook();
    },
    methods: {
        setNewBook() {
            if (!Object.keys(this.book).length) {
                this.newBook = {
                    user: {},
                    title: "",
                    author: "",
                    plot: "",
                    readingNumber: 0,
                    isbnCode: "",
                };
            } else {
                this.newBook = {
                    title: this.book.title,
                    author: this.book.author,
                    plot: this.book.plot,
                    readingNumber: this.book.readingNumber,
                    isbnCode: this.book.isbnCode,
                }
            }
        },
        submitForm() {
            // FACCIO PARTIRE IL LOADING
            this.store.loading = true;

            // RESETTO IL FORM DEGLI ERRORI
            this.formErrors = {};

            // EFFETTUO LE VALIDAZIONI (TUTTI I CAMPI SONO OBBLIGATORI)
            if (this.newBook.title === "") {
                this.formErrors.title = "Il titolo è obbligatorio";
            }
            if (this.newBook.author === "") {
                this.formErrors.author = "L'autore è obbligatorio";
            }
            if (this.newBook.plot === "") {
                this.formErrors.plot = "La trama è obbligatoria";
            }
            if (this.newBook.readingNumber === "" || this.newBook.readingNumber < 0) {
                this.formErrors.readingNumber = "Il numero di letture complete è obbligatorio e deve essere superiore o uguale a 0";
            }
            if (this.newBook.isbnCode === "") {
                this.formErrors.isbnCode = "Il codice ISBN è obbligatorio";
            }

            // SE CI SONO ERRORI, FERMO IL METODO
            if (Object.keys(this.formErrors).length > 0) {
                // FERMO IL LOADING
                this.store.loading = false;
                return;
            }

            if (!Object.keys(this.book).length) { // SE LA PROP "BOOK" È VUOTA, FACCIO LA CHIAMATA POST PER CREARE IL LIBRO

                this.newBook.user = this.store.loggedUser;

                axios.post(`${this.store.baseUrl}/api/books`, this.newBook).then((response) => {

                    // STAMPO IN CONSOLE LA RISPOSTA
                    console.log(response);

                    // FERMO IL LOADING
                    this.store.loading = false;

                    // LANCIO L'EMITS "CLOSE_PAGE"
                    this.$emit('closePage');

                }).catch((error) => {

                    // STAMPO IN CONSOLE L'ERRORE
                    console.error("Errore nella richiesta API storeBook: ", error);
                });

            } else { // SE LA PROP "BOOK" NON È VUOTA, FACCIO LA CHIAMATA PUT PER AGGIORNARE IL LIBRO

                axios.put(`${this.store.baseUrl}/api/books/${this.book.id}`, this.newBook).then((response) => {

                    // STAMPO IN CONSOLE LA RISPOSTA
                    console.log(response);

                    // FERMO IL LOADING
                    this.store.loading = false;

                    // LANCIO L'EMITS "CLOSE_PAGE"
                    this.$emit('closePage');

                }).catch((error) => {

                    // STAMPO IN CONSOLE L'ERRORE
                    console.error("Errore nella richiesta API updateBook: ", error);
                });
            }
        }
    },
}
</script>

<template>
    <div class="container-fluid py-5">
        <div class="container">
            <div class="row">
                <div class="col-12 text-center">
                    <button class="btn btn-dark" @click="$emit('closePage')">Torna Indietro</button>
                </div>
            </div>
        </div>
        <div class="container border rounded-5 shadow my-5">
            <div class="row justify-content-center py-3 px-3">
                <div class="col-12 mb-5" v-if="Object.keys(book).length > 0">
                    <h3>Modifica {{ this.book.title }}</h3>
                </div>
                <div class="col-12" v-if="Object.keys(formErrors).length > 0">
                    <div class="alert alert-danger">
                        <ul>
                            <li v-for="(error, key) in formErrors" :key="key" v-text="error"></li>
                        </ul>
                    </div>
                </div>
                <div class="col-12 text-center">
                    <form @submit.prevent="submitForm">
                        <div class="input-group mb-4">
                            <span class="input-group-text" id="span-addon-1">Titolo</span>
                            <input type="text" name="title" id="title" aria-describedby="span-addon-1" class="form-control"
                                placeholder="Inserisci il titolo" v-model="newBook.title">
                        </div>
                        <div class="input-group mb-4">
                            <span class="input-group-text" id="span-addon-2">Autore</span>
                            <input type="text" name="author" id="author" aria-describedby="span-addon-2"
                                class="form-control" placeholder="Inserisci l'autore" v-model="newBook.author">
                        </div>
                        <div class="input-group mb-4">
                            <span class="input-group-text" id="span-addon-3">Trama</span>
                            <textarea name="plot" id="plot" cols="30" rows="10" aria-describedby="span-addon-3"
                                class="form-control" placeholder="Inserisci la trama" v-model="newBook.plot"></textarea>
                        </div>
                        <div class="input-group mb-4">
                            <span class="input-group-text" id="span-addon-4">Numero di letture complete</span>
                            <input type="number" name="readingNumber" id="readingNumber" aria-describedby="span-addon-4"
                                class="form-control" placeholder="Inserisci il numero di letture complete" min="0" step="1"
                                v-model="newBook.readingNumber">
                        </div>
                        <div class="input-group mb-4">
                            <span class="input-group-text" id="span-addon-5">Codice ISBN</span>
                            <input type="text" name="isbnCode" id="isbnCode" aria-describedby="span-addon-5"
                                class="form-control" placeholder="Inserisci il codice ISBN" v-model="newBook.isbnCode">
                        </div>
                        <button type="submit"
                            :class="!Object.keys(this.book).length ? 'btn btn-success' : 'btn btn-warning'">
                            {{ !Object.keys(this.book).length ? " Crea" : "Aggiorna" }} </button>
                    </form>
                </div>
            </div>
        </div>
    </div>
</template>

<style lang="scss" scoped></style>