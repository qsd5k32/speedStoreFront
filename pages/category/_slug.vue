<template>
  <div>
    <v-container fluid>
      <v-row wrap>
        <v-col cols="12" md="6" v-for="product in products">
          <product :product="product" />
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
import {mapActions, mapGetters} from "vuex";


export default {
  name: "_slug",
  computed:{
    ...mapGetters({
      products: 'products/getData',
      loading: 'products/getLoading'
    })
  },
  data()
  {
    return {
      subCategories: [],
      subCategoriesValues: [],
    }
  },
  created() {
    this.getProducts(this.$route.params.slug);
  },
  methods: {
    ...mapActions({
      getProducts: 'products/fetchData'
    }),
  }
}
</script>

<style scoped>

</style>
