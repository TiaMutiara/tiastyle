<template>
  <div>
    <h1>Products</h1>
        <div class="text-muted" v-if="loading">
      <ClimbingBoxLoader />
    </div>
    <div class="row">
      <div class="col-md-3" v-for="produk in products" :key="produk.id">
        <div class="card mb-4">
          <div class="card-header">
            <img :src="produk.foto" width="100%" class="img-thumb">
          </div>
          <div class="card-body">
            <h4>{{ produk.nama }}</h4>
            <h4>Rp{{ produk.harga }}</h4>
            <a v-if="produk.stok > 0" :href="produk.link_eksternal" target="_blank" class="btn btn-danger btn-block">beli</a>
            <a v-else href="#"  class="disabled btn btn-dark btn-block">stok habis bestie!</a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ClimbingBoxLoader } from '@saeris/vue-spinners'

export default {
 components : {
   ClimbingBoxLoader
 },
 data() {
    return {
      products: "",
      loading : true,
    };
  },
  mounted() {
    this.getProduk();
  },
  methods: {
    async getProduk() {
      let { data, error } = await this.$supabase.from("tb_produk")
        .select(), .order('stok', {ascending : false})

      if (data) {
         this.products = data;
         this.loading = false; // sembunyikan tulisan loading.. :D
      }
      if (error) console.error(error);
    },
  },
};
</script>

<style lang="scss" scoped></style>
