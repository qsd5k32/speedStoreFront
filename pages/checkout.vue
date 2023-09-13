<template>
  <v-container fluid>
    <div>
      <h2>{{ $t('shoppingCart') }}</h2>
      <v-row class="mt-4 mb-4" justify="center" align="stretch" style="height: 100%;">
        <v-col cols="12" md="12" lg="12" class="d-flex flex-column">
          <v-card outlined class="flex-grow-1">
            <v-card-text>
              <div v-if="cart.total_items > 0">
                <div class="mb-4">
                  <p>{{ $t('totalItems') }} <span> {{ cart.total_items }} </span></p>
                </div>
                <v-simple-table dense>
                  <tbody>
                  <tr
                    v-for="(item,key) in cart.items"
                    :key="item.name"
                  >
                    <td>
                      <v-layout>
                        <v-img height="80px" max-height="50" max-width="50" width="50px" :src="item.product.images[0]"/>
                        <div>
                          <h3 class="mx-4">
                            {{ item.product.name }}
                          </h3>
                          <span class="mx-4">{{ item.specification }}</span>
                        </div>
                      </v-layout>
                    </td>
                    <td>
                      <div>
                        <v-text-field dense append-outer-icon="mdi-plus" readonly prepend-icon="mdi-minus" flat :items="10" @input="(e) => increaseQuantity(parseInt(e),key)" :value="item.quantity" style="max-width: 100px;min-width: 100px">
                          <template slot="append-outer">
                            <v-icon @click="changeQuantity({ itemKey:key , quantity: 10, method: 'increase' })" color="primary">mdi-plus-circle-outline</v-icon>
                          </template>
                          <template slot="prepend">
                            <v-icon v-if="item.quantity > 1" @click="changeQuantity({ itemKey:key , quantity: 5 , method: 'decrease' })" color="primary">mdi-minus-circle-outline</v-icon>
                            <v-icon color="red" v-else @click="deleteItem(key)">mdi-delete</v-icon>
                          </template>
                        </v-text-field>
                      </div>
                    </td>
                    <td>
                      <div class="mt-2">
                        <h3 class="mx-6">
                          <span v-if="item.product.reduction">{{ parseFloat(item.quantity * item.product.reduction) }}</span>
                          <span v-else>{{ parseFloat(item.quantity * item.product.price) }}</span>
                          {{ $t('kwd') }}
                        </h3>
                      </div>
                    </td>
                  </tr>
                  <tr>
                    <td><h3>{{ $t('total') }}</h3></td>
                    <td>
                      <div class="mt-2">
                        <h3 class="mx-12">
                          <span>{{ cart.total_items }}</span>
                        </h3>
                      </div>
                    </td>
                    <td>
                      <div class="mt-2">
                        <h3 class="mx-6">
                          <span>{{ cart.total }}</span>
                          {{ $t('kwd') }}
                        </h3>
                      </div>
                    </td>
                  </tr>
                  </tbody>
                </v-simple-table>
              </div>
              <div v-else>
                <h3 class="text-center">{{ $t('noItemFount') }}</h3>
                <v-layout class="mt-4">
                  <v-spacer/>
                  <v-btn nuxt :to="localePath('/')" outlined color="secondary">{{ $t('browsProducts') }}</v-btn>
                  <v-spacer/>
                </v-layout>
              </div>
            </v-card-text>
          </v-card>
        </v-col>
        <v-col cols="12" md="12" lg="12">
          <div>
            <v-card class="mb-4" outlined>
              <v-card-text>
                <v-text-field v-model="coupon" :label="$t('coupon')">
                  <template v-slot:append-outer>
                    <v-btn color="primary" small @click="checkCoupon">{{ $t('submit') }}</v-btn>
                  </template>
                </v-text-field>
                <p>{{ $t('totalItems') }} <span> {{ cart.total_items }} </span></p>
                <v-divider />
                <p v-if="cart.coupon_reduction > 0"><span>{{ $t('couponReduction') }}</span> : <span> {{ parseFloat(cart.coupon_reduction).toFixed(2) }} {{ $t('kwd') }}</span></p>
                <p>{{ $t('amount') }} : <span> {{ parseFloat(cart.total).toFixed(2) }} {{ $t('kwd') }}</span></p>
                <p v-if="cart.fees">{{ $t('tax') }} : <span> {{ parseFloat(cart.fees).toFixed(2) }} {{ $t('kwd') }}</span></p>
                <p>{{ $t('total') }} : <span> {{ calculateTotal }} {{ $t('kwd') }}</span></p>

              </v-card-text>
            </v-card>
            <v-card class="mt-4" outlined v-if="!placeOrder">
              <v-card-text>
                <v-layout>
                  <v-spacer />
                  <v-btn :disabled="cart.total < 1" color="secondary" class="text-capitalize" @click="placeOrder = true">{{ $t('placeOrder') }}</v-btn>
                  <v-spacer />
                </v-layout>
              </v-card-text>
            </v-card>
            <v-card class="mt-4" outlined v-if="placeOrder">
              <v-card-text>
                <place-order :coupon="coupon" v-if="cart.total > 0" />
              </v-card-text>
            </v-card>
          </div>
        </v-col>
      </v-row>
    </div>
  </v-container>
</template>

<script>
import {mapActions, mapGetters} from "vuex";

export default {
  name: "checkout",
  data(){
    return {
      coupon: null,
      quantity: [
        0,1,2
      ],
      placeOrder: false,
    }
  },
  mounted() {
    this.getCart(this.$cookies.get('cart_token'))
  },
  computed:{
    ...mapGetters({
      cart: "cart/getData",
    }),
    calculateTotal()
    {
      let deliveryFee = 0;
      let reduction = 0;
      if(this.cart.coupon_reduction) reduction = parseFloat(this.cart.coupon_reduction)
      let total = (parseFloat(this.cart.total) + deliveryFee + parseFloat(this.cart.fees));// - reduction;
      return (total > 0) ? parseFloat(total).toFixed(2) : 0
    }
  },
  methods: {
    ...mapActions({
      changeQuantity: 'cart/changeQuantity',
      applyCoupon: 'cart/applyCoupon',
      deleteItem: 'cart/deleteItem',
      getCart: 'cart/fetchData'
    }),
    checkCoupon()
    {
      this.applyCoupon({
        cart_token: this.$cookies.get('cart_token'),
        coupon: this.coupon})
    },
    increaseQuantity(value,key)
    {
      if(value > 0) this.changeQuantity({ itemKey:key , quantity: value })
      this.getCart(this.$cookies.get('cart_token'))
    }
  }
}
</script>

<style scoped>

</style>
