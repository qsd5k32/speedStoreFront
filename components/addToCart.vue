<template>
    <div style="width: 100%">
      <v-btn block outlined v-if="details" @click="add" :disabled="outOfStock" large min-width="250px" class="text-capitalize" color="secondary">
        <v-icon>mdi-plus</v-icon>
        {{ $t('add') }}
      </v-btn>

      <v-btn block outlined :disabled="outOfStock"  @click="add" class="text-capitalize" v-else color="secondary">
        <v-icon>mdi-plus</v-icon>
        {{ $t('add') }}
      </v-btn>
    </div>
</template>

<script>
import {mapActions} from "vuex";

export default {
  name: "addToCart",
  props: {
    details: Boolean,
    Product: String,
    Specification: String,
    outOfStock: Boolean
  },
  methods: {
    ...mapActions({
      storeData: 'cart/storeData'
    }),
    add() {
      console.log(this.$auth.user)
      this.storeData({
        product_slug: this.Product,
        specification: this.Specification,
        quantity: 1,
        token: this.$cookies.get('cart_token') ?? null,
        user_id: this.$auth.loggedIn ? this.$auth.user.id : null
      });
    }
  }
}
</script>

<style scoped>

</style>
