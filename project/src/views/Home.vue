<template>
  <div id="app">
    <div v-if="pageShow === `home`" class="homepage">
      <div class="nav">
        <div class="nav-left">
          <img class="nav-item valorant-logo" v-on:click="roleClick(`all`)" src="@/assets/valorant-logo.png" />
          <div class="role-buttons">
            <div class="nav-role">
              <div
            v-for="role in roles" 
            :key="role" 
            v-on:click="roleClick(role)" 
            class="role-div"
            >
            <button class="role-button">{{ role }}</button>
            </div>
            </div>
            
            
          </div>
        </div>
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
            <th class="delete-column item-column"><button v-on:click="deleteItem(item.name)" class="delete-button">&times;</button></th>
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
            <th class="item-column"><button v-on:click="pageShow = `check`" class="checkout-button">Checkout</button></th>
          </tr>
        </table>
        <h1 class="no-items" v-else>No items added</h1>
      </div>
    </div>
    <div class="agents-container">
      <Card
        v-for="agent in agentDisplay"
        :key="agent.name"
        :name="agent.name"
        :image="agent.icon"
        :price="agent.price"
        :addItem="addItem"
        :buyAgentClick="buyAgentClick"
        :homeShow="homeShow"
      />
    </div>
    </div>
    <Checkout v-if="pageShow === `check`" :grandTotal="grandTotal" :items="items" :homeShow="homeShow"/>
    <Buy v-if="pageShow === `buy`" :key="buyAgent" :homeShow="homeShow" :agent="buyAgent"/>
  </div>
</template>
<script>
import Card from "@/components/Card.vue";
import Checkout from "@/components/Checkout.vue"
import agents from "../assets/agents";
import Buy from "@/components/Buy.vue"

export default {
  name: "Home",
  components: {
    Card,
    Checkout,
    Buy
  },
  data() {
    return {
      agents: [],
      counter: 0,
      items: [],
      hidden: true,
      pageShow: "home",
      roles: [
        "Duelist",
        "Controller",
        "Initiator",
        "Sentinel"
      ],
      agentDisplay: [],
      buyAgent: {},
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
      this.pageShow = "home"
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
      this.agentDisplay = this.agents.filter((el) => el.role.displayName === role);
      
    },
    async getData() {
      try {
        let response = await fetch("https://valorant-api.com/v1/agents");
        let data = await response.json()
        data = data.data.filter(el => el.role != null)
        console.log(data)

        this.agents = data.map(function(el){
          const price = agents.agents.filter((agent) => agent.name === el.displayName)[0].price;
          return {
            name: el.displayName,
            icon: el.displayIcon,
            price: price,
            abilities: el.abilities,
            portrait: el.fullPortrait,
            role: el.role,
            description: el.description
          }
        })
        this.agentDisplay = this.agents
        console.log(this.agents)
      } catch (error) {
        console.log(error);
      }
    },
    buyAgentClick: function(name){
      console.log(name)
      this.buyAgent = this.agents.filter(el => el.name === name)[0]
      this.pageShow = "buy"
      console.log(this.buyAgent)
    }
  },
  created() {
    this.getData();
  },
};
</script>

<style>
@font-face {
    font-family: Valorant;
    src: url(../assets/Valorant-Font.ttf);
}

body{
  background: url("../assets/agent-background.png") no-repeat center center fixed; 
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover;
}

button{
  cursor: pointer;
}

.valorant-logo {
  margin-top: 2rem;
  margin-left: 2rem;
  height: 6rem;
}

.valorant-logo:hover{
  cursor: pointer;
}

.agents-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  margin: 2rem;
}

.nav {
  display: flex;
  border: solid #fd4556 1.5rem;
  width: 98%;
  height: 10rem;
  background-color: black;
  justify-content: space-between;
}

.nav-left{
  display: flex;
}
.nav-role{
  display: flex;
}
.role-buttons{
  margin-top: 4rem;
}


.role-button{
  background-color: transparent;
  color: #ede9e2;
  font-family: Valorant;
  font-size: 2em;
  border: none;
  border-right: 3px solid #fd4556;
  padding-left: 0.5rem;
  padding-right: 0.8rem;
}
.role-div{
  display: flex;
}
.line-break{
  color: #fd4556;
  font-size: 2em;
}
.cart-img {
  float: right;
  height: 5rem;
  margin-top: 2.5rem;
  margin-right: 3rem;
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
  top: 10rem;
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

.delete-button{
  border: none;
  background-color: #d11a2a;
  border-radius: 100%;
  color: white;
  font-size: 1em;
}

.delete-column{
  padding: 0;
}

@media (max-width: 1700px) {
  .valorant-logo {
    margin-left: 2rem;
  }

  .role-button{
    font-size: 1.5em;
  }
}

@media (max-width: 1024px) {
  .role-buttons{
    margin-top: 3rem;
  }
  .item-column{
    padding: 0.2rem;
  }

  .valorant-logo{
    margin-top: 1rem;
    margin-left: 1rem;
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
  .agents-container{
    margin: 1rem;
  }
  .cart-items-container{
    top: 7rem;
  }

  .item-column{
    padding: 0;
    padding-right: 0.2rem;
    font-size: 0.6em;
  }
  .cart-img {
    float: right;
    height: 3rem;
    margin-top: 1.5rem;
    margin-right: 1.5rem;
  }
  .delete-button{
    text-align: center;
    width: 15px;
    height: 15px;
  }

  .role-buttons{
    margin-top: 3.5rem;
  }
  .role-button{
    font-size: 0.7em;
  }
  .checkout-button{
    border-radius: 7px;
    font-size: 1em;
    padding: 0.3rem;
  }

  .nav-item{
    height: 4rem;
  }
}

@media (max-width: 640px) {
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