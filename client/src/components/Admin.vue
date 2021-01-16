<template>
  <div class="container">
    <div class="row align-content-around mt-5">
      <label class="display-4">Admin</label>
    </div>
    <div class="row justify-content-center">
      <div class=" p-3 col-md-8 bg-white rounded border">
          <form  @submit.prevent="">
                <div>
                  <div style="text-align: initial;">
                    <table class="table">
                        <thead class="thead-dark">
                          <th>User</th>
                          <th>Event</th>
                          <th>Role</th>
                          <th>Assign</th>
                        </thead>
                        <tr v-for="d in doc">
                          <td>{{d[0]}}</td>
                          <td>{{d[1]}}</td>
                          <td>{{d[2]}}</td>
                          <td>
                              <button v-if="d[4]==''" @click="assign(d[3])" class="btn btn-outline-warning" style="width: 75%;">Assign</button>
                              <button v-if="d[4]=='true'" @click="remove(d[3])" class="btn btn-outline-danger" style="width: 75%;">Remove</button>
                          </td>
                        </tr>
                      </table>
                  </div>
                  <div style=" text-align: right;">
                      <button @click="cancel" class="btn btn-outline-dark float-lg-right" style="margin-right: 50px;width: 115px;">Cancel</button>
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
  name: 'Admin',
  data() {
    return {
      documents: [],
      doc: [],
      user: '',
    }
  },
  mounted() {
    axios.get('/eventAll')
    .then(res => {

        this.documents=res.data
        this.documents.forEach(element => {
        let name=element.data_user.fname+" "+element.data_user.lname;
        this.doc.push([name, element.title, element.role, element.id, element.status]);
        });
    })
  },
  methods: {
    cancel(){
        this.$router.push('/username');
    },
    assign(idEvent){
        axios.put('/AssignEvent/'+idEvent)
        .then((res) =>
        {
          window.location.reload();
        })
    },
    remove(idEvent){
        axios.delete('/event/'+idEvent)
        .then((res) => {
          window.location.reload();
        })
    },
  }
}
</script>

