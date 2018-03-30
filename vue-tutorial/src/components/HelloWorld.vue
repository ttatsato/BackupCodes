<template>
  <div id="app-2">
    <button v-on:click="find">axios.get</button>
    <p>↓　axiosで取得した結果をここに表示する↓</p>
    <hr>
    <div v-for="item in apiResult">
    <p>{{item.title}}</p>
    <p>{{item.description}}</p>
    <p>{{item}}</p>
    </div>
    <form @submit="signIn()">
      <input id="firstNameGroup"
                    label="First Name:"
                    description="Your First Name">
      <button type="submit" variant="primary">Sign Up</button>
    </form>
</div>
  </div>
</template>

<script>
import axios from 'axios'
axios.defaults.xsrfHeaderName = 'Csrf-Token'
axios.defaults.xsrfCookieName = 'PLAY_CSRF_TOKEN'
axios.defaults.withCredentials = true
const baseUrl = 'https://qiita.com/api/v2/'

export default {
  data () {
    return {
      apiResult : [],
    }
  },
  methods: {
    testfunc: function (event) {
      alert('ssssss')
    },
    reverseMessage: function () {
      this.test = this.test.split('').reverse().join('')
    },
    create: function (path, params, appendConfig, callback, errorHandler) {
      let targetPath = baseUrl + path
      axios.post(targetPath, params, appendConfig).then((response) => {
        callback(response)
      }).catch(function (error) {
        errorHandler(error)
      })
    },
    find: function (path, id, callback, errorHandler) {
      let targetPath =  baseUrl + 'schema'
      axios.get(targetPath)
      .then((response) => {
        console.log(response.data)
        console.log(response.property)
        this.apiResult = response
      }).catch(function (error) {
        console.log(error)
        errorHandler(error)
      })
    },
    signIn: function (event) {
      let params = {
        firstName: this.form.firstName,
        lastName: this.form.lastName,
        email: this.form.email,
        password: this.form.password
      }
      Auth.authenticate('/signUp', params, (response) => {
        this.$router.push('/')
      })
    },
    home: function (callback, errorHandler) {
      Auth.home(callback, errorHandler)
    },
  created: function () {
    this.home((result) => {
      if (result.status === 200) {
        this.$router.push('/')
      } else {
        this.$router.push('/signUp')
      }
    }, (error) => {
      console.log(error)
      this.$router.push('/signUp')
    })
  }
  }
}
</script>
