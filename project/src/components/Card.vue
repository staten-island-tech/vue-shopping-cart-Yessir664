<template>
  <div v-on:click="buyAgentClick(name)" class="agent-card">
    <h1 class="agent-name">{{ name }}</h1>
    <img class="agent-icon" :src="image" />
    <h2 class="agent-price">${{ price }}</h2>
    <button v-on:click="addItem(name, price, image)" class="add-button">
      Add to Cart
    </button>
  </div>
</template>

<script>
export default {
  name: "Card",
  props: {
    name: String,
    image: String,
    price: Number,
    addItem: Function,
    buyAgentClick: Function,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.agent-icon {
  width: 15rem;
}

.agent-card {
  margin: 40px;
  text-align: center;
  background-color: white;
  padding: 2rem;
  padding-left: 3rem;
  padding-right: 3rem;
  border-radius: 1rem;
}

.add-button {
  border: none;
  background-color: black;
  color: white;
  padding: 1rem;
  padding-left: 2rem;
  padding-right: 2rem;
  border-radius: 12px;
}

.agent-card:hover {
  cursor: pointer;
}

@media (max-width: 1325px) {
  .agent-card{
    padding: 2rem;
    margin: 1.5rem;
  }
}
@media (max-width: 1100px) {
  .agent-icon {
    width: 10rem;
  }
  .agent-card{
    padding: 1.5rem;
    margin: 1rem;
  }
}

@media (max-width: 800px) {
  .agent-icon {
    width: 10rem;
  }
  .agent-card {
    margin: 1rem;
  }

  .agent-icon{
    width: 7rem;
  }

  .add-button{
    font-size: 0.75em;
    padding: 1.5rem;
    padding-top: 0.75rem;
    padding-bottom: 0.75rem;
  }
}

@media (max-width: 630px) {
  .agent-icon {
    width: 6rem;
  }
  .agent-card{
    padding: 0.75rem;
  }
}

@media (max-width: 576px) {
  .agent-icon {
    width: 4rem;
  }

  .agent-card{
    font-size: 0.6em;
  }

  .add-button {
    font-size: 0.7em;
    padding: 0.5rem;
    padding-left: 1rem;
    padding-right: 1rem;
    border-radius: 12px;
  }
}
</style>
