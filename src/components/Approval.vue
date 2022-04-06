<template>
  <div class="approve">
    <h4>Accept or Reject Booking Request</h4>
    <ul>
      <button v-on:click="approved=true">Accept</button>
      &nbsp;
      <button v-on:click="approved=false">Reject</button>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'Approval',
}
</script>

<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
