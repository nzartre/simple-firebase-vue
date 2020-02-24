<template>
  <div>
    <p v-show="isLoading">Loading data</p>
    <ul>
      <li v-for="food in foods" :key="food.name">{{ food.name }}</li>
    </ul>
  </div>
</template>

<script>
  import * as firebase from 'firebase'
  import Vue from 'vue'

  const firebaseConfig = {
    apiKey: '',
    authDomain: '',
    databaseURL: '',
    projectId: '',
    storageBucket: '',
    messagingSenderId: '',
    appId: ''
  };
  let db;

  export default Vue.extend({
    name: 'HelloWorld',
    props: {
      msg: String
    },
    data() {
      return {
        foods: [],
        isLoading: true
      }
    },
    methods: {
      // Define a function to fetch foods from Firestore
      getFoods() {
        db.collection("foods").get().then((querySnapshot) => {
          // Add each item (food) to the array
          querySnapshot.forEach((food) => {
            this.foods.push(food.data())
          });
        }).finally(() => {
          this.isLoading = false;
        });
      }
    },
    // Run these statements automatically when the component is created
    created() {
      firebase.initializeApp(firebaseConfig);
      db = firebase.firestore();

      this.getFoods()
    }
  })
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
