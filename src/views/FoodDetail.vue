<template>
    <div class="food-detail">
        <Navbar />
        <div class="container">
            <div class="row mt-4">
                <div class="col">
                    <nav aria-label="breadcrumb">
                        <ol class="breadcrumb">
                            <li class="breadcrumb-item">
                                <router-link class="text-dark" to="/">Home</router-link>
                            </li>
                            <li class="breadcrumb-item">
                                <router-link class="text-dark" to="/foods">Foods</router-link>
                            </li>
                            <li class="breadcrumb-item active" aria-current="page">Food Order</li>
                        </ol>
                    </nav>
                </div>
            </div>
            <div class="row mt-3">
                <div class="col-md-6">
                    <img :src="'../assets/images/'+product.gambar" class="img-fluid shadow" alt />
                </div>
                <div class="col--md-6">
                    <h2>
                        <strong>Nama Makanan :</strong>
                        {{product.nama}}
                    </h2>
                    <h4>
                        Harga :
                        <strong>{{product.harga}}</strong>
                    </h4>
                    <form action v-on:submit.prevent>
                        <div class="form-group">
                            <label for>Jumlah Pesan</label>
                            <input type="number" class="form-control" v-model="pesan.jumlah_pemesanan" />
                        </div>
                        <div class="form-group">
                            <label for>Keterangan</label>
                            <textarea v-model="pesan.keterangan" name id placeholder="Keterangan...." rows="5"
                                class="form-control"></textarea>
                        </div>
                        <button @click="pemesanan" type="submit" class="btn btn-success btn-block">
                            <b-icon-cart></b-icon-cart>Pesan
                        </button>
                    </form>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import Navbar from "@/components/Navbar.vue";
    import axios from "axios";
    export default {
        name: "FoodDetail",
        components: {
            Navbar,
        },
        data() {
            return {
                product: {},
                pesan: {},
            };
        },
        methods: {
            setProduct(data) {
                this.product = data;
            },
            pemesanan() {
                if (this.pesan.jumlah_pemesanan) {
                    this.pesan.products = this.product;
                    axios
                        .post("http://localhost:3000/keranjangs/", this.pesan)
                        .then(() => {
                            this.$toast.success('Berhasil menambah ke keranjang.', {
                                type: 'success',
                                position: 'top-right',
                                duration: 3000
                            })

                            this.$router.push({path: "/keranjang"})
                        })
                        .catch((err) => console.log(err));
                } else {
                    this.$toast.error('Wajib mengisi jumlah pesanan', {
                        type: 'error',
                        position: 'top-right',
                        duration: 3000
                    })
                }
            },
        },
        mounted() {
            axios
                .get("http://localhost:3000/products/" + this.$route.params.id)
                .then((response) => this.setProduct(response.data))
                .catch((error) => console.log(error));
        },
    };
</script>

<style>
</style>