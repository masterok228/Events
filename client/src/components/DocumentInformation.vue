<template>
  <div class="container">
    <div class="row align-content-around mt-5">
      <label class="display-4">{{document.title}}</label>
    </div>
    <div class="row justify-content-center">
      <div class="p-4 bg-white rounded border">
          <form  @submit.prevent="">
              <div class="rounded border">
                <div class="rounded border bg-dark text-light text-left pl-2">
                  <b>Document Information</b>
                  <img :src="require('@/assets/edit.png')" style="padding:2px; width: 22px;float: right;margin-right: 4px;cursor: pointer;"
                   @click="edit_dates"><br>
                </div>
                <div class="pt-2 pl-2" style="width: 500px;">
                  <div style="text-align: initial;">
                    <label style="padding-right: 9px;"><b>Document Title: </b></label>{{document.title}}<br>
                    <label style="padding-right: 9px;"><b>Day: </b></label>{{document.day}}<br>
                    <label style="padding-right: 9px;"><b>Document Content: </b></label>{{document.content}}<br>
                    <label style="padding-right: 9px;"><b>For: </b>{{document.role}}</label><br>
                  </div>
                </div>
              </div><br>
              <div style=" text-align: right;">
                <button @click="cancel" class="btn btn-outline-dark mr-1" style="width: 48.5%;">Cancel</button>
                <button @click="deletee" class="btn btn-outline-dark ml-1" style="width: 48.5%;">Delete</button><br>
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
  name: 'DocumentInformation',
  data() {
    return {
      document: '',
      success: '',
      id: this.$route.params.id,
      id_doc: this.$route.params.id_doc,
    }
  },
  mounted() {
      axios.get('/document/'+this.id_doc)
      .then(res => {
        this.document=res.data;
      })
  },
  methods: {
    edit_dates(){
          this.$router.push('/documents/'+this.id_doc+'/update');
    },
    cancel(){
          this.$router.push('/documents');
    },
    update(){
        axios.put(`/updateEvent`,{ })
        .then((res) =>
        {
          this.success='Data updated successfully'
        })
    },
    deletee(){
        axios.delete('/document/'+this.id_doc)
        .then((res) => {
          this.$router.push('/documents');
        })
    }
  }
}
</script>
