<template>
  <div id="app">
    <img alt="logo" src="./assets/logo.png" width="500" height="500">
    <h1>TCD Room Booking App</h1>
    <DataDisplay v-if="dataFound" :bookingRef="bookingRef" :userEmail="userEmail"/>
    <Approval v-if="dataFound" :bookingRef="bookingRef" :userEmail="userEmail" :userID="userID"/>
    <div v-if="!dataFound">
      Something went wrong. Please approve this booking via our app!
    </div>
  </div>
</template>
<script>
import Approval from './components/Approval.vue';
import DataDisplay from './components/DataDisplay.vue';

export default {
  name: 'app',
  components: {
    Approval,
    DataDisplay
  },
  data (){
    return {
      refFound: Boolean,
      idFound: Boolean,
      emailFound: Boolean,
      dataFound: false,
      userEmail: "",
      bookingRef: "",
      userID: "",
      useState: Object
    }
  },
  created() {
    let urlParams = new URLSearchParams(window.location.search);
    this.refFound = urlParams.has('ref'); // check for booking ref
    console.log(this.refFound); 
    this.emailFound = urlParams.has('email'); // check for user email
    console.log(this.emailFound); 
    this.idFound = urlParams.has('id'); // check for user id
    console.log(this.idFound); 
    this.bookingRef = urlParams.get('ref');
    console.log(this.bookingRef);
    this.userEmail = urlParams.get('email')
    console.log(this.userEmail);
    this.userID = urlParams.get('id')
    console.log(this.userID);
    this.dataFound = this.refFound && this.idFound && this.emailFound;
  },  
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
