<script>
import { store } from '../store';

export default {
    data() {
        return {
            store,
        }
    },
}
</script>

<template>
    <div class="container-fluid py-5">
        <div class="container">
            <div class="row">
                <!-- BUTTON CLOSE PAGE -->
                <div class="col-12 text-center">
                    <button class="btn btn-dark" @click="this.store.userDetails = false">Torna Indietro</button>
                </div>
            </div>
        </div>
        <div class="container bg-white border rounded-5 shadow my-5">
            <div class="row justify-content-center py-3 px-3">
                <div class="col-12 my-3">
                    <label class="fw-bold" for="user-name">Nome:&nbsp;</label>
                    <span v-text="this.store.loggedUser.name" id="user-name"></span>
                </div>
                <div class="col-12 my-3">
                    <label class="fw-bold" for="user-surname">Cognome:&nbsp;</label>
                    <span v-text="this.store.loggedUser.surname" id="user-surname"></span>
                </div>
                <div class="col-12 my-3">
                    <label class="fw-bold" for="user-email">Email:&nbsp;</label>
                    <span v-text="this.store.loggedUser.email" id="user-email"></span>
                </div>
            </div>
        </div>
    </div>
</template>

<style lang="">
    
</style>