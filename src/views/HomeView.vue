<template>
  <div>
      <div>
        Burgers
          <!--Burger v-for="burger in burgers"
                v-bind:burger="burger" 
                v-bind:key="burger.name"/-->
      </div>
      <div id="map" v-on:click="addOrder">
        click here
      </div>
  

<section class="BURGERSELECT">
    <h1 id="SELECTBURGERHEADER"> Välkommen till BurgerOnline </h1>
            
</section>



<section class="BurgerSection">
    <h1 style="font: size 300%;">Select Your Burger </h1>
    <div id="wrapper">
      <Burger v-for="burger in burgers"
                v-bind:burger="burger" 
                v-bind:key="burger.name"
                v-on:orderedBurger="addToOrder($event)"/>
                
    </div>
    

              
              
    
            
</section>

<section class="OrderInformation">
    <h1>
        Customer Information
    </h1>
  
            <input v-model="firstname" required="required" placeholder="First Name" />
            
            <input v-model="email" required="required" placeholder="E-mail adress" />
            
            <input v-model="streetadress" required="required" placeholder="Street Adress" />
            
            <input v-model="number" required="required" placeholder="House Number">
            
            
      
    
<h1>
    Customer Information  
</h1>


</section>

<section>
    <h1>
        Payment Alternatives
    </h1>
      <p>
        <label for="Payment Option">Payment Options</label><br>
          <select id="Payment Option" name="rcp">
            <option>Endless Suffering </option>
            <option>Percentage of Soul(direct)</option>
            <option>Unborn Child(Terms apply)</option>
            <option>Paypal</option>
            <option>Overused Creditcard</option>
          </select>
      </p>


    <div>
    <input type="radio" id="MAN" name="GENDER" value="MAN" checked>
      <label for="MAN">MAN</label>
    <input type="radio" id="WOMAN" name="GENDER" value="WOMAN">
      <label for="MAN">WOMAN</label>
    <input type="radio" id="OTHER/DO NOT WISH TO PROVIDE" name="GENDER" value="OTHER/DO NOT WISH TO PROVIDE">
      <label for="OTHER/DO NOT WISH TO PROVIDE">OTHER/DO NOT WISH TO PROVIDE</label>
        </div>
      


</section>
<div style="width:800px; margin:0 auto; margin-left: 50px; margin-bottom: 25px;">
  <button v-on:click="sendInfo"> 
    Send Order
    <img src="https://manuals.fibaro.com/wp-content/uploads/2019/05/download.png" style="width:100px;height:100px;">
  </button>

</div>


<footer>
        Alla försök till att inskränka på BurgerOnline kommer att bemötas med en direkt-skickad <span class="bolded">ICBM</span>
        
    </footer>
</div>


</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io();

/*function MenuItem(name,kCal, url, lactose, gluten){
  this.name=name;
  this.kCal=KCal;
  this.lactose;
  this.gluten;
  
    
}*/






export default {
  
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: menu,
      firstname:"",
      streetadress:"",
      email:"",
      number:"",
      amountOrdered:0,
      orderedBurgers:{},
      

      
    }
  },
  methods: {

    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y },
                                orderItems: ["Beans", "Curry"]
                              }
                 );
    },
    sendInfo: function(){
    console.log("hejhej"),
    console.log(this.firstname)
    

  },

  
  addToOrder:function(event){
    this.orderedBurgers[event.name] = event.amount;
    console.log(this.orderedBurgers)
  },
  },
  
}
</script>

<style>
  #map {
    width: 300px;
    height: 300px;
    background-color: red;
  }
  @import 'https://fonts.googleapis.com/css?family=Pacifico|Dosis';
  /*@import url("reset.css");*/
  body {
    font-family: arial;
    font-size: 12pt;

}
 .BURGERSELECT{
    background-image: url("https://wallpaperaccess.com/full/104879.jpg");
    opacity: 0.6;
    width:auto;height:250px;
    overflow:hidden

 }
 #SELECTBURGERHEADER {
  
    width: 100%;
    height:auto;
    font-size: 25pt;
    position: absolute;
    color: white;
    text-align: center;
    overflow: hidden;
}




.BurgerSection{
background-color: rgb(255, 255, 255);
color: rgb(0, 0, 0);
border: 21px double #ff0000;;
}




.OrderInformation{
    background-color: rgb(8, 8, 8);
    color: rgb(98, 0, 255);
    border: 21px dotted #3903fa ;
    border-radius:25px ;
}

.BLACKBACKGROUNDTHINGY{
        background-color: rgb(11, 25, 235);
         color: white;
}   

   


.bolded { font-weight: bold; }


button {
        transition-duration: 0.4s;
      }
      
.button:hover {
        background-color: #4CAF50; /* Green */
        color: white;
      }
     
#wrapper {
        display: grid;   
        grid-template-columns: 1fr 1fr 1fr;
        grid-template-rows: 1fr;
    }
   
</style>