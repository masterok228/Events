<template>
  <div class="container">
    <div class="row align-content-around mt-5">
      <label class="display-4">Dates</label>
    </div>
    <div class="row justify-content-center">
      <div class="p-3 bg-white rounded border">
          <form  @submit.prevent="">
                      <table class="table">
                        <thead>
                          <th class="table-active">С-2</th>
                          <th class="table-success">С1</th>
                          <th class="table-warning">С+1</th>
                          <th class="table-info">С+2</th>
                        </thead>
                        <tr>
                          <td class="table-active">{{event.sDate}}</td>
                          <td class="table-success">{{event.c1Date}}</td>
                          <td class="table-warning">{{event.c_1Date}}</td>
                          <td class="table-info">{{event.fDate}}</td>
                        </tr>
                      </table>
                <div style="padding: 11px 126px;">
                  <div style="text-align: initial;">
                    <label style="padding-right: 9px;"><b>Start Date: </b></label><input type="date" class="form-control"  style="width: 498px;" v-model="event.sDate"><br>
                    <label style="padding-right: 9px;"><b>C1 Date: </b></label><input type="date" class="form-control"  v-model="event.c1Date"><br>
                    <label style="padding-right: 9px;"><b>C+1 Date: </b></label><input type="date" class="form-control"  v-model="event.c_1Date"><br>
                    <label style="padding-right: 9px;"><b>Finish Date: </b></label><input type="date" class="form-control"  v-model="event.fDate"><br>
                    <div style=" text-align: right;">
                      <button @click="update" class="btn btn-outline-dark mr-1" style="width: 48.5%;">Ok</button>
                      <button @click="cancel" class="btn btn-outline-dark ml-1" style="width: 48.5%;">Cancel</button><br>
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
  name: 'Dates',
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
