<template>
  <div class="container">
    <div class="row align-content-around mt-5">
      <label class="display-4">Documents</label>
    </div>
    <div class="row justify-content-center">
      <div class=" p-3  col-md-8 bg-white rounded border">
          <form  @submit.prevent="">
                <div>
                  <div style="text-align: initial;">
                    <table class="table">
                        <thead class="thead-dark">
                          <th>Document</th>
                          <th>Day</th>
                          <th>For</th>
                          </thead>
                        <tr v-for="document in documents">
                          <td><a :href="'/#/event/'+id+'/documents/'+document.id+'/info'">{{document.title}}</a></td>
                          <td>{{document.day}}</td>
                          <td>{{document.role}}</td>
                        </tr>
                      </table>
                  </div>
                  <div style=" text-align: right;">
                      <button @click="add" class="btn btn-outline-dark float-right" style="width: 175px;">Add Document</button>
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
  name: 'Documents',
  data() {
    return {
      documents: '',
      success: '',
      id: '',
    }
  },
  mounted() {
    axios.get('/user', { headers: { token: localStorage.getItem('token')}})
      .then(res => {
        this.id=res.data.id;
        axios.get('/documentsAll/'+ this.id)
        .then(res => {
          this.documents=res.data
        })

      })
  },
  methods: {
    add(){
        this.$router.push('/documents/newDocument');
    }
  }
}
</script>
