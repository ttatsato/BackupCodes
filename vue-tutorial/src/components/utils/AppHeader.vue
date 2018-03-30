<template>
  <div id="app-header">
  <b-navbar toggleable="md" class="navbar-kenken">

    <b-navbar-toggle target="nav_collapse"></b-navbar-toggle>

    <b-navbar-brand href="#">Kenken 2 Proof of Concept</b-navbar-brand>

    <b-collapse is-nav id="nav_collapse">

      <b-navbar-nav>
        <b-nav-item href="#/search">Search Articles</b-nav-item>
        <b-nav-item href="#/users">Users</b-nav-item>
        <!--<b-nav-item href="#" disabled>Disabled</b-nav-item> -->
      </b-navbar-nav>

      <!-- Right aligned nav items -->
      <b-navbar-nav class="ml-auto">

        <b-card style="height: 40px;" :sub-title="myName"></b-card>
        <b-nav-item-dropdown right>
          <!-- Using button-content slot -->
          <template slot="button-content">
            <em>User</em>
          </template>
          <!-- <b-dropdown-item href="#">Profile</b-dropdown-item>
          <b-dropdown-item href="#/password/change">Password Change</b-dropdown-item>
          -->
          <b-dropdown-item-button v-on:click="signOut">Sign Out</b-dropdown-item-button>
        </b-nav-item-dropdown>

        <b-nav-item-dropdown text="System" right>
          <b-dropdown-item href="#">This service version is v{{ appVersion }}</b-dropdown-item>
        </b-nav-item-dropdown>

      </b-navbar-nav>

    </b-collapse>
  </b-navbar>
  </div>
</template>

<script>
import ApiClient from './ApiClient'
import Router from 'vue-router'

export default {
  name: 'SignOut',
  data () {
    return {
      appVersion: '0.0.0',
      myName: null
    }
  },
  methods: {
    signOut: function () {
      var router = new Router()

      let targetPath = '/signOut'
      ApiClient.search(targetPath, () => {
        router.go('/signIn')
      }, (error) => {
        console.log(error)
        router.go('/signIn')
      })
    }
  },
  created: function () {
    var targetPath = '/api/version-info'

    const self = this
    ApiClient.search(targetPath, (response) => {
      self.appVersion = response.data.version
    }, (error) => {
      console.log(error)
      this.$router.push('/signIn')
    })

    targetPath = '/api/users/me'
    ApiClient.search(targetPath, (response) => {
      self.myName = response.data.user_name
      console.log(response)
    }, (error) => {
      console.log(error)
      this.$router.push('/signIn')
    })
  }
}
</script>

<style>
  .navbar-kenken {
    background-color: #FF80AB;
    opacity: 1.0;
    color: black;
    z-index: 10;
  }
</style>
