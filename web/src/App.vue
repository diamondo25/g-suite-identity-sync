<template>
  <div class="page-container">
    <md-app md-mode="reveal">
      <md-app-toolbar class="md-primary">
        <md-button class="md-icon-button" @click="menuVisible = !menuVisible">
          <md-icon>menu</md-icon>
        </md-button>
        <h2 class="md-title">{{ titleMsg }}</h2>
      </md-app-toolbar>
      <md-app-drawer :md-active.sync="menuVisible">
        <md-toolbar class="md-transparent" md-elevation="0">
          <md-list v-if="$auth.loggedIn" class="md-transparent">
            <md-list-item class="md-avatar-list">
              <md-avatar class="md-large">
                <img v-bind:src="$auth.userInfo.imageUri" alt="People"/>
              </md-avatar>
            </md-list-item>
            <md-list-item>
              <div class="md-list-text-container">
                <span class="md-list-item-text">{{ $auth.userInfo.name }}</span>
                <span class="md-list-item-text">{{ $auth.userInfo.email }}</span>
              </div>
            </md-list-item>
          </md-list>
        </md-toolbar>
        <md-list>
          <md-list-item v-if="!$auth.loggedIn" @click="routeTo('/sign-in')">
            <md-icon>
              <svg style="width:24px;height:24px" viewBox="0 0 24 24">
                <path fill="#000000" d="M19,3H5C3.89,3 3,3.89 3,5V9H5V5H19V19H5V15H3V19A2,2 0 0,0 5,21H19A2,2 0 0,0 21,19V5C21,3.89 20.1,3 19,3M10.08,15.58L11.5,17L16.5,12L11.5,7L10.08,8.41L12.67,11H3V13H12.67L10.08,15.58Z" />
              </svg>
            </md-icon>
            <span class="md-list-item-text">Sign In</span>
          </md-list-item>
          <md-list-item v-if="$auth.loggedIn" @click="routeTo('/')">
            <md-icon>home</md-icon>
            <span class="md-list-item-text">Home</span>
          </md-list-item>
          <md-list-item v-if="isAdmin()" @click="routeTo('/settings')">
            <md-icon>settings</md-icon>
            <span class="md-list-item-text">Settings</span>
          </md-list-item>
          <md-list-item v-if="$auth.loggedIn" @click="routeTo('/about')">
            <md-icon>help</md-icon>
            <span class="md-list-item-text">About</span>
          </md-list-item>
          <md-list-item v-if="$auth.loggedIn" @click="signOut()">
            <md-icon>
              <svg style="width:24px;height:24px" viewBox="0 0 24 24">
                <path fill="#000000" d="M14.08,15.59L16.67,13H7V11H16.67L14.08,8.41L15.5,7L20.5,12L15.5,17L14.08,15.59M19,3A2,2 0 0,1 21,5V9.67L19,7.67V5H5V19H19V16.33L21,14.33V19A2,2 0 0,1 19,21H5C3.89,21 3,20.1 3,19V5C3,3.89 3.89,3 5,3H19Z" />
              </svg>
            </md-icon>
            <span class="md-list-item-text">Sign Out</span>
          </md-list-item>
        </md-list>
      </md-app-drawer>
      <md-app-content>
        <router-view></router-view>
      </md-app-content>
    </md-app>
  </div>
</template>

<script>
export default {
  name: 'app',
  data: () => ({
    menuVisible: false,
    titleMsg: 'Accout Manager'
  }),
  methods: {
    open() {
      console.log('Opened menu')
    },
    close() {
      console.log('Closed menu')
    },
    routeTo(ref) {
      console.info('Routing to ' + ref)
      this.$router.push(ref)
      this.menuVisible = false
    },
    signOut() {
      console.info('Signing out...')
      this.menuVisible = false
      this.$auth.logout()
    },
    isAdmin() {
      return this.$auth.loggedIn && this.$auth.userInfo.amAdmin
    }
  }
}
</script>

<style>
.md-app {
  min-height: 100vh;
}
</style>
