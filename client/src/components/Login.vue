<template>
  <div class="container">
    <div class="row align-content-around mt-5">
      <label class="display-4">Log in</label>
    </div>
    <div class="row justify-content-center">
      <div class="pt-5 pl-5 pr-5 pb-4 bg-white rounded border">
        <form @submit.prevent="">
          <div class="align-content-center">
            <div class="form-group">
              <label class="float-left">E-mail</label>
              <input type="email" class="form-control" id="exampleInputEmail1" v-model="email">
            </div>
            <div class="form-group">
              <label class="float-left">Password</label>
              <input type="password" class="form-control" id="exampleInputPassword1" v-model="password">
            </div>
            <div class="form-group mt-4">
              <button @click="login" class="btn btn-outline-dark" style="width: 47.5%">Sign In</button>
            </div>
            <span class='error text-danger'>{{ error }}</span>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>


<script>
import axios from 'axios';

export default {
  name: 'Login',
  data() {
    return {
      email: '',
      password: '',
      error: ''
    }
  },
  methods: {
    signup() {
      this.$router.push('/signup');
    },
    login() {
      let user = {
        email: this.email,
        password: this.password
      }
      axios.post('/login', user)
        .then(res => {
            if (res.status === 200) {
              localStorage.setItem('token', res.data.token);
              this.$router.push('/username');
              window.location.reload();
            }
          }
          , err => {
            console.log(err.response);
            this.error = err.response.data.title
          })
    }
  }
}
</script>
