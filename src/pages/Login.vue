<script>
export default {
    props: {
        users: Array,
    },
    data() {
        return {
            loggedUserId: "",
            error: false,
        }
    },
    methods: {
        selectUserAndEmit() {
            if (this.loggedUserId !== "") {
                this.$emit('getBooks', this.loggedUserId);
            } else {
                this.error = true;
            }
        }
    },
}
</script>

<template>
    <div class="container border rounded-5 shadow my-5">
        <div class="row py-5">
            <div class="col-12 pb-5 px-4">
                <h3>Scegli l'account con cui entrare</h3>
            </div>
            <div class="col-12">
                <div class="row">
                    <div class="col-12 form-check text-center py-5">
                        <select class="form-select mb-5" v-model="this.loggedUserId">
                            <option value="" selected hidden>Seleziona l'account</option>
                            <option v-for="user in users" :key="user.id" :value="user.id" v-text="user.email"></option>
                        </select>
                        <span v-if="error" class="text-danger fw-bold">Devi selezionare l'account</span>
                    </div>
                    <div class="col-12 text-center mt-3">
                        <button type="submit" class="btn btn-primary text-center" @click="selectUserAndEmit">Entra</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<style lang="scss" scoped></style>