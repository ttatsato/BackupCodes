<script>
import axios from 'axios'
axios.defaults.xsrfHeaderName = 'Csrf-Token'
axios.defaults.xsrfCookieName = 'PLAY_CSRF_TOKEN'
axios.defaults.withCredentials = true

const baseUrl = process.env.api_base_url

export default {
  data () {
    return {}
  },
  find: function (path, id, callback, errorHandler) {
    let targetPath = baseUrl + path + id

    axios.get(targetPath).then((response) => {
      callback(response)
    }).catch(function (error) {
      errorHandler(error)
    })
  },
  search: function (pathWithQuery, callback, errorHandler, params) {
    let targetPath = baseUrl + pathWithQuery

    axios.get(targetPath, params).then((response) => {
      callback(response)
    }).catch(function (error) {
      errorHandler(error)
    })
  },
  update: function (path, params, callback, errorHandler) {
    let targetPath = baseUrl + path

    axios.put(targetPath, params).then((response) => {
      callback(response)
    }).catch(function (error) {
      errorHandler(error)
    })
  },
  create: function (path, params, appendConfig, callback, errorHandler) {
    let targetPath = baseUrl + path

    axios.post(targetPath, params, appendConfig).then((response) => {
      callback(response)
    }).catch(function (error) {
      errorHandler(error)
    })
  },
  destroy: function (path, id, callback, errorHandler) {
    let targetPath = baseUrl + path + '/' + id

    axios.delete(targetPath).then((response) => {
      callback(response)
    }).catch(function (error) {
      errorHandler(error)
    })
  }
}
</script>
