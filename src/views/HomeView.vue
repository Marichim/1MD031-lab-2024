<template>

<header>
    <div id="header-image">
        <img src="/img/header.png" alt="header">
        <h1>Welcome to BurgerHeaven Online</h1>
    </div>
    </header>
    <!--<nav> Menu items </nav>-->
    <main>
        <section id="burger-section">       
            <h2>Select burger</h2>
            This is where you execute burger selection
            <div class="burger-grid">
                <Burger
                  v-for="burger in burgers"
                  v-bind:burger="burger"
                  v-bind:key="burger.name"
                  v-on:orderedBurgers="addToOrder($event)"
                />
            </div>
        </section>
      
        <section id="customer-section">
            <h2>Customer information</h2>
            This is where you provide necessary information

            <h3>Delivery information:</h3>

            <form>
                <p>
                    <label for="fullname">Full name</label><br>
                    <input type="text" id="fullname" v-model="fullname" required="required" placeholder="First- and Last name">
                </p>
                <p>
                    <label for="email">E-mail</label><br>
                    <input type="email" id="email" v-model="email" required="required" placeholder="E-mail address">
                </p>

                <p>
                <label for="payment">Payment options</label><br>
                <select id="payment" v-model="payment">
                    <option>Credit card</option>
                    <option>Swish</option>
                    <option>PayPal</option>
                    <option>Apple Pay</option>
                </select>
                </p>
                
                <div id="map-container">
                  <div class="map" id="map" v-on:click="setLocation">  
                    <div id="dots">
                      <div class="dot" v-bind:style="{ left: location.x + 'px', top: location.y + 'px' }">T</div>
                    </div>
                  </div>
                </div>
              
                <p>
                <label>Gender</label><br>
                    <label><input type="radio" v-model="gender" value="Male"> Male</label><br>
                    <label><input type="radio" v-model="gender" value="Female"> Female</label><br>
                    <label><input type="radio" v-model="gender" value="Non-binary"> Non-binary</label><br>
                    <label><input type="radio" v-model="gender" value="Undisclosed"> Undisclosed</label>
                </p>
            </form>
        </section>
        <button class="button" type="submit" v-on:click="placeOrder">
            <img src="/img/green check mark in circle.png" style="width:20px; height:20px;">
            Send Info
        </button>
    </main>
    <footer>
        © 2018 Hypothetical Burgers Inc.
    </footer>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json';

const socket = io("localhost:3000");

const burgersArray = menu;

console.log(burgersArray);

export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: burgersArray,
      fullname: "",
      email: "",
      street: "",
      house: "",
      payment: "",
      gender: "",
      orderedBurgers: {},
      location: { x: 0,
                  y: 0
                }
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    setLocation: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};

      this.location.x = event.clientX - offset.x;
      this.location.y = event.clientY - offset.y;
      
      console.log("Location updated:", this.location);

    },
    placeOrder: function () {
      console.log("ORDER INFORMATION:");
      console.log("Name:", this.fullname);
      console.log("Email:", this.email);
      console.log("Payment:", this.payment);
      console.log("Gender:", this.gender);
      

      socket.emit("addOrder", { 
                          orderId: this.getOrderNumber(),
                          details: {
                            x: this.location.x,
                            y: this.location.y,
                            fullname: this.fullname,
                            email: this.email,
                            gender: this.gender,
                            payment: this.payment
                          },
                          orderItems: this.orderedBurgers
                  });
      console.log("Order sent to server:", this.location);
    },
    addToOrder: function (event) {
      this.orderedBurgers[event.name] = event.amount;
      console.log("Ordered burgers:", this.orderedBurgers);
    },
  }
}
</script>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Agbalumo&family=Cormorant:wght@700&display=swap');
  body {
      font-size: 12pt;
  }

  p {
      color: rgb(0, 0, 0);
  }

  h1 {
      font-family: 'Poppins';
      font-size: 36pt;
  }
  main, header, footer, nav ul {
      max-width: 100rem;
      margin: 0 auto 0 auto;
  }
  main {
      background-color: rgb(255, 255, 255);
  }

  footer {
      border-top: 2px solid #ccc;   
      margin-top: 10px;             
      padding: 10px 0;             
      font-size: 16px;              
      color: #333;             
      text-align: left;         
      width: 100%;
  }

  /* nav ul li {
      display: inline-block;
      background-color: grey;
      padding: 1em;
      margin: 1em;
  } */

  /*header {
      background-image: url("../img/polacks.jpg");
      background-size: cover;
      overflow: hidden;
      width: 100%;
      height: 200px;
      opacity: 0.5;
      position: relative
  }

  header h1 {
      width:40rem;
      margin: 0 auto;
      text-align: center;
  }*/

  nav ul {
      display: grid;
      grid-template-columns: repeat(auto-fill, 9.25em);
      gap: 1em;
      padding: 0;
  }

  nav li {
      display: block;
      background-color: grey;
      padding: 1em;
  }

  .Very-good {
      color: green;
  }

  .Master {
      color: green;
      font-weight: bold;
  }

  .allergy {
      font-weight: bold;
  }

  #header-image {
      margin: 20px;
      overflow: hidden;
      position: relative;   
  }

  #header-image img {
      width: 100%;
      height: 80%;
      display: block;
      opacity: 0.5;
  }

  #header-image h1 {    
      position: absolute;
      left: 25%;
      margin-top: -500px;
      text-align: center;
  }

  button {
      background-color: #eee;
      border: 2px solid #999;
      padding: 10px;
      font-size: 16px;
      border-radius: 6px;
      cursor: pointer;
      margin: 20px;
  }

  .button:hover {
      background-color: #555555;
      color: white;
  }

  section {
      margin: 20px;
      padding: 10px;    
      border: 5px dashed #696868;
      border-radius: 10px;
  }

  #burger-section {
      background-color: #111;
      color: white;
  }

  .burger-grid {
      display: grid;
      grid-template-columns: 500px 500px 500px;
      gap: 20px;
      text-align: left;
  }

  #map-container {
      width: 1000px;     
      height: 700px; 
      overflow: scroll;
  }

  #map {
      width: 1920px;
      height: 1078px;
      position: relative;
      background: url("/img/polacks.jpg");
  }
  
  .dot {
    position: absolute;
    background: black;
    color: white;
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;
  }

  #dots {
    position: relative;
  }
</style>