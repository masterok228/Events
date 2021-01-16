<template>
  <div class="container">
    <div class="row align-content-around mt-5">
      <label class="display-4">Events</label>
    </div>
    <div class="row justify-content-center">
      <div class=" p-3  col-md-8 bg-white rounded border">
          <form  @submit.prevent="">
                <div>
                  <div style="text-align: initial;">
                    <table class="table">
                        <thead class="thead-dark">
                          <th>Event</th>
                          <th>Dates</th>
                          <th>Participants</th>
                          </thead>
                        <tr v-for="event in events">
                          <td><a :href="'/#/event/' + event.id">{{event.title}}</a></td>
                          <td>{{event.sDate}}-{{event.fDate}}</td>
                          <td>{{event.participants}}</td>
                        </tr>
                      </table>
                  </div>
                    <button @click="newevent" class="btn btn-outline-dark float-right" style="width: 25%;">Add Event</button>
                </div>
          </form>
        </div>
    </div>
</div>
</template>
<script>
import axios from 'axios';
export default {
  name: 'Events',
  data() {
    return {
      events: '',
      success: '',
    }
  },
  mounted() {
    axios.get('/events', { headers: { token: localStorage.getItem('token')}})
      .then(res => {
        this.events=res.data;
      })
  },
  methods: {
    newevent(){
        this.$router.push('/events/newEvent');
    }
  }
}
</script>
