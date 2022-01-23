<template>
  <div id="app">
    <div class="nav">
      <img class="nav-item valorant-logo" src="@/assets/valorant-logo.png" />
      <img class="nav-item cart-img" src="@/assets/shopping.png" />
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
    <p>{{ counter }}</p>
  </div>
</template>

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
@media(max-width: 1700px){
  .nav-item {
    width: 12%;
  }
  .valorant-logo {
    margin-left: 2rem;
  }
}

@media(max-width: 1024px){
  .nav-item {
      width: 17%;
    }
}

@media(max-width: 780px){
  .nav-item{
    width: 25%;
  }
}

@media(max-width: 630px){
  .nav-item{
    width: 20%;
  }
  .nav {
  border: solid #fd4556 1rem;
  height: 7rem;
}
}
</style>

<script>
import Card from "@/components/Card.vue";
import agents from "./assets/agents";

export default {
  name: "App",
  components: {
    Card,
  },
  data() {
    return {
      agents: agents.agents,
      counter: 0
    };
  },
  methods: {
    addItem: function(price){
      this.counter += price;
    }
  }
};
</script>
