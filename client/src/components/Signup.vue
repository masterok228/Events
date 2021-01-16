<template>
  <div class="container">
    <div class="row align-content-around mt-5">
      <label class="display-4">Registration</label>
    </div>
    <div class="row justify-content-center">
      <div class="p-5 bg-white rounded border">
        <form @submit.prevent="">
          <div class="form-group d-inline-block align-top">
            <img class="rounded-circle border" :src="require('@/assets/img/'+img)" style="width: 200px;"><br>
            <input type="file" name="image_uploads"
                   @change="onAttachmentChange"><br>
            <button class="btn btn-outline-dark" style="width: 47.5%">Upload</button>
            <br>
            <button @click="delete_img" class="btn btn-outline-dark mt-3" style="width: 47.5%">Delete</button>
          </div>
          <div class="form-group d-inline-block align-top">
            <div class="form-group">
              <label class="float-left">First Name</label>
              <input type="text" class="form-control" id="exampleInputEmail1" v-model="fname">
            </div>
            <div class="form-group">
              <label class="float-left">Last Name</label>
              <input type="text" class="form-control" id="exampleInputEmail1" v-model="lname">
            </div>
            <div class="form-group">
              <label  style="float: left;">Country</label>
              <select onfocus="this.size=5" onblur="this.size=1;" onchange="this.size=1; this.blur();" v-model="country" class="custom-select">
                <option disabled value="n1">selected country</option>
                <option v-for="country in countries">{{country}}</option>
              </select>
            </div>
            <div class="form-group">
              <label class="float-left">E-mail</label>
              <input type="email" class="form-control" id="exampleInputEmail1" v-model="email">
            </div>
            <div class="form-group">
              <label class="float-left">Password</label>
              <input type="password" class="form-control" id="exampleInputPassword1" v-model="password">
            </div>
            <div class="form-group">
              <label class="float-left">Repeat password</label>
              <input type="password" class="form-control" id="exampleInputPassword1" v-model="rep_password">
            </div>
            <div class="form-group mt-4">
              <button @click="signup" class="btn btn-outline-dark mr-1" style="width: 47.5%">OK</button>
              <button @click="cancel" class="btn btn-outline-dark ml-1" style="width: 47.5%">Cancel</button>
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
  name: 'Signup',
  data() {
    return {
      fname: '',
      lname: '',
      country: 'n1',
      email: '',
      password: '',
      rep_password: '',
      img: "default.jpg",
      error: '',
      myFilter: (option, label, search) => {
        let temp = search.toLowerCase();
        return option.toLowerCase().indexOf(temp)
      },
      countries : ['Afghanistan', 'Albania', 'Algeria', 'American Samoa', 'Andorra', 'Angola', 'Anguilla', 'Antigua and Barbuda', 'Argentina', 'Armenia', 'Aruba', 'Australia', 'Austria', 'Azerbaijan', 'Bangladesh', 'Barbados', 'Bahamas', 'Bahrain', 'Belarus', 'Belgium', 'Belize', 'Benin', 'Bermuda', 'Bhutan', 'Bolivia', 'Bosnia and Herzegovina', 'Botswana', 'Brazil', 'British Indian Ocean Territory', 'British Virgin Islands', 'Brunei Darussalam', 'Bulgaria', 'Burkina Faso', 'Burma', 'Burundi', 'Cambodia', 'Cameroon', 'Canada', 'Cape Verde', 'Cayman Islands', 'Central African Republic', 'Chad', 'Chile', 'China', 'Christmas Island', 'Cocos (Keeling) Islands', 'Colombia', 'Comoros', 'Congo-Brazzaville', 'Congo-Kinshasa', 'Cook Islands', 'Costa Rica', 'Croatia', 'Cura?ao', 'Cyprus', 'Czech Republic', 'Denmark', 'Djibouti', 'Dominica', 'Dominican Republic', 'East Timor', 'Ecuador', 'El Salvador', 'Egypt', 'Equatorial Guinea', 'Eritrea', 'Estonia', 'Ethiopia', 'Falkland Islands', 'Faroe Islands', 'Federated States of Micronesia', 'Fiji', 'Finland', 'France', 'French Guiana', 'French Polynesia', 'French Southern Lands', 'Gabon', 'Gambia', 'Georgia', 'Germany', 'Ghana', 'Gibraltar', 'Greece', 'Greenland', 'Grenada', 'Guadeloupe', 'Guam', 'Guatemala', 'Guernsey', 'Guinea', 'Guinea-Bissau', 'Guyana', 'Haiti', 'Heard and McDonald Islands', 'Honduras', 'Hong Kong', 'Hungary', 'Iceland', 'India', 'Indonesia', 'Iraq', 'Ireland', 'Isle of Man', 'Israel', 'Italy', 'Jamaica', 'Japan', 'Jersey', 'Jordan', 'Kazakhstan', 'Kenya', 'Kiribati', 'Kuwait', 'Kyrgyzstan', 'Laos', 'Latvia', 'Lebanon', 'Lesotho', 'Liberia', 'Libya', 'Liechtenstein', 'Lithuania', 'Luxembourg', 'Macau', 'Macedonia', 'Madagascar', 'Malawi', 'Malaysia', 'Maldives', 'Mali', 'Malta', 'Marshall Islands', 'Martinique', 'Mauritania', 'Mauritius', 'Mayotte', 'Mexico', 'Moldova', 'Monaco', 'Mongolia', 'Montenegro', 'Montserrat', 'Morocco', 'Mozambique', 'Namibia', 'Nauru', 'Nepal', 'Netherlands', 'New Caledonia', 'New Zealand', 'Nicaragua', 'Niger', 'Nigeria', 'Niue', 'Norfolk Island', 'Northern Mariana Islands', 'Norway', 'Oman', 'Pakistan', 'Palau', 'Panama', 'Papua New Guinea', 'Paraguay', 'Peru', 'Philippines', 'Pitcairn Islands', 'Poland', 'Portugal', 'Puerto Rico', 'Qatar', 'R?union', 'Romania', 'Russia', 'Rwanda', 'Saint Barth?lemy', 'Saint Helena', 'Saint Kitts and Nevis', 'Saint Lucia', 'Saint Martin', 'Saint Pierre and Miquelon', 'Saint Vincent', 'Samoa', 'San Marino', 'S?o Tom? and Pr?ncipe', 'Saudi Arabia', 'Senegal', 'Serbia', 'Seychelles', 'Sierra Leone', 'Singapore', 'Sint Maarten', 'Slovakia', 'Slovenia', 'Solomon Islands', 'Somalia', 'South Africa', 'South Georgia', 'South Korea', 'Spain', 'Sri Lanka', 'Sudan', 'Suriname', 'Svalbard and Jan Mayen', 'Sweden', 'Swaziland', 'Switzerland', 'Syria', 'Taiwan', 'Tajikistan', 'Tanzania', 'Thailand', 'Togo', 'Tokelau', 'Tonga', 'Trinidad and Tobago', 'Tunisia', 'Turkey', 'Turkmenistan', 'Turks and Caicos Islands', 'Tuvalu', 'Uganda', 'Ukraine', 'United Arab Emirates', 'United Kingdom', 'United States', 'Uruguay', 'Uzbekistan', 'Vanuatu', 'Vatican City', 'Vietnam', 'Venezuela', 'Wallis and Futuna', 'Western Sahara', 'Yemen', 'Zambia', 'Zimbabwe'],
    }
  },
  mounted() {
    let input = document.querySelector('input');
    input.style.opacity = 0;
  },
  methods: {
    onAttachmentChange(e) {
      this.img = e.target.files[0].name;
    },
    delete_img() {
      this.img = 'default.jpg';
    },
    cancel() {
      this.$router.push('/login');
    },
    signup() {
      let newUser = {
        fname: this.fname,
        lname: this.lname,
        country: this.country,
        email: this.email,
        password: this.password,
        img: this.img,
      }
      if (this.password == this.rep_password) {
        axios.post('/signup', newUser)
          .then(res => {
            console.log(res.data)
            this.error = '';
            this.$router.push('/login');
          }, err => {
            console.log(err.response)
            this.error = err.response.data.title
          })
      } else {
        this.error = "Password does not match"
      }
    }
  }
}
</script>
