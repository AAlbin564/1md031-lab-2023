<template>
  <header>
        <img  src="_e0ff5835-8279-450d-ae83-57e0c792456f.jpg" alt="" id="hi">
        <h1>Welcome to BurgerTown Bredäng</h1>
    </header>
    <main>
    <section id="burger_info">
    <h2>
        order our burger:
    </h2>
    <p>
        we serve several burger including GoodBurger&copy;, ChiliBurger&copy; and Ångströmburger&copy;
    </p>
    
    <div class="wrapper">
      <Burger v-for="burger in burgers"
              v-bind:burger="burger" 
              v-bind:key="burger.name" 
              v-on:orderedBurger="addToOrder($event)"/>
              
    </div>
    </section>
    <section id="contact">
        <h2> 
            customer info:
        </h2>
        <p> 
            please provide your full name and address for delivery, we will also need your gender and your prefered payment method
        </p>
    
        <form>
            <p>
                <label for="name">name</label><br>
                <input type="text" id="name" v-model="fn" required="required" placeholder="Full legal name">
            </p>
            
            <p>
                <label for="email">email</label><br>
                <input type="email" id="email" v-model="em" required="required" placeholder="E-mail address">
            </p>
            <p>
                <label for="options">Payment procedure</label><br>
                <select name="rcp" id="options" v-model="selected">
                    <option>Swish bank</option>
                    <option>wall street bank</option>
                    <option>kiruna bank</option>
                    <option selected="selected">swedbank</option>
                </select>
            </p>
            <p>
                <label for="gender3">Male</label><br>
                <input type="radio" id="gender3" value="male" checked="checked" v-model="picked"> 
            </p>
            <p>
                <label for="gender2">Female</label><br>
                <input type="radio" id="gender2"  value="female" v-model="picked"> 
            </p>
            <p>
                <label for="gender1">Does not wish to relay</label><br>
                <input type="radio" id="gender1" value="N/A"  v-model="picked"> 
            </p>
            
            
        </form>
        
        <br>
    </section>
    <div id="rescaler">
    <div id="map" v-on:click="setLocation">
      <div id="dots1">
        <div  v-bind:style="{ left: this.location.x + 'px', 
                      top: this.location.y  + 'px',}">
    T
</div>
</div>
      click here
</div>
        
      </div>
      
      
    <button type="submit" value="place order" v-on:click="sendIt">
        <img src="https://i.pinimg.com/originals/a5/bc/dd/a5bcdd47a1cded2da258ace8bbe7f1ec.jpg" alt=":(" style="width: 50px; height: 20px;">
        place order
    </button>
    <br>
    <br>
    <hr>
    <footer>
        <div style="display: inline-block;"><p>&copy; copyright 2023</p></div>
        <div style="display: inline-block;"><p>öppettider <br> 07:00-06:00</p></div>
        
        
    </footer>
    </main>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io();


function MenuItem(name,urlimg,kCal,glut,lact,desc){
  this.name = name;
  this.urlimg = urlimg;
  this.kCal = kCal;
  this.glut = glut;
  this.lact = lact;
  this.desc = desc;


}
export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers : menu,
      em: '',
      fn: '',
      hn:'',
      st:'',
      selected:'',
      picked: 'male',
      orderedBurgers:{
        
      },
      location: { 
            x: 0,
            y: 0
          }
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    sendIt: function(){
      console.log(this.picked,this.selected,this.st,this.hn,this.fn,this.em, this.orderedBurgers)
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: this.location.x,
                                           y: this.location.y,
                                           email: this.em,
                                           fullName: this.fn,
                                           pay: this.selected,
                                           gen: this.picked
                                          },
                                orderItems: this.orderedBurgers
                              }
                              
                              
                 );
    },
    addToOrder: function(event){
      this.orderedBurgers[event.name] = event.amount;
    },
    setLocation: function(event){
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
                    this.location = {x: event.clientX - offset.x -75,y: event.clientY-offset.y -75}

    }
    
  }
}
</script>

<style>
  #dots1{
    position: relative;
  }
  #dots1 div{
    position: absolute;
  }
  #map {
    height: 1030px;
    width: 1871px;
    background-size: cover;
    background: url("C:\lab_1\1md031-lab-2023\public\img\polacks.jpg");
    cursor: crosshair;
    
  }
  #rescaler{
    height: 400px;
    overflow: scroll;
  }
  #hi{
    opacity: 0.75;
    width: 100%;
    height: auto;
}
header{
    margin-left: 25px;
    height: 150px;
    overflow: hidden;
}
.wrapper{
    display: grid;
    /*grid-template-columns: 100px 100px 100px;*/
    grid-template: 100% / 1fr 1fr 1fr;
    height: 750px;
    grid-gap: 200px
}

header h1{
    position: absolute;
    margin-top: -1250px;
    margin-left: 35px;
}
body {
    font-size: 3vh;
    font-family: arial;
    
 }
 section{
    margin-left: 0;
    margin: 0px 0px 0px 25px;
    border-style: dashed;
 }
 section p{
    padding-left: 10px;
 }
 .nut_info{
    font-weight: bold;
 }
 #burger_info{
    color: #ffffff;
    background-color: #000000;
    border-color: #ffffff;
    
 }
 #contact{
    border-color: #000000;
 }
 button:hover {
    background-color: aqua;
    cursor: pointer;
 }
 button{
    margin: 25px 0px 0px 50px;
 }
 div{
    margin: 10px;
    padding: 25px;
 }

</style>