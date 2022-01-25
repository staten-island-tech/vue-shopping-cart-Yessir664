<template>
  <div id="app">
    <div v-if="checkshow === false" class="homepage">
      <div class="nav">
      <img class="nav-item valorant-logo" src="@/assets/valorant-logo.png" />
      <img
        v-on:click="hidden = !hidden"
        class="nav-item cart-img"
        src="@/assets/shopping.png"
      />
      <div v-if="!hidden" class="cart-items-container">
        <table v-if="items.length > 0" class="item-table">
          <tr class="table-headers">
            <th></th>
            <th class="item-column">Name</th>
            <th class="item-column">Price Per</th>
            <th class="item-column">Quantity</th>
            <th class="item-column">Total</th>
          </tr>
          <tr class="item-row" v-for="item in items" :key="item.name">
            <th class="item-image-column"><img class="cart-item-img" :src="item.image" /> </th>
            <th class="item-column">{{ item.name }}</th>
            <th class="item-column">${{ item.price }}</th>
            <th class="item-column">{{ item.counter }}</th>
            <th class="item-column">${{ item.total }}</th>
            <th class="item-column"><button v-on:click="deleteItem(item.name)" class="delete-button">&times;</button></th>
          </tr>
          <tr>
            <th></th>
            <th></th>
            <th></th>
            <th></th>
            <th class="item-column">${{ grandTotal() }}</th>
          </tr>
          <tr>
            <th></th>
            <th></th>
            <th></th>
            <th></th>
            <th class="item-column"><button v-on:click="checkshow = true" class="checkout-button">Checkout</button></th>
          </tr>
        </table>
        <h1 class="no-items" v-else>No items added</h1>
      </div>
    </div>
    <div class="agents-container">
      <Card
        v-for="agent in agents"
        :key="agent.name"
        :name="agent.name"
        :image="agent.icon"
        :price="agent.price"
        :addItem="addItem"
      />
    </div>
    </div>
    <Checkout v-if="checkshow === true" :grandTotal="grandTotal" :items="items" :homeShow="homeShow"/>
  </div>
</template>
<script>
import Card from "@/components/Card.vue";
import agents from "../assets/agents";
import Checkout from "@/components/Checkout.vue"

export default {
  name: "Home",
  components: {
    Card,
    Checkout
  },
  data() {
    return {
      agents: agents.agents,
      counter: 0,
      items: [],
      hidden: true,
      checkshow: false
    };
  },
  methods: {
    addItem: function (name, price, image) {
      console.log(name, price);
      let found = false;
      this.counter += price;
      this.items.forEach(function (el) {
        if (el.name === name) {
          found = true;
          el.counter++;
          el.total = el.price * el.counter;
        }
      });
      if (found === false) {
        this.items.push({
          name: name,
          counter: 1,
          total: price,
          price: price,
          image: image
        });
      }
    },
    grandTotal: function(){
      let total = 0;
      this.items.forEach((el) => total += el.total)
      return total
    },
    homeShow: function(){
      this.checkshow = false
    },
    deleteItem: function(name){
      for(let i = 0; i < this.items.length; i++){
        if(this.items[i].name === name){
          this.items.splice(i, 1);
          break
        }
      }
    }
  },
};
</script>

<style>
.valorant-logo {
  width: 7%;
  margin-top: 1.5rem;
  margin-left: 3rem;
}

.agents-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  margin: 20px;
}

.nav {
  border: solid #fd4556 1.5rem;
  width: 98%;
  height: 10rem;
  background-color: black;
}

.cart-img {
  width: 7%;
  float: right;
  margin-top: 0.5rem;
  margin-right: 1rem;
}

.cart-img:hover {
  cursor: pointer;
  transform: scale(1.02);
}

.cart-items-container{
  border: solid black 10px;
  position: absolute;
  right: 5%;
  background-color: white;
  padding: 1rem;
}

.cart-item-img{
  width: 5rem;
}

.item-column{
  padding: 0.5rem;
}

.table-headers{
  font-size: 1.5em;
}

.checkout-button{
  margin-top: 2rem;
  border: none;
  border-radius: 12px;
  padding: 0.5rem;
  background-color: #007bff;
  color: white;
  text-decoration: none;
}

.checkout-button:hover{
  cursor: pointer;
}

.delete-button{
  border: none;
  background-color: #d11a2a;
  border-radius: 100%;
  color: white;
  font-size: 1em;
}

.delete-button:hover{
  cursor: pointer;
}

@media (max-width: 1700px) {
  .nav-item {
    width: 12%;
  }
  .valorant-logo {
    margin-left: 2rem;
  }
}

@media (max-width: 1024px) {
  .nav-item {
    width: 17%;
  }
  .cart-items-container{
    border: solid black 10px;
    position: absolute;
    right: 3%;
    background-color: white;
    padding: 0.5rem;
  }

  .cart-item-img{
    width: 4rem;
  }

  .item-column{
    padding: 0.2rem;
  }

  .table-headers{
    font-size: 1.3em;
  }

  .checkout-button{
    margin-top: 1rem;
  }
}

@media (max-width: 780px) {
  .nav-item {
    width: 25%;
  }
}

@media (max-width: 630px) {
  .nav-item {
    width: 20%;
  }
  .nav {
    border: solid #fd4556 1rem;
    height: 7rem;
  }
}
</style>
