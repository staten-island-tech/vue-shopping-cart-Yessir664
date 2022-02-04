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
        v-for="agent in agentDisplay"
        v-bind:key="agent.name"
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
import Checkout from "@/components/Checkout.vue"
import agents from "../assets/agents";

export default {
  name: "Home",
  components: {
    Card,
    Checkout
  },
  data() {
    return {
      agents: [],
      counter: 0,
      items: [],
      hidden: true,
      checkshow: false,
      roles: [
        "Duelist",
        "Controller",
        "Initator",
        "Sentinel"
      ],
      agentDisplay: [],
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
    },
    roleClick: function(role){
      if(role === "all"){
        this.agentDisplay = this.agents;
        return
      }
      this.agentDisplay = this.agents.filter((el) => el.role === role);
      
    },
    async getData() {
      try {
        let response = await fetch("https://valorant-api.com/v1/agents");
        let data = await response.json()
        console.log(data.data)
        this.agents = data.data.map(function(el){
          const price = agents.agents.filter((agent) => agent.name === el.displayName)[0].price;
          return {
            name: el.displayName,
            icon: el.displayIcon,
            price: price,
            abilities: el.abilities,
            portrait: el.fullPortrait,
            role: el.role,
          }
        })
        this.agentDisplay = this.agents
        console.log(this.agents)
      } catch (error) {
        console.log(error);
      }
    },
  },
  created() {
    this.getData();
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
  .cart-img{
    margin-top: 1rem;
    margin-right: 1.5rem;
  }

  .table-headers{
    font-size: 1.3em;
  }

  .checkout-button{
    margin-top: 1rem;
  }
  
  .role-button{
    font-size: 1em;
  }
  .nav {
    border: solid #fd4556 1rem;
    height: 7rem;
  }
  .nav-item{
    margin-top: 1.5rem;
    height: 4rem;
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
  .role-button{
    border-right: 2px solid #fd4556;
  }
  .nav-item{
    height: 3.5rem;
    margin-top: 1.75rem;
  }
  .agents-container {
    margin: 10px;
  }
}

@media (max-width: 576px) {
  .nav-item{
    height: 3rem;
    margin-top: 2rem;
  }

  .valorant-logo{
    margin-left: 0.5rem;
  }

}

@media (max-width: 475px) {
  .role-button{
    padding: 0;
    padding-left: 0.3rem;
    padding-right: 0.3rem;
    font-size: 0.6em;
  }
}

@media (max-width: 410px) {
  .role-button{
    padding: 0;
    padding-left: 0.3rem;
    padding-right: 0.3rem;
    font-size: 0.6em;
  }
  .cart-img{
    height: 2rem;
    margin-right: 1rem;
    margin-top: 2.5rem;
  }
}
</style>
