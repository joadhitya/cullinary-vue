<template>
    <div class="hero">
        <!-- DESKTOP -->
        <div class="d-none d-md-block">
            <div class="row mt-4 ">
                <div class="col-md-6">
                    <div class="d-flex h-100">
                        <div class="justify-content-center align-self-center">
                            <h2><strong>Delicoisu</strong><br> Aku Siap siap siap</h2>
                            <p>Ayo madhang ayo madhang</p>
                            <router-link class="btn btn-lg btn-success" to="/foods">
                                <b-icon-arrow-right></b-icon-arrow-right> Pesan
                            </router-link>
                        </div>
                    </div>
                </div>
                <div class="col-md-6">
                    <img src="../assets/images/hero.svg" width="100%" alt="">
                </div>
            </div>
        </div>

        <!-- MOBILE -->
        <div class="d-sm-block d-md-none">
            <div class="row mt-4 ">

                <div class="col-md-6 my-2">
                    <img src="../assets/images/hero.svg" width="100%" alt="">
                </div>
                <div class="col-md-6 my-2">
                    <div class="d-flex h-100">
                        <div class="justify-content-center align-self-center">
                            <h2><strong>Delicoisu</strong><br> Aku Siap siap siap</h2>
                            <p>Ayo madhang ayo madhang</p>
                            <router-link class="btn btn-lg btn-success" to="/">
                                <b-icon-arrow-right></b-icon-arrow-right> Pesan
                            </router-link>
                        </div>
                    </div>
                </div>
            </div>

        </div>
    </div>

</template>

<script>
    export default {
        name: "Hero"
    }
</script>

<style>

</style>