<template>
  <div class="approve">
    <h4>Accept or Reject Booking Request</h4>
    <ul>
      <button v-on:click="onClick(true)">Accept</button>
      &nbsp;
      <button v-on:click="onClick(false)">Reject</button>
    </ul>
  </div>
</template>

<script>
// import firebase from "../firebaseInit";
import { firebase } from "../firebaseInit";
import 'firebase/compat/firestore';
import 'firebase/compat/auth';

export default {
  name: 'Approval',
  props: {
      bookingRef: String,
      userEmail: String,
      userID: String
  },
  data (){
    return {
      response: Boolean,
      result: Boolean,
    }
  },
  methods: {
    readUser(){
      let response = firebase.firestore()
          .collection('users')
          .doc(this.userID)
          .get()
          .then()
          .catch(error => {
              console.log(error)
          });
          console.log(response)
  },
    readBooking(){
      const collection = firebase.firestore().collection('bookings')
      const userID = this.props.userID;
      const userType = "admin"
      const timestamp = firebase.firestore.Timestamp.now()
      console.log(timestamp)
      console.log(userType)
      if (userType == "admin"){
          collection
          .where("open", "==", true)
          .where("dateOfEvent", ">", timestamp)
          .onSnapshot(
              querySnapshot => {
                  const newEntities = []
                  querySnapshot.forEach(doc => {
                      const entity = doc.data()
                      entity.id = doc.id
                      newEntities.push(entity)
                  });
                  this.props.setEntities(newEntities)
              },
              error => {
                  console.log('Error : ', error)
              }
          )
      }
      else {
          collection
          .where("open", "==", true)
          .where("tcdEmail", "==", userID)
          .where("dateOfEvent", ">", timestamp)
          .onSnapshot(
              querySnapshot => {
                  const newEntities = []
                  querySnapshot.forEach(doc => {
                      const entity = doc.data()
                      entity.id = doc.id
                      newEntities.push(entity)
                  });
                  this.props.setEntities(newEntities)
              },
              error => {
                  console.log('Error : ', error)
              }
          )
      }
    },
    onClick(approve){
      this.response = approve;
      console.log(this.response);
      this.readUser();
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
