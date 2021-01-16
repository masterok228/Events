<template>
  <div class="container">
    <div class="row align-content-around mt-5">
      <label class="display-4">Profile</label>
    </div>
    <div class="row justify-content-center">
      <div class="pt-4 pr-5 pb-2 bg-white rounded border">
          <form  @submit.prevent="">
            <div class="form-group" style="display: inline-block;vertical-align: top;">
              <img class="rounded-circle border" :src="require('@/assets/img/'+img)" style="width: 180px;"><br>
              <input type="file" name="image_uploads" style="cursor: pointer;"@change="onAttachmentChange"><br>
              <button class="btn btn-outline-dark" style="margin-top: -37px; width: 47.5%; ">Upload</button><br>
              <button @click="delete_img" class="btn btn-outline-dark" style="width: 47.5%;">Delete</button>
            </div>
            <div  class="form-group d-inline-block align-top" >
              <div class="form-group">
                <label style="float: left;">First Name</label>
                <input type="text" class="form-control" id="exampleInputEmail1"  v-model="fname">
              </div>
              <div class="form-group">
                <label  style="float: left;">Last Name</label>
                <input type="text" class="form-control" id="exampleInputEmail1" v-model="lname">
              </div>
              <div class="form-group">
                <label  style="float: left;">Country</label>
                <input type="text" class="form-control" id="exampleInputEmail1"  v-model="country">
              </div>
              <div class="form-group">
                <label  style="float: left;">About</label>
                <textarea  class="form-control" v-model="about"/>
              </div>
              <button @click="save" class="btn btn-outline-dark mr-1" style="width: 47.5%;">Ok</button>
              <button @click="cancel" class="btn btn-outline-dark ml-1" style="width: 47.5%;">Cancel</button><br>
              <span class='success' style="color: green;">{{success}}</span>
            </div>
          </form>
        </div>
    </div>
</div>
</template>
<script>
import axios from 'axios';
export default {
  name: 'Signup',
  data() {
    return {
      fname: '',
      lname: '',
      country: '',
      img: 'default.jpg',
      about: '',
      pin: '',
      success: '',
    }
  },
  mounted() {
    let input = document.querySelector('input');
    input.style.opacity = 0;

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

    onAttachmentChange (e) {
        axios.put(`/updateImg`,{ img: e.target.files[0].name, token: localStorage.getItem('token')})
        .then((res) =>
        {
          this.img=e.target.files[0].name;
        })
    },
    delete_img(){
        axios.put(`/updateImg`,{ img: 'default.jpg', token: localStorage.getItem('token')})
        .then((res) =>
        {
          this.img='default.jpg';
        })
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
