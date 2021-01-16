<template>
  <nav class="navbar navbar-dark  navbar-expand-md navbar-light bg-dark">
    <button
      class="navbar-toggler"
      type="button" data-toggle="collapse"
      data-target="#navbarNav"
      aria-controls="navbarNav"
      aria-expanded="false"
      aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>

    <div class="navbar-collapse collapse" id="navbarNav">

      <ul class="navbar-nav ml-auto">
        <li class="nav-item active bg-warning rounded" v-if="this.flag == false">
          <a class="nav-link" href="/#/events">Events</a>
        </li>

        <li class="nav-item active bg-info ml-1 rounded" v-if="this.flag == false">
          <a class="nav-link" href="/#/documents">Documents</a>
        </li>

        <li class="nav-item active bg-success ml-1 rounded" v-if="this.status == 'admin'">
          <a class="nav-link" href="/#/admin">Admin</a>
        </li>
      </ul>
    </div>

    <div class="navbar-collapse collapse w-100 order-3 dual-collapse2" id="navbarNav1">

      <ul class="navbar-nav ml-auto">

        <li class="nav-item active bg-light rounded" v-if="this.flag == false">
          <a class="nav-link text-dark" href="/#/username">Profile</a>
        </li>

        <li class="nav-item active  bg-light rounded" v-if="this.flag == true">
          <a class="nav-link text-dark" href="/#/login">Sign In</a>
        </li>

        <li class="nav-item active  bg-light rounded ml-1" v-if="this.flag == true">
          <a class="nav-link text-dark" href="/#/signup">Sign Up</a>
        </li>

        <li class="nav-item active bg-light ml-1 rounded" v-if="this.flag == false">
          <a class="nav-link text-dark" style="cursor: pointer" @click="logout" >Logout</a>
        </li>

      </ul>
    </div>

  </nav>
</template>

<script>
import axios from 'axios'
export default {
  data()
  {
    return {
      flag:true,
      status:'',
    }
  },
  mounted() {
    if(localStorage.getItem('token')!=null)
    {
      this.flag=false
      this.hr="/#/calendar";

      axios.get('/user', { headers: { token: localStorage.getItem('token')}})
        .then(res => {
          this.status=res.data.roled
        })
    }
  },
  methods:{
    logout(){
      localStorage.clear();
      this.$router.push('/login');
      window.location.reload();
    },
  }
}
</script>
