<template>
  <div id="welcome">
    <div id="hello">Hello, {{ name }}!</div>
    <div id="greeting">Welcome back!</div>
    <div id="thanks">Thank you for being part of our community to preserve our mother Earth</div>
    <div id="points">
      You have {{points}} EcoPoints
      <img
        id="icon"
        src="https://image.flaticon.com/icons/png/128/53/53775.png"
      />
    </div>
  </div>
</template>

<script>
import { fb, database } from "../firebase.js";
export default {
  data() {
    return {
      points: null,
      name: null
    };
  },
  methods: {
    fetchUserData: function() {
      let id = fb.auth().currentUser.uid;
      database
        .collection("users")
        .doc(id)
        .get()
        .then(doc => {
          this.name = doc.data().name;
          this.points = doc.data().points;
        });
    }
  },
  created() {
    this.fetchUserData();
  }
};
</script>

<style scoped>
#welcome {
  width: 40%;
  height: 250px;
  background: #81af93;
  border-radius: 25px;
  position: absolute;
  top: 170px;
  left: 30%;
}
#hello {
  font-family: "EB Garamond";
  padding: 2%;
  color: black;
  font-weight: bold;
  font-size: 24px;
  text-transform: uppercase;
}
#greeting {
  font-family: "EB Garamond";
  padding: 2%;
  color: #00565e;
  font-weight: bold;
  font-size: 30px;
}
#thanks {
  font-family: "EB Garamond";
  padding: 2%;
  color: #00565e;
  font-weight: normal;
  font-size: 15px;
}
#points {
  font-family: "EB Garamond";
  padding: 2%;
  color: black;
  font-weight: bold;
  font-size: 30px;
}
#icon {
  max-width: 30px;
}
#dp {
  max-width: 80px;
  position: absolute;
  left: 5%;
  top: 10%;
  border-radius: 1000px;
}
</style>