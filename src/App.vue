<template>
  <h1>Support Ticket Tracker</h1>
  <br />
  <div class="container">
    <SupportDesk />
  </div>
</template>

<script>
import SupportDesk from './components/SupportDesk.vue'

export default {
  name: 'App',
  components: { SupportDesk }
}
</script>
