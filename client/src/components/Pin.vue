<template>
  <div class="container">
    <div class="row align-content-around mt-5">
      <label class="display-4">PIN</label>
    </div>
    <div class="row justify-content-center">
      <div class="p-5 bg-white rounded border">
          <form  @submit.prevent="">
                <div style="padding: 13px 13px 2px 13px;">
                  <div style="text-align: initial;">
                    <label style="padding-right: 9px;"><b>Enter Pin: </b></label><input type="password" class="form-control"  v-model="pin"><br>
                    <div style=" text-align: right;">
                      <button @click="save" class="btn btn-outline-dark mr-1" style="width: 47%;">Ok</button>
                      <button @click="cancel" class="btn btn-outline-dark ml-1" style="width: 47%;">Cancel</button><br>
                      <span class='success' style="color: green;">{{success}}</span>
                    </div>
                  </div>
                </div>

          </form>
        </div>
    </div>
</div>
</template>
<script>
import axios from 'axios';
export default {
  name: 'UpdateLoginInformation',
  data() {
    return {
      fname: '',
      lname: '',
      country: '',
      img: '',
      about: '',
      pin: '',
      success: '',
    }
  },
  mounted() {
    axios.get('/user', { headers: { token: localStorage.getItem('token')}})
      .then(res => {
        this.fname = res.data.fname;
        this.lname = res.data.lname;
        this.country = res.data.country;
        this.img = res.data.img;
        this.about = res.data.about;
        this.pin = res.data.pin;
      })
  },
  methods: {
    cancel(){
          this.$router.push('/username');
    },
    save(){
        axios.put(`/updateProfile`,{ fname: this.fname, lname: this.lname ,country:this.country, about:this.about, pin:this.pin, token: localStorage.getItem('token')})
        .then((res) => {
          this.$router.push('/username');
            })
    },
  }
}
</script>
