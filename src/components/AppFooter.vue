<script>
export default {

}
</script>

<template>
    <footer>
        <div class="container-fluid bg-black py-4 h-100">
            <div class="row">
                <div class="col-12 text-white text-center">
                    <p>&copy; 2024 BookVault™. All Rights Reserved.</p>
                    <span>Contattaci:&nbsp;</span>
                    <span id="contact-us">info@libreriawebapp.com</span>
                </div>
            </div>
        </div>
    </footer>
</template>
  
<style lang="scss" scoped>
footer {
    position: fixed;
    bottom: 0;
    width: 100%;

    #contact-us {
        font-weight: 600;

        &:hover {
            text-decoration: underline;
            cursor: pointer;
        }
    }
}
</style>