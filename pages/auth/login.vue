<!--
  - Copyright (c) 2020.
  - Author:  Belgacemi mohamed seddik
  - github:  http://github.com/seddik32
  - facebook: https://www.facebook.com/Qsd5k
  - email: sed75dik@gmail.com
  - Last modified: 6/12/20, 1:21 PM
  -->

<template>
  <v-container>
    <v-tabs
      v-model="tab"
      color="primary"
      grow
    >
      <v-tab
        v-for="item in items"
        :key="item"
      >
        {{ item }}
      </v-tab>
    </v-tabs>

    <v-tabs-items v-model="tab">
      <v-tab-item
        :key="'login'"
      >
        <login-card />
      </v-tab-item>
      <v-tab-item
        :key="'register'"
      >
        <register-card/>
      </v-tab-item>
    </v-tabs-items>
  </v-container>
</template>

<script>
    import LoginCard from "../../components/loginCard";
    export default {
      name: 'Login',
      components: {LoginCard},
      middleware: 'guest',
      data() {
        return {
          loading: false,
          valid: false,
          notAuthenticate: false,
          fields: {
            username: '',
            password: '',
            client_id: '2',
            client_secret: 'K6Vrs6deHbwsN1Bvo2MDU00aLWIP2E4Il8Y0fCIc',
            grant_type: 'password'
          },
          tab: null,
          items: [
            'Login', 'Register'
          ],
        }
      },
      head() {
        return {
          title: 'login',
        }
      },
      mounted() {
        console.log(this.$auth.loggedIn)
      },
      methods: {
        login() {

          try {

            this.$auth
              .loginWith('local', {
                data: this.fields
              })
              .then((res) => {
                console.log(res)
                console.log(this.$auth)
                console.log(this.$auth.user)
                //this.$router.push('/')
              }).catch(e => {
              console.error(e)
              this.error = true
            }).finally(() => {

            });
          } catch (e) {
             this.$router.push('/login')
          }
        },
    }
  }
</script>

<style scoped></style>
