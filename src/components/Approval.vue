<template>
  <div class="approve">
    <h4>Accept or Reject Booking Request</h4>
    <ul>
      <button v-on:click="approve=true">Accept</button>
      &nbsp;
      <button v-on:click="approve=false">Reject</button>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'Approval',
  data (){
    return {
      approve: Boolean,
      result: Boolean,
    }
  }
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
