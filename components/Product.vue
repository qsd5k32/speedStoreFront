<template>
  <div>
    <v-hover
      v-slot="{ hover }"
    >
      <v-card rounded flat width="100%" class="mb-4 mt-4 transition-swing">
        <div style="width: 100%">
          <nuxt-link class="text-decoration-none" :to="localePath('/product/' + product.slug)" v-if="product.images">
            <v-img :src="product.images[0]" :height="'170px'" width="100%" >
            </v-img>
          </nuxt-link>
          <v-sheet v-else height="170px" width="100%" color="primary"></v-sheet>
        </div>
        <v-card-text class="mb-0 pb-0">
          <nuxt-link :to="localePath('/product/' + product.slug)" class="text-decoration-none">{{ product.name.substr(0,18) }} <span v-if="product.name.length > 18">...</span></nuxt-link>
          <small class="font-weight-medium text-decoration-line-through" v-if="product.reduction">{{ product.price }} {{ $t('kwd') }}</small>
          <p class="font-weight-bold">
            <span v-if="product.reduction">{{ product.reduction }}</span>
            <span v-if="!product.reduction">{{ product.price }}</span>
            {{ $t('kwd') }}</p>
        </v-card-text>
        <v-card-actions class="mt-0 pt-0">
          <add-to-cart :product="product.slug"/>
        </v-card-actions>
      </v-card>
    </v-hover>
  </div>
</template>

<script>

export default {
  name: "Product",
  methods: {

  },
  props: ['product']
}
</script>

<style scoped>

</style>
