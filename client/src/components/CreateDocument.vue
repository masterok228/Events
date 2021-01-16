<template>
  <div class="container">
    <div class="row align-content-around mt-5">
      <label class="display-4">New document</label>
    </div>
    <div class="row justify-content-center">
      <div class=" p-3 col-md-5 bg-white rounded border">
          <form  @submit.prevent="">
            <div  style="display: inline-block;text-align: right;">
              <div class="form-group">
                <label style="float: left;">Document Title</label>
                <input type="text" class="form-control" v-model="dTitle">
              </div>
              <div class="form-group">
                <label  style="float: left;">Event</label>
                <select v-model="EventSelect" class="custom-select">
					        <option disabled value="n1">Selected event</option>
                  <option v-for="event in events" v-bind:value="event.id">{{event.title}}</option>
                </select>
              </div>
              <div class="form-group">
                <label  style="float: left;">Day</label>
                <select v-model="DaytSelect" class="custom-select">
                  <option disabled value="n1">Selected day</option>
                  <option value="С-2">C-2</option>
                  <option value="С1">C1</option>
                  <option value="С+1">C+1</option>
                  <option value="С+2">C+2</option>
                </select>
              </div>
              <div class="form-group">
                <label  style="float: left;">Document Content</label>
                <textarea  class="form-control" v-model="dContent"/>
              </div>
              <div class="form-group" style="text-align: left;">
                <label  style="float: left;">For</label>
                <div class="ml-5">
                  <input v-model="role" type="radio" value="Experts" checked> Experts<br>
                  <input v-model="role" type="radio" value="Competitiors"> Competitiors
                </div>
              </div>
              <button @click="create" class="btn btn-outline-dark mr-1" style="width: 48%;" >Ok</button>
              <button @click="cancel" class="btn btn-outline-dark ml-1" style="width: 48%;" >Cancel</button><br>
              <span class='success' style="color: red;">{{error}}</span>
            </div>
          </form>
        </div>
    </div>
</div>
</template>
<script>
import axios from 'axios';
export default {
  name: 'CreateDocument',
  data() {
    return {
      dTitle: '',
      EventSelect: 'n1',
      DaytSelect: 'n1',
      dContent: '',
      role: '',
      events: '',
      user: '',
      error: '',
    }
  },
  mounted() {
    axios.get('/events', { headers: { token: localStorage.getItem('token')}})
      .then(res => {
        this.events=res.data;
      })

    axios.get('/user', { headers: { token: localStorage.getItem('token')}})
      .then(res => {
        this.user = res.data;
      })
  },
  methods: {
    cancel(){
          this.$router.push('/documents');
    },

    create(){
      let newDocument = {
        id_event: this.EventSelect,
        id_user: this.user.id,
        title: this.dTitle,
        day: this.DaytSelect,
        role: this.role,
        content: this.dContent,
        state: 'false'
      }
      axios.post('/createDocument', {document:newDocument})
        .then(res => {
          this.error = '';
          this.$router.push('/documents');
        }, err => {
          console.log(err.response)
          this.error = err.response.data.title
        })
    }
  }
}
</script>

