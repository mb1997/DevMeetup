<template>
  <v-app>
   
    <v-toolbar dark class="secondary">
       <v-navigation-drawer v-model="sideNav">
    <v-list>
      <v-list-tile v-for="item in menuItems" :key="item.title" @click="">
        <v-list-tile-action>
          <v-icon>{{ item.icon }}</v-icon>
        </v-list-tile-action>
       <v-list-tile-content>{{ item.title }}</v-list-tile-content> 
      </v-list-tile>
      <v-list-tile v-if="userIsAuthenticated" @click="OnLogout">
        <v-list-tile-action>
          <v-icon>exit_to_app</v-icon>
        </v-list-tile-action>
       <v-list-tile-content>Log Out</v-list-tile-content> 
      </v-list-tile>
    </v-list>
  </v-navigation-drawer>
      <v-toolbar-side-icon
        @click.stop="sideNav = !sideNav" 
        class="hidden-sm-and-up" ></v-toolbar-side-icon>
      <v-toolbar-title large> <router-link to="/" tag="span" style="cursor: pointor"> DevMeetup </router-link>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-toolbar-items class="hidden-xs-only" >
          <v-btn flat 
          v-for="item in menuItems" :key="item.title"
          :to="item.link">
            <v-icon left dark>{{ item.icon }}</v-icon>
            {{ item.title }}
          </v-btn>
          <v-btn flat v-if="userIsAuthenticated" @click="OnLogout">
            <v-icon left dark>exit_to_app</v-icon>
            Logout
          </v-btn>
      </v-toolbar-items>
    </v-toolbar>
    <main>
      <router-view></router-view>
    </main>
  </v-app>
</template>

<script>
export default {
  data () {
    return {
      sideNav: false
    }
  },
  computed: {
    menuItems () {
      let menuItems = [
        { icon: 'face', title: 'Sign up', link: '/signup' },
        { icon: 'lock_open', title: 'Sign in', link: '/signin' }
      ]
      if (this.userIsAuthenticated) {
        menuItems = [
          { icon: 'supervisor_account', title: 'View Meetups', link: '/meetup' },
          { icon: 'room', title: 'Organize Meetups', link: '/newmeetup' },
          { icon: 'person', title: 'Profile', link: '/profile' }
        ]
      }
      return menuItems
    },
    userIsAuthenticated () {
      return this.$store.getters.user != null && this.$store.getters.user !== undefined
    }
  },
  methods: {
    OnLogout () {
      this.$store.dispatch('logout')
    }
  }
}
</script>
