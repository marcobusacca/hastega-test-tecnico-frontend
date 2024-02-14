<script>
export default {
    props: {
        books: Array,
        loggedUser: Object,
    },
    data() {
        return {
        }
    },
    methods: {
    },
}
</script>

<template>
    <div class="container border rounded-5 shadow my-5">
        <div class="row justify-content-center p-4">
            <div class="col-12">
                <span v-text="`Ciao ${this.loggedUser.name} ${this.loggedUser.surname}!`"></span>
            </div>
            <div class="col-12 text-center py-5">
                <button class="btn btn-success mx-2">Aggiungi Libro</button>
                <button class=" btn btn-danger mx-2" @click="$emit('logout')">Logout</button>
            </div>
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
                        <tr v-for="book in books" :key="book.id">
                            <th scope="row" v-text="book.createdAt"></th>
                            <td v-text="book.title"></td>
                            <td v-text="book.author"></td>
                            <td>
                                <button class="btn btn-info mx-1">
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
</template>

<style lang="scss" scoped></style>