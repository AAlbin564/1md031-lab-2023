<template>
    <div>
    <img v-bind:src="burger.urlimg">
    <h3>{{burger.name}}&copy;</h3>
    <ul>
        <li>grainfed beef</li>
        <li v-if="burger.lact" class="nut_info">contains lactose</li>
        <li v-if="burger.glut" class="nut_info">contains gluten</li>
        <li>{{ burger.Kcal }} kcal</li>
        <li>{{ burger.desc }}</li>
    </ul>
    <p>ordered {{ amountOrdered }}</p>
    <button v-on:click="increaseAm">inc</button>
    <button v-on:click="decreaseAm">dec</button>
    </div>
  </template>
  
  <script>
  export default {
    name: 'OneBurger',
    props: {
      burger: Object
    },
    data: function () {
  return {
    amountOrdered: 0,
  }
  },
  methods: {
    increaseAm: function(){
      this.amountOrdered++;
      this.$emit('orderedBurger', 
      {name: this.burger.name,
      amount: this.amountOrdered}
      )
      console.log(this.burger.name, this.amountOrdered)
    },
    decreaseAm: function(){
      if (this.amountOrdered)
      this.amountOrdered--;
      this.$emit('orderedBurger', {name: this.burger.name,
      amount: this.amountOrdered})
    }
  }
  }
  
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
 .nut_info{
    font-weight: bold;
 }
 img{
  width: 70%;
  height: 50%;
 }
 div{
  background-color: blue;
 }
  </style>
  