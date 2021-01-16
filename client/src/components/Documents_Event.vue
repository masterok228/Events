<template>
  <div class="container">
    <div class="row align-content-around mt-5">
      <label class="display-4">{{event.title}}</label>
    </div>
    <div class="row justify-content-center">
      <div class="p-3 bg-white rounded border">
          <form  @submit.prevent="">
                <div style="padding: 13px 13px 2px 13px;">
                  <div style="text-align: initial;">
                    <table class="table" style="width: 614px;">
                        <thead class="thead-dark">
                          <th>Document</th>
                          <th>Day</th>
                          <th>State</th>
                          </thead>
                        <tr v-for="document in documents">
                          <td><a :href="'/#/event/'+id+'/documents/'+document.id+'/info'">{{document.title}}</a></td>
                          <td>{{document.day}}</td>
                          <td v-if="document.state=='false'"><button @click="setState(document.id)" class="btn btn-warning" style="color: white; padding: 4px 15px 4px 15px">Signed</button></td>
                          <td v-if="document.state=='true'">Signed</td>
                        </tr>
                      </table>
                  </div>
                  <button @click="cancel" class="btn btn-outline-dark float-right" style="width: 25%;">Cancel</button><br>
                </div>
          </form>
        </div>
    </div>
</div>
</template>
<script>
import axios from 'axios';
export default {
  name: 'Documents',
  data() {
    return {
      documents: '',
      success: '',
      event: '',
      id: this.$route.params.id,
    }
  },
  mounted() {
    axios.get('/documents/'+this.id)
      .then(res => {
        this.documents=res.data;
      })
    axios.get('/event/'+this.id, { headers: { token: localStorage.getItem('token')}})
      .then(res => {
        this.event=res.data;
      })
  },
  methods: {
    setState(id){
        this.$router.push('/event/'+this.id+'/documents/'+id);
    },
    cancel(){
      this.$router.push('/event/'+this.id);
    }
  }
}
</script>
