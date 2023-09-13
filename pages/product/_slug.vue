<!--
  - Copyright (c) 2020.
  - Author:  Belgacemi mohamed seddik
  - github:  http://github.com/seddik32
  - facebook: https://www.facebook.com/Qsd5k
  - email: sed75dik@gmail.com
  - Last modified: 6/12/20, 1:21 PM
  -->

<template>
  <section>
    <v-snackbar :color="message.color" v-model="message.activate">
      {{ $t(message.text) }}
      <v-btn @click="message.activate = false" color="white" icon>
        <v-icon>mdi-close</v-icon>
      </v-btn>
    </v-snackbar>
    <v-container>
      <div>
        <v-card flat elevation="0" v-if="!loading">
          <v-row>
            <v-col cols="12" md="12">
              <v-chip
                v-if="product.category"
                :to="'/category/' + product.category.slug"
                class="white--text"
                color="primary"
                small
                nuxt
              >{{ product.category.name }}
              </v-chip>
              <v-carousel
                height="250"
                cycle
                delimiter-icon="mdi-minus"
                hide-delimiter-background
                hide-delimiters
                show-arrows-on-hover
                v-model="carousel"
              >
                <v-carousel-item
                  :src="item"
                  v-for="item in product.images"
                />
              </v-carousel>
              <v-slide-group
                center-active
                class="pa-2 mt-2"
                show-arrows
                v-model="carousel"
              >
                <v-slide-item
                  :key="key"
                  v-for="(item, key) in product.images"
                  v-slot:default="{ active, toggle }"
                >
                  <v-card
                    :color="active ? 'blue' : 'grey lighten-1'"
                    @click="toggle"
                    class="ma-1"
                    height="40"
                    width="40"
                  >
                    <v-avatar size="40" tile>
                      <v-img :src="item">
                        <v-overlay absolute v-if="active"/>
                      </v-img>
                    </v-avatar>
                  </v-card>
                </v-slide-item>
              </v-slide-group>
            </v-col>
            <v-col cols="12" md="12">
              <v-card flat height="100%">
                <h3>{{ product.name }}</h3>
<!--                <h3 class="ma-3" v-if="product.reduction">
                <span class="red&#45;&#45;text" style="text-decoration: line-through;"
                >{{ product.price }}{{ $t('kwd') }}</span
                >
                  - {{ product.reduction }} {{ $t('kwd') }}
                </h3>-->
                <h3 class="ma-3" >
                  {{ product.price }} <span>{{ $t('kwd') }}</span>
                </h3>
                <div class="ma-3">
                  <div>{{ $t('quantity') }} :</div>
                  <v-btn @click="minQuantity" color="primary" icon slot="prepend"
                  >
                    <v-icon>mdi-minus-circle-outline</v-icon>
                  </v-btn
                  >
                  {{ quantity }}
                  <v-btn @click="plusQuantity" color="primary" icon slot="append"
                  >
                    <v-icon>mdi-plus-circle-outline</v-icon>
                  </v-btn
                  >
                </div>
                <v-text-field :label="$t('specification')" v-model="specification"/>

                <v-card-actions>
                  <add-to-cart :product="product.slug" :specification="specification" details/>
                </v-card-actions>
              </v-card>
            </v-col>
          </v-row>
        </v-card>

        <v-row v-else>
          <v-col cols="12" md="6">
            <v-skeleton-loader
              class="mx-auto"
              ref="skeleton"
              type="image"
            ></v-skeleton-loader>
          </v-col>
          <v-col cols="12" md="6">
            <v-skeleton-loader type="card-avatar"/>
            <v-skeleton-loader type="article"/>
            <v-skeleton-loader type="actions"/>
          </v-col>
        </v-row>
      </div>
    </v-container>
  </section>
</template>

<script>


  export default {
    name: 'Slug',
    data() {
      return {
        conversationDialog: false,
        product: {},
        specification: null,
        refundDate: '',
        carousel: 0,
        images: [],
        features: [],
        videoLink: null,
        delivery: null,
        productId: this.$route.params.slug,
        loading: true,
        quantity: 1,
        delivery_date: null,
        delivery_time: null,
        message: {
          activate: false,
          message: '',
          color: null
        },
        valid: true
      }
    },
    head() {
      return {
        title: this.product.name,
      }
    },
    mounted() {
      this.$axios.get('product/' + this.productId).then(res => {
        if(res.data.data)
        {
          this.product = res.data.data
          this.loading = false
        }
      })
    },
    methods: {

      addToCart() {
        const product_id = this.product.id
        const quantity = parseInt(this.quantity)
        const options = '{}'

        const specification = this.specification;

        /*
         * this.$store.commit('set_content',res.data);
         * */
        if (this.$auth.loggedIn) {
          if (this.product.stock < 1) {
            this.message = {
              activate: true,
              text: 'out_of_stock',
              color: 'error'
            }
          } else if (this.product.deliveries.length > 0 && !this.delivery) {
            this.message = {
              activate: true,
              text: 'chose_delivery',
              color: 'warning'
            }
          } else {

          }
        } else {
          this.$router.push('/auth/login')
        }
      },
      minQuantity() {
        if (this.quantity > 1) {
          this.quantity--
        }
      },
      plusQuantity() {
        this.quantity++
      },
    }
  }
</script>

<style scoped>
  .h-100 {
    height: 100% !important;
  }
</style>
