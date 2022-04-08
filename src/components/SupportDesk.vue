<template>
  <div class="ticket-panel__submission-panel">
    <p class="ticket-panel__date">{{ currentDate }}</p>
    <form class="ticket-panel__create-ticket" @submit.prevent="createNewTicket">
      <label for="ticketDate" class="label">Ticket Date</label>
      <input type="date" id="ticketDate" v-model="newTickets.ticketDate" />
      <br />
      <label for="ticketURL" class="label">Ticket URL</label>
      <input type="text" id="ticketURL" v-model="newTickets.ticketURL" />
      <br />
      <label for="quotedHours" class="label">Quoted Hours</label>
      <input type="text" id="quotedHours" v-model="newTickets.quotedHours" />
      <br />
      <label for="actualHours" class="label">Actual Hours Date</label>
      <input type="text" id="actualHours" v-model="newTickets.actualHours" />
      <button class="custom-button" :disabled="Object.values(this.newTickets).every((x) => x === '')">Add Ticket</button>
    </form>
  </div>
  <div class="ticket-panel__tickets-wrapper">
    <p class="ticket-panel_header">Tickets</p>
    <div v-if="this.warningContent !== ''">
    <p class="warningMessage">{{this.warningContent}}</p>
    </div>
    <br>
    <table class="ticket-panel__tickets-table">
      <thead>
        <tr>
          <th>Date</th>
          <th>Link</th>
          <th>Time Quoted</th>
          <th>Actual Time</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(ticket) in this.tickets" :key="ticket.id">
          <td>{{ ticket.date }}</td>
          <td>{{ ticket.url }}</td>
          <td>{{ ticket.quoted_hours }}</td>
          <td>{{ ticket.actual_hours }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'SupportDesk',
  data () {
    return {
      tickets: [],
      newTickets: {
        ticketDate: '',
        ticketURL: '',
        quotedHours: '',
        actualHours: ''
      },
      currentDate: '',
      warningContent: ''
    }
  },
  methods: {
    createNewTicket () {
      const self = this
      if (Object.values(this.newTickets).every((x) => x !== '')) {
        axios
          .post('http://localhost:3000/tickets', {
            date: this.newTickets.ticketDate,
            url: this.newTickets.ticketURL,
            quoted_hours: this.newTickets.quotedHours,
            actual_hours: this.newTickets.actualHours
          })
          .then(function (response) {
            self.getTickets()
            console.log(response)
          })
      } else {
        const message = 'You have not filled in all ticket data'
        this.warningMessage(message)
      }
    },
    getCurrentDate () {
      const date = new Date()
      const month = date.getMonth()
      const day = date.getDate()
      const year = date.getFullYear()
      this.currentDate = `${day + '/' + month + '/' + year}`
    },
    getTickets () {
      axios
        .get('http://localhost:3000/tickets')
        .then((response) => (this.tickets = response.data))
    },
    warningMessage (message) {
      this.warningContent = message
    }
  },
  mounted () {
    this.getTickets()
    this.getCurrentDate()
  }
}
</script>
