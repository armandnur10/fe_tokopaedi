<template>
  <div>
    <a href="http://localhost:3000/gadget" class="btn btn-success mt-3 ml-3">Back</a>
    <div class="container mt-2 ">
        <h1 class="text-right">Ingfo Produk</h1>
      <div class="row mt-5">
        <div class="col-md-6">
      <h2 class="pt-2">{{ items.nama_barang }}</h2>
      <p class="pt-2">{{ items.harga }}</p>
      <p class="pt-2">{{ items.kategori }}</p>
      </div>
      <div class="col-md-6 d-flex justify-content-end">
      <img :src="items.gambar" height="400px" width="400px" class="rounded-circle" alt="">
      </div>
      </div>
      
    </div>
      
  </div>
</template>

<script>
  import axios from 'axios';

  export default {
    name: 'Detail',
    data() {
      return {
        items: [],
        show: true
      };
    },
    methods: {
      async getapi() {
        const data = await this.$axios.$get(`http://localhost:3500/users/gadget/${this.$route.params.id}`);
        this.items = data.data;
        console.log(data.data);
      },
    },

    mounted() {
      this.getapi();
    }

  };

</script>
