<template>
  <div class="container">
    <div class="row align-content-around mt-5">
      <label class="display-4">{{dTitle}}</label>
    </div>
    <div class="row justify-content-center">
      <div class="p-5 bg-white rounded border">
          <form  @submit.prevent="">
            <div  style="display: inline-block;text-align: right;">
              <div class="form-group">
                <label style="float: left;">Document Title</label>
                <input type="text" class="form-control" v-model="dTitle">
              </div>
              <div class="form-group">
                <label style="float: left;">Event</label>
                <select v-model="EventSelect" class="custom-select">
					        <option disabled value="n1">Selected event</option>
                  <option v-for="event in events" v-bind:value="event.id">{{event.title}}</option>
                </select>
              </div>
              <div class="form-group">
                <label style="float: left;">Day</label>
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
                <div  class="ml-5">
                  <input v-model="role" type="radio" value="Experts" checked> Experts<br>
                  <input v-model="role" type="radio" value="Competitiors"> Competitiors
                </div>
              </div>
              <button @click="update" class="btn btn-outline-dark mr-1" style="width: 48%;">Ok</button>
              <button @click="cancel" class="btn btn-outline-dark ml-1" style="width: 48%;">Cancel</button><br>
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
  name: 'UpdateDocument',
  data() {
    return {
      id_doc: this.$route.params.id_doc,
      events: '',
      dTitle: '',
      EventSelect: '',
      DaytSelect: '',
      dContent: '',
      role: '',
      success: '',
    }
  },
  mounted() {
    axios.get('/document/'+this.id_doc)
      .then(res => {
        this.dTitle=res.data.title;
        this.EventSelect=res.data.id_event;
        this.DaytSelect=res.data.day;
        this.dContent=res.data.content;
        this.role=res.data.role;
      })

     axios.get('/events', { headers: { token: localStorage.getItem('token')}})
      .then(res => {
        this.events=res.data;
      })
  },
  methods: {
    cancel(){
          this.$router.push('/documents');
    },
    update(){
      let Doc = {
        title: this.dTitle,
        id_event: this.EventSelect,
        day: this.DaytSelect,
        content: this.dContent,
        role: this.role,
      }
        axios.put('/UpdateDoc/'+this.id_doc, { document:Doc })
        .then(res => {
          this.$router.push('/documents');
        })
    }
  }
}
</script>
