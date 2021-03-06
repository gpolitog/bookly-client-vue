<template>
  <div class="toolbar">

    <!-- toggle drawer button (only visible on mobile) -->
    <button
      class="hide-on-drawer-visible"
      @click="$emit('toggleDrawer')">
      <i>menu</i>
    </button>

    <div class="toolbar-content">

      <span class="toolbar-element toolbar-title">
        <router-link
          class="toolbar-title-text"
          to="/"
          style="color: white">
          Bookly
        </router-link>
      </span>

      <!-- toolbar links; only shown on lg sizes -->
      <span class="toolbar-links gt-sm inline" v-if="isLoggedIn">

        <!-- link to Books list page -->
        <router-link
          class="toolbar-element toolbar-link toolbar-link-books"
          active-class="toolbar-link-active"
          :to="{ name: routes.books.list }">
          Books
        </router-link>

        <!-- link to Authors list page -->
        <router-link
          class="toolbar-element toolbar-link toolbar-link-authors"
          active-class="toolbar-link-active"
          :to="{ name: routes.authors.list }">
          Authors
        </router-link>
      </span>

    </div><!-- /toolbar-content -->

    <!-- account/profile button/menu -->
    <button>
      <i>account_circle</i>
      <q-popover ref="popover" anchor="bottom left" self="top left">
        <div class="list item-delimiter highlight">

          <span v-if="isLoggedIn" class="dropdown-logged-in">
            <!-- button to go to "my account" page -->
            <div
              class="item item-link item-link-account"
              @click="handleAccountClick">
              <div class="item-content">
                My Account
              </div>
            </div>

            <!-- button to log out -->
            <div
              class="item item-link item-link-logout"
              @click="handleLogoutClick">
              <div class="item-content">
                Logout
              </div>
            </div>
          </span>
          <span v-else class="dropdown-not-logged-in">
            <!-- button to go to "login" page -->
            <div class="item item-link item-link-login" @click="handleLoginClick">
              <div class="item-content">
                Log In
              </div>
            </div>
            <!-- button to go to "register" page -->
            <div class="item item-link item-link-register" @click="handleRegisterClick">
              <div class="item-content">
                Sign Up
              </div>
            </div>
          </span>

        </div><!-- /list -->
      </q-popover>
    </button><!-- /account/profile button/menu -->

  </div><!-- /toolbar -->
</template>

<script>
import { mapActions, mapGetters } from 'vuex'
import { Loading, Toast } from 'quasar'

import { localUrls, routes } from '../../globals/urls'

export default {
  data () {
    return {
      routes // expose to template
    }
  },

  computed: {
    ...mapGetters([
      'isLoggedIn'
    ])
  },

  methods: {
    handleAccountClick () {
      this.$refs.popover.close()
      this.$router.push(localUrls.account)
    },

    handleRegisterClick () {
      this.$refs.popover.close()
      this.$router.push(localUrls.register)
    },

    handleLoginClick () {
      this.$refs.popover.close()
      this.$router.push(localUrls.login)
    },

    async handleLogoutClick () {
      this.$refs.popover.close()
      this.working = true
      Loading.show({ message: 'Logging out...' })

      await this.logout()
      Toast.create.info('Logged out!')
      this.$router.push(localUrls.login)
      this.working = false
      Loading.hide()
    },

    ...mapActions([
      'logout'
    ])
  }
}
</script>
