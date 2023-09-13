<template>
  <v-app class="overflow-hidden">
    <v-main class="pa-0 overflow-hidden">
      <toast-notification />
      <v-row class="ma-0">
        <v-col class="pa-0" md="5" cols="12">
          <div v-if="$route.name !== 'index___en' && $route.name !== 'index___ar'">
            <v-toolbar flat dense color="white">
              <v-btn @click="$router.back()" icon><v-icon>mdi-arrow-left</v-icon></v-btn>
            </v-toolbar>
            <v-divider />
          </div>
          <Nuxt />
        </v-col>
        <v-col cols="12" class="pa-0" md="7">
          <v-parallax dark width="100%" src="/background.jpg" :height="pageHeight + 'px'">
            <v-card color="transparent" flat :height="pageHeight + 'px'" width="100%">
              <v-toolbar
                width="100%"
                flat
                color="transparent"
              >
                <v-btn @click="rightDrawer = !rightDrawer" class="mx-2" color="white" light fab small>
                  <v-icon>mdi-menu</v-icon>
                </v-btn>
                <v-btn
                  color="white"
                  fab
                  small
                  class="mx-2"
                  light
                  nuxt
                  to="/checkout"

                >
                  <v-icon>mdi-shopping-outline</v-icon>
                </v-btn>
                <v-btn
                  color="white"
                  fab
                  small
                  class="mx-2"
                  light
                  nuxt
                  to="/auth/login"
                  v-if="!$auth.loggedIn"

                >
                  <v-icon>mdi-account-circle</v-icon>
                </v-btn>
                <profile-menu v-if="$auth.loggedIn" />
              </v-toolbar>
            </v-card>
          </v-parallax>
        </v-col>
      </v-row>
    </v-main>
    <v-navigation-drawer
      v-model="rightDrawer"
      :left="!this.$vuetify.rtl"
      :right="this.$vuetify.rtl"
      temporary
      app
    >
      <v-card flat>
        <v-card-text class="text-capitalize">
          <v-list-item nuxt :to="localePath('/')">
            <v-list-item-content>{{ $t('home') }}</v-list-item-content>
          </v-list-item>
          <v-list-item nuxt :to="localePath('/search')">
            <v-list-item-content>{{ $t('shop') }}</v-list-item-content>
          </v-list-item>
          <v-divider />
          <v-list-item nuxt :to="localePath('/page/about-us')">
            <v-list-item-content>{{ $t('about_us') }}</v-list-item-content>
          </v-list-item>
          <v-list-item nuxt :to="localePath('/contact')">
            <v-list-item-content>{{ $t('contactUs') }}</v-list-item-content>
          </v-list-item>
        </v-card-text>
      </v-card>
    </v-navigation-drawer>
    <v-dialog v-model="loginDialog" v-if="!$auth.loggedIn" max-width="700">
      <v-card>
        <login-card v-if="!registerCard" />
        <register-card :login-dialog="loginDialog" v-else />
        <v-card-text>
          <v-btn v-if="!registerCard" text @click="registerCard = true">{{
              $t('auth.register')
            }}
          </v-btn>
          <v-btn v-else text @click="registerCard = false">{{
              $t('auth.login')
            }}
          </v-btn>
        </v-card-text>

      </v-card>
    </v-dialog>
  </v-app>
</template>

<script>
import {mapActions, mapGetters} from "vuex";

export default {
  name: 'DefaultLayout',
  data () {
    return {
      clipped: false,
      drawer: false,
      loginDialog: false,
      registerCard: false,
      shopping_cart: false,
      fixed: false,
      query: null,
      items: [
        {
          icon: 'mdi-apps',
          title: 'Welcome',
          to: '/'
        },
        {
          icon: 'mdi-chart-bubble',
          title: 'Inspire',
          to: '/inspire'
        }
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'بالجملة'
    }
  },
  computed:{
    pageHeight()
    {
      return window.innerHeight
    },
    ...mapGetters({

    })
  },
  mounted() {

  },
  methods: {
    ...mapActions({

    }),
    search()
    {
      this.$router.push(this.localePath('/search?query=' + this.query))
    }
  }
}
</script>
<style>
body,html {
  overflow: hidden !important;
  background: lightgrey;
}
input[type="password"]
{
  font-family: auto;
}
</style>
