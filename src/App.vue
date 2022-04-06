<template>
  <div id="app">
    <img alt="logo" src="./assets/logo.png" width="500" height="500">
    <h1>TCD Room Booking App</h1>
    <DataDisplay v-if="dataFound" :bookingRef="bookingRef" :userID="userID"/>
    <Approval v-if="dataFound"/>
    <div v-if="!dataFound">
      Something went wrong. Please approve via our app!
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
      dataFound: false,
      userID: "",
      bookingRef: ""
    }
  },
  created() {
    let urlParams = new URLSearchParams(window.location.search);
    this.refFound = urlParams.has('ref'); // check for booking ref
    console.log(this.refFound); 
    this.idFound = urlParams.has('id'); // check for user id
    console.log(this.idFound); 
    this.bookingRef = urlParams.get('ref');
    console.log(this.bookingRef);
    this.userID = urlParams.get('id')
    console.log(this.userID);
    this.dataFound = this.refFound && this.idFound;
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
