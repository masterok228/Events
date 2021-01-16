<template>
  <div class="container">
    <div class="row align-content-around mt-5">
      <label class="display-4">My profile</label>
    </div>
    <div class="row justify-content-center">
      <div class="p-5 bg-white rounded border">
          <form  @submit.prevent="">
            <div class="form-group align-top" style="display: inline-block;vertical-align: top;">
              <img  class="rounded-circle border" :src="require('@/assets/img/'+img)" style="width: 200px;"><br>
              <input type="file" name="image_uploads" style="cursor: pointer;"@change="onAttachmentChange"><br>
              <button class="btn btn-outline-dark ml-1" style="width: 47.5%; margin-top: -10px;">Upload</button><br>
              <button @click="delete_img" class="btn btn-outline-dark ml-1" style="width: 47.5%; margin-top: 10px;">Delete</button>
            </div>
            <div style="display: inline-block;text-align: right;margin-left: 20px;">
              <div class="rounded border">
                <div class="rounded border bg-dark text-light text-left pl-2">
                  <b>Login Information</b>
                  <img :src="require('@/assets/edit.png')" style="width: 22px; padding:2px; float: right;margin-right: 4px;cursor: pointer;"
                    @click="edit_login"><br>
                </div>
                <div style="width: 448px;padding: 13px 13px 2px 13px;">
                  <div style="text-align: initial;">
                    <label style="padding-right: 9px;"><b>E-mail: </b></label>{{email}}<br>
                    <label style="padding-right: 9px;"><b>Password: </b></label>{{password}}
                  </div>
                </div>
              </div>
              <div class="rounded border" style="margin-top: 20px;border: 1px solid black;">
                <div style="text-align: justify;padding-left: 13px;color: white;background-color: rgb(49 55 62);">
                  <b>Profile Information</b>
                  <img :src="require('@/assets/edit.png')" style="width: 22px; padding:2px; float: right;margin-right: 4px;cursor: pointer;"
                   @click="edit_profile"><br>
                </div>
                <div style="width: 448px;padding: 13px 13px 2px 13px;">
                  <div style="text-align: initial;">
                    <label style="padding-right: 9px;"><b>First Name: </b></label>{{fname}}<br>
                    <label style="padding-right: 9px;"><b>Last Name: </b></label>{{lname}}<br>
                    <label style="padding-right: 9px;"><b>Country: </b></label>{{country}}<br>
                    <label style="padding-right: 9px;"><b>About: </b></label>{{about}}
                  </div>
                </div>
              </div>
              <div class="rounded border" style="margin-top: 20px;border: 1px solid black;">
                <div style="text-align: justify;padding-left: 13px;color: white;background-color: rgb(49 55 62);">
                  <b>Personal Identification Number</b>
                  <button @click="edit_pin" class="btn btn-outline-warning mr-2" style="margin-top: 28px; margin-bottom:5px; float: right;width: 25%;">SET PIN</button><br>
                </div>
                <div style="width: 448px;padding: 13px 13px 2px 13px;">
                  <div style="text-align: initial;">
                    <label style="padding-right: 9px;"><b>Pin: </b></label>{{pin}}
                  </div>
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
  name: 'Username',
  data() {
    return {
      fname: '',
      lname: '',
      country: '',
      email: '',
      password: '********',
      rep_password: '',
      img: "default.jpg",
      about: '',
      pin: '****',
      error: '',
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
        this.email = res.data.email;
        this.img = res.data.img;
        this.about = res.data.about;
      })
  },
  methods: {
    edit_login(){
        this.$router.push('/username/login');
    },
    edit_profile(){
        this.$router.push('/username/profile');
    },
    edit_pin(){
        this.$router.push('/username/pin');
    },
    logout(){
          localStorage.clear();
          this.$router.push('/login');
          window.location.reload();
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
  }
}
</script>
