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
      allData: Object
    }
  },
  methods: {
  //   async readUser(){
  //     let response = await firebase.firestore()
  //       .collection('users')
  //       .doc(this.userID)
  //       .get()
  //       .catch(error => {
  //         console.log(error)
  //       });
  //     console.log(response.data)
  // },

  async UpdateBooking(propData) {
    
    const update = propData.update
    const booking = propData.booking
    console.log(update + " booking " + booking)
    const entityRef = firebase.firestore().collection('bookings').doc(booking);
    entityRef
        .update({status:update})
        .then(() => {
            console.log('Booking updated!');
          })
        .catch(error => {
            console.log(error)
        });
},
    approveBooking () {
      var propData = {
          booking:this.bookingRef,
          update:"Approved"
      }
      this.UpdateBooking(propData)
    },
    denyBooking () {
      var propData = {
          booking:this.bookingRef,
          update:"Denied"
      }
      this.UpdateBooking(propData)
    },

    // async readBooking(){
    //   const timestamp = firebase.firestore.Timestamp.now()
    //   console.log(timestamp)
    //   let response = await firebase.firestore().collection('bookings')
    //       .where("open", "==", true)
    //       .where("dateOfEvent", ">", timestamp)
    //       .get()
    //       .then(querySnapshot => {
    //         const documents = querySnapshot.docs.map(doc => doc.data())
    //         this.allData = documents;
    //           error => {
    //               console.log('Error : ', error)
    //           }
    //       })
    //       console.log(response)
    // },

    onClick(approve){
      if(approve)
        this.approveBooking();
      else
        this.denyBooking();
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
