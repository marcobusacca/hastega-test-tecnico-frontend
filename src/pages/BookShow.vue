<script>
export default {
    emits: ['closePage'],
    props: {
        book: Object,
    },
}
</script>

<template>
    <div class="container-fluid py-5">
        <div class="container">
            <div class="row">
                <!-- BUTTON CLOSE PAGE -->
                <div class="col-12 text-center">
                    <button class="btn btn-dark" @click="$emit('closePage')">Torna Indietro</button>
                </div>
            </div>
        </div>
        <div class="container bg-white border rounded-5 shadow my-5">
            <div class="row justify-content-center py-3 px-3">
                <!-- DATA DI INSERIMENTO NEL DB DEL LIBRO -->
                <div class="col-12">
                    <span v-text="book.formattedCreatedAt" class="fw-bold"></span>
                </div>
                <div class="col-12 py-5">
                    <div class="row">
                        <!-- TITOLO -->
                        <div class="col-12 my-3">
                            <h1 v-text="book.title"></h1>
                        </div>
                        <!-- AUTORE -->
                        <div class="col-12 my-3">
                            <label class="fw-bold" for="book-author">Autore:&nbsp;</label>
                            <span v-text="book.author" id="book-author"></span>
                        </div>
                        <!-- TRAMA -->
                        <div class="col-12 my-3">
                            <label class="fw-bold" for="book-plot">Trama</label>
                            <p v-text="book.plot" id="book-plot"></p>
                        </div>
                        <!-- NUMERO DI LETTURE COMPLETE -->
                        <div class="col-12 my-3">
                            <label class="fw-bold" for="book-reading-number">Numero di letture complete:&nbsp;</label>
                            <span v-text="book.readingNumber" id="book-reading-number"></span>
                        </div>
                    </div>
                </div>
                <!-- CODICE ISBN -->
                <div class="col-12">
                    <label class="fw-bold" for="book-isbn">ISBN:&nbsp;</label>
                    <span v-text="book.isbnCode" id="book-isbn"></span>
                </div>
            </div>
        </div>
    </div>
</template>

<style lang="scss" scoped></style>