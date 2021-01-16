<template>
  <div class="container">
    <div class="row align-content-around mt-5">
      <label class="display-4">Participants</label>
    </div>
    <div class="row justify-content-center">
      <div class="p-3 bg-white rounded border">
          <form  @submit.prevent="">
                <div style="padding: 13px 13px 2px 13px;">
                  <div style="text-align: initial;">
                    <label style="padding-right: 9px;"><b>Participants </b></label><input type="text" class="form-control"  v-model="event.participants"><br>
                    <div style=" text-align: right;">
                      <button @click="update" class="btn btn-outline-dark mr-1" style="width: 47%;">Ok</button>
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
  name: 'EventInformation',
  data() {
    return {
      event: '',
      success: '',
      id: this.$route.params.id,

    }
  },
  mounted() {
      axios.get('/event/'+this.id, { headers: { token: localStorage.getItem('token')}})
      .then(res => {
        this.event=res.data;
      })
  },
  methods: {
    cancel(){
          this.$router.push('/event/'+this.id);
    },
    update(){
        axios.put(`/updateEvent`,{ event: this.event})
        .then((res) =>
        {
          this.$router.push('/event/'+this.id);
        })
    },
  }
}
</script>
