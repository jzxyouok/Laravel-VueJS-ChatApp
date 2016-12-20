
<!--
<template>

  <div id="app">
    <img src="./assets/logo.png">
    <hello></hello>
  </div>
</template>
-->
<script>
  import {loginUrl, getHeader, userUrl} from './config'
  import {clientId, clientSecret} from './env'
  export default {
    data () {
      return {
        login: {
          email: 'webdev@gmail.com',
          password: 'password'
        }
      }
    },
    methods: {
      handleLoginFormSubmit () {
        const postData = {
          grant_type: 'password',
          client_id: clientId,
          client_secret: clientSecret,
          username: this.login.email,
          password: this.login.password,
          scope: ''
        }
        const authUser = {}
        this.$http.post(loginUrl, postData)
          .then(response => {
            if (response.status === 200) {
              console.log('Oauth token', response)
              authUser.access_token = response.data.access_token
              authUser.refresh_token = response.data.access_token
              window.localStorage.setItem('authUser', JSON.stringify(authUser))
              this.$http.get(userUrl, {headers: getHeader()})
                .then(response => {
                  console.log('user object', response)
                  authUser.email = response.body.email
                  authUser.name = response.body.name
                  window.localStorage.setItem('authUser', JSON.stringify(authUser))
                  this.$router.push({name: 'dashboard'})
                })
            }
          })
      }
    }
  }
</script>

<template>
<div class="wrapper" id="home-wrapper">
  <section class="login">
    <div class="row">
      <div class="col-md-6 col-md-push-3">
        <div class="panel panel-default">
          <div class="panel-heading"><strong>Login</strong></div>
          <div class="panel-body">
            <form v-on:submit.prevent="handleLoginFormSubmit()">
              <div class="form-group">
                <label>Email address</label>
                <input type="text" class="form-control" placeholder="Enter your email address" v-model="login.email">
              </div>
              <div class="form-group">
                <label>Password</label>
                <input type="password" class="form-control" placeholder="Enter your password" v-model="login.password">
              </div>
              <button class="btn btn-primary">Login</button>
            </form>
          </div>
      </div>
      </div>
    </div>
  </section>
</div>
</template>
<style lang="sass">
  @import './assets/css/bootstrap.css'
</style>


<!--
<script>
import Hello from './components/Hello'

export default {
  name: 'app',
  components: {
    Hello
  },
  created () {
    const postData = {
      grant_type: 'password',
      client_id: 2,
      client_secret: 'eVoW9Us93n5dRXUfPD14uCwdjbZ38Iy4gQnc6tIp',
      username: 'webdev@gmail.com',
      password: 'password',
      scope: ''
    }

    this.$http.post('http://localhost:8000/oauth/token', postData)
      .then(response => {
        console.log(response)
        const header = {
          'Accept': 'application/json',
          'Authorization': 'Bearer ' + response.body.access_token
        }
        this.$http.get('http://localhost:8000/api/user', {headers: header})
          .then(response => {
            console.log(response)
          })
      })
  }
}
</script>
-->
<!--
<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
-->
