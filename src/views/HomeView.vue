<template>
  <div>
      <div>
        Burgers
          <!--Burger v-for="burger in burgers"
                v-bind:burger="burger" 
                v-bind:key="burger.name"/-->
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
        <form>
          <input type="text" id="firstname" v-model="formdata.firstname" required="required" placeholder="Ditt första Namn">
            
            
          <input type="email" id="email" v-model="formdata.email" required="required" placeholder="E-mail address">
            <!--input v-model="streetadress" required="required" placeholder="Street Adress" />
            <input v-model="number" required="required" placeholder="House Number"-->
          </form>
            
      
    
<h1>
    Customer Information  
</h1>


</section>

<section>
    <h1>
        Payment Alternatives
    </h1>
      <p>
        
        <label for="paymentoption" id="paymentoption123">Payment Options</label><br>
          <select id="paymentoption" v-model="formdata.paymentoption"><!--tappade mina förvalda inställningar, och de verkar inte vilja komma tillbaka :()-->
            <option>Endless Suffering </option>
            <option>Percentage of Soul(direct)</option>
            <option>Unborn Child(Terms apply)</option>
            <option>Paypal</option>
            <option>Overused Creditcard</option>
          </select>
      </p>


    <div class="genderoptionbutton">

    <input type="radio" id="MAN" value="MAN" checked v-model="formdata.gender">
      <label for="MAN">MAN</label>
    <input type="radio" id="WOMAN" value="WOMAN" v-model="formdata.gender">
      <label for="MAN">WOMAN</label>
    <input type="radio" id="OTHER/DO NOT WISH TO PROVIDE" value="DO NOT WISH TO PROVIDE" v-model="formdata.gender">
      <label for="OTHER/DO NOT WISH TO PROVIDE">OTHER/DO NOT WISH TO PROVIDE</label>
        </div>
      


</section>
<div style="width:800px; margin:0 auto; margin-left: 50px; margin-bottom: 25px;">
  <button v-on:click="sendInfo()" id="placeOrderButton" type="submit" value="Place Order"> 
    Send Order
    <img src="https://manuals.fibaro.com/wp-content/uploads/2019/05/download.png" style="width:100px;height:100px;">
  </button>

</div>

<div id="mapclass">
      
      <div id="map" v-on:click="setLocation($event)">
      
        <div id="dots" v-bind:style="{position:'relative', background: 'url(' + require('../../public/img/polacks.jpg')+ ')' }">
                 
                    <div v-bind:location="location" v-bind:style="{ left: this.location.x + 'px', top: this.location.y + 'px', position: 'absolute'}" v-bind:key="'dots'">
                      T
                    </div>
                </div>
              
  </div>    
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




export default {
  
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      formdata:{firstname:"",email:"",gender:"",paymentoption:""}, 
      selectedBurgers: [],
      burgers: menu,
      //firstname:"",
      //streetadress:"",
      //email:"",
      //number:"",
      amountOrdered:0,
      orderedBurgers:{},
      location: { x: 0,
            y: 0
          },
      

      
    }
  },
  methods: {

    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    sendInfo: function () { //de här namnet borde bytas till något som har med order att göra men men//
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: this.location.x,
                                           y: this.location.y },
                                orderItems: this.orderedBurgers,
                                customer:{
                                  firstname:this.formdata.firstname,
                                  email: this.formdata.email,
                                  gender:this.formdata.gender,
                                  paymentoption:this.formdata.paymentoption

                                }
                              }
                 );
      
    },
    setLocation: function(event){
    var offset = {x: event.currentTarget.getBoundingClientRect().left,
                  y: event.currentTarget.getBoundingClientRect().top};
    this.location.x=event.clientX - 10 - offset.x,
    this.location.y=event.clientY - 10 - offset.y},
  
    

    addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y },
                                orderItems: [this.orderedBurgers],
                                customerData: [this.formdata]
                              }
                 );
      this.location.x = event.clientX;
      this.location.y = event.clientY;
                            },
  
  addToOrder:function(event){
    this.orderedBurgers[event.name] = event.amount;
    console.log(this.orderedBurgers)
  },
  },
  
}
//oanvänd constructor
//function MenuItem(name, kCal, lactose, gluten, url, ) {
  //  this.name = name;
    //this.kCal = kCal;
    //this.lactose=lactose;
    //this.gluten=gluten;
    //this.url = url;
    
//}






</script>

<style>
  #map {
    width: 1920px;
    height: 1078px;
    background: url('../../public/img/polacks.jpg');
        
  }
  #mapclass{
  overflow: scroll;
  height:800px;
  width:800px;
  
}

#dots {
    position: relative;
    margin: 0;
    padding: 0;
    background-repeat: no-repeat;
    width:1920px;
    height: 1078px;
    cursor: crosshair;
    
  }
  #dots div {
    position: absolute;
    background: rgb(253, 1, 1);
    color: white;
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;
    
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