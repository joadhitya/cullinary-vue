<template>
    <div class="keranjang">
        <Navbar :updateKeranjang="keranjangs" />
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
                            <li class="breadcrumb-item active" aria-current="page">Keranjang</li>
                        </ol>
                    </nav>
                </div>
            </div>

            <div class="row">
                <div class="col">
                    <h2>
                        Keranjang
                        <strong>Saya</strong>
                    </h2>
                    <div class="table-responsive mt-3">
                        <table class="table">
                            <thead>
                                <tr>
                                    <th scope="col">#</th>
                                    <th scope="col">Gambar</th>
                                    <th scope="col">Makanan</th>
                                    <th scope="col">Keterangan</th>
                                    <th scope="col">Jumlah</th>
                                    <th scope="col">Harga</th>
                                    <th scope="col">Total Harga</th>
                                    <th scope="col">Aksi</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(keranjang, index) in keranjangs" :key="keranjang.id">
                                    <th>{{index + 1}}</th>
                                    <td>
                                        <img :src="'../assets/images/'+keranjang.products.gambar" width="50px"
                                            class="img-fluid shadow" alt />
                                    </td>
                                    <td>
                                        <strong>{{keranjang.products.nama}}</strong>
                                    </td>
                                    <td>
                                        <strong>{{keranjang.keterangan ? keranjang.keterangan : "-"}}</strong>
                                    </td>
                                    <td>
                                        <strong>{{keranjang.jumlah_pemesanan}}</strong>
                                    </td>
                                    <td align="right">Rp. {{keranjang.products.harga}}</td>
                                    <td align="right">
                                        <strong>
                                            Rp.
                                            {{keranjang.products.harga * keranjang.jumlah_pemesanan}}
                                        </strong>
                                    </td>
                                    <td align="center" class="text-danger" style="cursor:pointer">
                                        <b-icon-trash @click="hapusKeranjang(keranjang.id)"></b-icon-trash>
                                    </td>
                                </tr>
                                <tr>
                                    <td colspan="6" align="right">
                                        <strong>Total Pemesanan</strong>:
                                    </td>
                                    <td align="right">
                                        <strong>Rp. {{totalHarga}}</strong>
                                    </td>
                                    <td></td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>

            <!-- Form checkout -->
            <div class="row justify-content-end">
                <div class="col-md-4">
                    <form action v-on:submit.prevent>
                        <div class="form-group">
                            <label for="nama">Nama Pelanggan</label>
                            <input type="text" class="form-control" v-model="pesan.nama" />
                        </div>
                        <div class="form-group">
                            <label for="noMeja">Nomor Meja</label>
                            <input type="number" class="form-control" v-model="pesan.noMeja" />
                        </div>
                        <button @click="checkout" type="submit" class="btn btn-success btn-block">
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
        name: "Keranjang",
        components: {
            Navbar,
        },
        data() {
            return {
                keranjangs: [],
                pesan: {}
            };
        },
        methods: {
            setKeranjang(data) {
                this.keranjangs = data;
            },
            hapusKeranjang(id) {
                axios
                    .delete("http://localhost:3000/keranjangs/" + id)
                    .then(() => {
                        this.$toast.error("Sukses hapus data makanan", {
                            type: "error",
                            position: "top-right",
                            duration: 3000,
                        });
                        axios
                            .get("http://localhost:3000/keranjangs")
                            .then((response) => this.setKeranjang(response.data))
                            .catch((error) => console.log(error));
                    })
                    .catch((error) => console.log(error));
            },
            checkout() {
                if (this.pesan.nama && this.pesan.noMeja) {
                    this.keranjangs.map(function (item) {
                       axios
                            .delete("http://localhost:3000/keranjangs/" + item.id)
                            .catch((error) => console.log(error));
                    })

                    this.pesan.keranjangs = this.keranjangs;
                    axios
                        .post("http://localhost:3000/pesanans/", this.pesan)
                        .then(() => {
                            this.$toast.success('Berhasil memesan makanan.', {
                                type: 'success',
                                position: 'top-right',
                                duration: 3000
                            })

                            this.$router.push({
                                path: "/pesanan-sukses"
                            })
                        })
                        .catch((err) => console.log(err));

                    console.log("Pesan", this.pesan)
                } else {
                    this.$toast.error("Silahkan lengkapi data", {
                        type: "error",
                        position: "top-right",
                        duration: 3000,
                    });
                }
            }
        },
        mounted() {
            axios
                .get("http://localhost:3000/keranjangs")
                .then((response) => this.setKeranjang(response.data))
                .catch((error) => console.log(error));
        },
        computed: {
            totalHarga() {
                return this.keranjangs.reduce(function (items, data) {
                    return items + data.products.harga * data.jumlah_pemesanan;
                }, 0);
            },
        },
    };
</script>

<style>
</style>