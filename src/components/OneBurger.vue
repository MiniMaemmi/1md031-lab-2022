<template>
    <div>
      
          <h1>{{burger.name}}</h1> 
            
              <img v-bind:src="burger.url" alt="Bruger on fiire" style="width:500px;height:500px;">

            <div style="width:800px; margin:0 auto; margin-left: 50px; margin-bottom: 25px;">
              <button v-on:click="IncreaseBurger">+ </button> 
              <button v-on:click="DecreaseBurger">-</button>
              Amount {{burger.orderedBurgers}}
            </div>
          
       



    </div>

    <!--div style="width:800px; margin:0 auto; margin-left: 50px; margin-bottom: 25px;">
              <button v-on:click="DecreaseBurger"> - </button>
              {{ amountOrdered }}
              <button v-on:click="IncreaseBurger"> + </button-->



    








  </template>
  
  <script>
  export default {
    name: 'OneBurger',
    props: {
      burger: Object
      
    },
    data:function(){
      return{amountOrdered:0}
    },

    methods:{
      IncreaseBurger:function(){
        this.amountOrdered=this.amountOrdered+1
        this.$emit('orderedBurger', { name: this.burger.name, 
                                  amount: this.amountOrdered 
                              }
  );
  },
      DecreaseBurger:function(){
       if (this.amountOrdered >0){
         this.amountOrdered=this.amountOrdered-1}
         this.$emit('orderedBurger', { name:   this.burger.name, 
                                  amount: this.amountOrdered 
                              }
  );
  },

    }
  }
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
  
  </style>
  