<template>
  <div class="container">
    <div class="row align-content-around mt-5">
      <label class="display-4">Event</label>
    </div>
    <div class="row justify-content-center">
      <div class="pt-4 pr-5 pb-4 bg-white rounded border">
          <form  @submit.prevent="">
            <div class="form-group" style="display: inline-block;vertical-align: top;">
              <img class="rounded-circle border mb-2" :src="require('@/assets/event/'+img)" style="width: 200px;"><br>
              <input type="file" name="image_uploads" style="cursor: pointer;"@change="onAttachmentChange"><br>
              <button class="btn btn-outline-dark" style="margin-top: -37px; width: 47.5%;">Upload</button><br>
              <button @click="delete_img" class="btn btn-outline-dark"  style="width: 47.5%;">Delete</button>
            </div>
            <div style="display: inline-block;text-align: right;margin-left: 40px;">
              <div  class="rounded border">
                <div class="rounded border bg-dark text-light text-left pl-2">
                  <b>Event Information</b>
                  <img :src="require('@/assets/edit.png')" style="padding:2px; width: 22px;float: right;margin-right: 4px;cursor: pointer;"
                    @click="edit_information"><br>
                </div>
                <div style="width: 448px;padding: 13px 13px 2px 13px;">
                  <div style="text-align: initial;">
                    <label style="padding-right: 9px;"><b>Event Title: </b></label>{{event.title}}<br>
                  </div>
                </div>
              </div>
              <div class="rounded border mt-2">
                <div class="rounded border bg-dark text-light text-left pl-2">
                  <b>Dates Information</b>
                  <img :src="require('@/assets/edit.png')" style="padding:2px; width: 22px;float: right;margin-right: 4px;cursor: pointer;"
                   @click="edit_dates"><br>
                </div>
                <div style="width: 448px;padding: 13px 13px 2px 13px;">
                  <div style="text-align: initial;">
                    <label style="padding-right: 9px;"><b>Start Date: </b></label>{{event.sDate}}<br>
                    <label style="padding-right: 9px;"><b>C1 Date: </b></label>{{event.c1Date}}<br>
                    <label style="padding-right: 9px;"><b>C+1 Date: </b></label>{{event.c_1Date}}<br>
                    <label style="padding-right: 9px;"><b>Finish Date: </b></label>{{event.fDate}}
                  </div>
                </div>
              </div>
              <div class="rounded border mt-2">
                <div class="rounded border bg-dark text-light text-left pl-2">
                  <b>Participants</b>
                  <img :src="require('@/assets/edit.png')" style="width: 22px;float: right; padding:2px; margin-right: 4px;cursor: pointer;"
                   @click="edit_part"><br>
                </div>
                <div style="width: 448px;padding: 13px 13px 2px 13px;">
                  <div style="text-align: initial;">
                    <label style="padding-right: 9px;"><b>Participants: </b></label>{{event.participants}}
                  </div>
                </div>
              </div>
                <div class="rounded border text-dark text-left pl-3 pt-2 pb-5 mt-2 text-align: initial;" style="height: 50px">
                  <span class="font-weight-bold" style="margin-left: -5px;">Documents</span>
<!--                  <img :src="require('@/assets/edit.png')" style="padding:2px; width: 22px;float: right;margin-right: 4px;cursor: pointer;"-->
<!--                   @click="edit_documents"><br>-->
                  <button @click="edit_documents" class="btn btn-outline-success mt-1 mr-2" style="float: right;width: 25%;">View</button><br>
                </div>
              <br>
              <button @click="cancel" class="btn btn-outline-dark" style="width: 47.5%;">Cancel</button><br>
            </div>
          </form>
        </div>
    </div>
</div>
</template>
<script>
import axios from 'axios';
export default {
  name: 'Event',
  data() {
    return {
      img: '1.jpg',
      event: '',
      error: '',
      id: this.$route.params.id,
    }
  },
  mounted() {
    let input = document.querySelector('input');
    input.style.opacity = 0;

    axios.get('/event/'+this.id, { headers: { token: localStorage.getItem('token')}})
      .then(res => {
        this.event=res.data;
        this.img=this.event.img;
      })
  },
  methods: {
    edit_information(){
        this.$router.push('/event/'+this.id+'/info');
    },
    edit_dates(){
        this.$router.push('/event/'+this.id+'/dates');
    },
    edit_part(){
        this.$router.push('/event/'+this.id+'/part');
    },
    edit_documents(){
        this.$router.push('/event/'+this.id+'/documents');
    },
    cancel(){
          this.$router.push('/events');
    },
    onAttachmentChange (e) {
        axios.put(`/imgEvent`,{ img: e.target.files[0].name, id: this.id})
        .then((res) =>
        {
          this.img=e.target.files[0].name;
        })
    },
    delete_img(){
        axios.put(`/imgEvent`,{ img: '1.jpg', id: this.id})
        .then((res) =>
        {
          this.img='1.jpg';
        })
    },
  }
}
</script>
