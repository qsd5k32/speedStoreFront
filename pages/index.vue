<template>
  <div class="pa-0 ma-0">
    <v-card flat class="ma-0">
      <v-list-item class="pa-4" nuxt to="/contact">
        <v-list-item-avatar class="mt-0" size="55" tile color="primary"></v-list-item-avatar>
        <v-list-item-content class="pt-0">
          <v-list-item-title class="font-weight-bold">Store Name</v-list-item-title>
          <v-list-item-subtitle>
            <v-icon color="green">mdi-cash</v-icon>
            <v-icon color="green">mdi-credit-card</v-icon>
            <span>Min order: 5KD</span>
          </v-list-item-subtitle>
        </v-list-item-content>
        <v-list-item-action>
          <v-btn icon color="primray">
            <v-icon>mdi-information-outline</v-icon>
          </v-btn>
        </v-list-item-action>
      </v-list-item>
      <v-divider />
      <v-layout justify-space-around class="mt-6 mb-6">

        <v-btn color="primary" class="text-capitalize">Delivery</v-btn>
        <v-btn color="primary" outlined class="text-capitalize">pick up</v-btn>
      </v-layout>
      <v-divider />
      <v-list>
        <v-list-item>
          <v-list-item-icon>
            <v-icon>mdi-motorbike</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-subtitle>Pick up from</v-list-item-subtitle>
          </v-list-item-content>
          <v-list-item-action>
            <v-btn text tile class="text-capitalize">edit</v-btn>
          </v-list-item-action>
        </v-list-item>
        <v-list-item>
          <v-list-item-icon>
            <v-icon>mdi-clock-outline</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-subtitle>Pick up from</v-list-item-subtitle>
          </v-list-item-content>
          <v-list-item-action>
            <v-btn text tile class="text-capitalize">edit</v-btn>
          </v-list-item-action>
        </v-list-item>
      </v-list>
    </v-card>
    <v-card flat>
      <v-card-text>
        <v-row>
          <v-col cols="12" md="6" v-for="category in categories">
            <v-card nuxt :to="'/category/' + category.slug" >
              <v-img :src="category.image" max-height="250px" width="100%">
                <v-overlay absolute>
                  <span class="font-weight-bold text-capitalize" style="font-size: 18px">{{ category.name }}</span>
                </v-overlay>
              </v-img>
            </v-card>
          </v-col>
        </v-row>
      </v-card-text>
    </v-card>
  </div>
</template>

<script>

import {mapActions, mapGetters} from "vuex";

export default {
  name: 'IndexPage',
  components: {},
  computed:{
    ...mapGetters({
      categories: "categories/getData"
    }),
    fullHeight()
    {
      return window.innerHeight - 120;
    }
  },
  data () {
    return {
      products: null,
      cycle: false,
      slides: [
        'First',
        'Second',
        'Third',
        'Fourth',
        'Fifth',
      ],
    }
  },
  mounted() {
    this.getCategories()
  },
  methods: {
    ...mapActions({
      getCategories: "categories/fetchData"
    }),
  }
}
</script>
