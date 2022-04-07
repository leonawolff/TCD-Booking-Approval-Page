<template>
  <div class="approve">
    <div v-if="!clicked">
      <h4>Accept or Reject Booking Request</h4>
      <ul>
        <button v-on:click="onClick(true)">Accept</button>
        &nbsp;
        <button v-on:click="onClick(false)">Reject</button>
      </ul>
    </div>
    <div v-if="clicked && success">
      <h4>Success!</h4>
      <p>Your response has been recorded. Thanks!</p>
    </div>
    <div v-if="clicked && failure">
      <h4>Uh oh!</h4>
      <p>Something went wrong. Please approve/deny this booking via our app!</p>
    </div>
  </div>
</template>

<script>
import { firebase } from "../firebaseInit";
import "firebase/compat/firestore";
import "firebase/compat/auth";
import { getDoc } from "firebase/firestore";

export default {
  name: "Approval",
  props: {
    bookingRef: String,
    userEmail: String,
    userID: String,
  },
  data() {
    return {
      success: false,
      failure: false,
      clicked: false,
      approverFound: false
    };
  },
  methods: {
    async UpdateBooking(propData) {
      const updateMsg = propData.updateMsg;
      const booking = propData.booking;
      const userEmail = propData.user;

      console.log(updateMsg + " booking " + booking);
      const entityRef = firebase
        .firestore()
        .collection("bookings")
        .doc(booking);


      const docSnap = await getDoc(entityRef);
      if (docSnap.exists()) {
        console.log("Document data:", docSnap.data());
      } else {
        console.log("No such document!");
      }
      let approverList = docSnap.data().approvers;
      var size = Object.keys(approverList).length;
      console.log(size);

      for(const key of Object.getOwnPropertyNames(approverList)){
        console.log(key)
        console.log(userEmail)
        if(key == userEmail){
          this.approverFound = true;
        }
      }
      if (this.approverFound){
        approverList[userEmail] = updateMsg;
      }

      entityRef
        .update({
          approvers: approverList
        })
        .then(() => {
          console.log("Booking updated!");
          this.success = true;
        })
        .catch((error) => {
          console.log(error);
          this.failure = true;
        });
    },
    approveBooking() {
      var propData = {
        booking: this.bookingRef,
        user: this.userEmail,
        updateMsg: "Approved",
      };
      this.UpdateBooking(propData);
    },
    denyBooking() {
      var propData = {
        booking: this.bookingRef,
        user: this.userEmail,
        updateMsg: "Denied",
      };
      this.UpdateBooking(propData);
    },

    onClick(approve) {
      this.clicked = true;
      if (approve) this.approveBooking();
      else this.denyBooking();
    },
  },
};
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
