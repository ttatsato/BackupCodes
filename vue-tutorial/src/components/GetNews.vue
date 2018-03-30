<template>
  <div id="app-2">
  <span v-bind:title="message">
    Hover your mouse over me for a few seconds
    to see my dynamically bound title!
  </span>
</div>
</template>

<script>
import ApiClient from './utils/ApiClient'
// import AppHeader from './utils//AppHeader'

export default {
  data () {
    return {
      users: [],
      fields: {
        id: {label: '-', sortable: false},
        email: {label: 'email', sortable: true},
        role: {label: 'role', sortable: true},
        update: {label: ' ', sortable: true}
      },
      form: {
        uid: '',
        email: '',
        role: ''
      }
    }
  },
  // components: { AppHeader },
  methods: {
    blurHandler: function (role) {
      if (this.editableRow === false) {
        this.editableRow = true
      } else {
        console.log(role)
        this.editableRow = false
      }
    },
    find: function (id) {
      ApiClient.find('/api/users/', id, (response) => {
        console.log(response.data)
      }, (error) => {
        console.log(error)
      })
    },
    reSearch: function () {
      let targetPath = '/api/users'

      ApiClient.search(targetPath, (response) => {
        this.users = response.data.map(function (user) {
          user.editable = false
          user.changed = false
          user.options = [
            { value: user.role, text: user.role },
            { value: (user.role === 'admin') ? 'normal' : 'admin', text: (user.role === 'admin') ? 'normal' : 'admin' }
          ]
          user.selected = user.role
          return user
        })
      }, (error) => {
        console.log(error)
        this.$router.push('/signIn')
      })
    },
    update: function () {
      let params = {
        id: Number(this.form.uid),
        role: this.form.role
      }

      ApiClient.update('/api/users', params, (response) => {
        console.log(response)
        this.reSearch()
      }, (error) => {
        console.log(error)
      })
    },
    create: function () {
      let params = {
        id: 0, // dummy value
        name: this.form.name,
        kind: this.form.email
      }

      ApiClient.create('/api/users', params, (response) => {
        this.reSearch()
      }, (error) => {
        console.log(error)
      })
    },
    destroy: function () {
      ApiClient.destroy('/api/users', this.form.uid, (response) => {
        this.reSearch()
        this.addToggle = false
      }, (error) => {
        console.log(error)
      })
    }
  },
  created: function () {
    this.reSearch()
  }
}
</script>
