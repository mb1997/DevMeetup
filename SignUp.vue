<template>
    <v-container>
    <v-layout row v-if="error">
        <v-flex xs12 sm6 offset-sm3>
            <app-alert @dismissed="onDismissed" :text="error.message"></app-alert>
        </v-flex>
    </v-layout>
        <v-layout>
            <v-flex xs12 sm6 offset-sm3>
              <v-card>
                <v-card-text>
                    <form @submit.prevent="onSignup">
                       <v-layout>
                           <v-flex xs12 sm6 offset-sm3>
                            <v-text-field
                                name = "email"
                                label = "Mail"
                                id = "email"
                                v-model = "email"
                                type="email"
                                required>
                            </v-text-field>
                           </v-flex>
                       </v-layout>
                       <v-layout>
                           <v-flex xs12 sm6 offset-sm3>
                            <v-text-field
                                name = "password"
                                label = "password"
                                id = "password"
                                v-model = "password"
                                type="password"
                                required>
                            </v-text-field>
                           </v-flex>
                       </v-layout>
                       <v-layout>
                           <v-flex xs12 sm6 offset-sm3>
                            <v-text-field
                                name = "Confirmpassword"
                                label = "Confirmpassword"
                                id = "Confirmpassword"
                                v-model = "Confirmpassword"
                                type="password"
                                :rules="[PasswordIsvalid]">
                            </v-text-field>
                           </v-flex>
                       </v-layout>
                       <v-layout>
                           <v-flex xs12>
                            <v-btn type="submit" :disabled="loading" :loading="loading">
                                SignUp 
                                <span slot="loader" class="custom-loader">
                                <v-icon light>cached</v-icon>
                                </span>
                            </v-btn>
                           </v-flex>
                       </v-layout>
                    </form>
                </v-card-text>
              </v-card>
            </v-flex>
        </v-layout>
    </v-container>
</template>

<script>
    export default {
      data () {
        return {
          email: '',
          password: '',
          Confirmpassword: ''
        }
      },
      computed: {
        PasswordIsvalid () {
          return this.password !== this.Confirmpassword ? 'Passwords do not match' : ''
        },
        user () {
          return this.$store.getters.user
        },
        error () {
          return this.$store.getters.error
        },
        loading () {
          return this.$store.getters.loading
        }
      },
      watch: {
        user (value) {
          if (value !== null && value !== undefined) {
            this.$router.push('/')
          }
        }
      },
      methods: {
        onSignup () {
          this.$store.dispatch('signUserUp', {email: this.email, password: this.password})
        },
        onDismissed () {
          this.$store.dispatch('clearError')
        }
      }
    }
</script>