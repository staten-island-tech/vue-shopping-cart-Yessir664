<template>
  <div class="agent-card">
    <h1 class="agent-name">{{ name }}</h1>
    <img class="agent-icon" :src="image" />
    <h2 class="agent-price">${{ price }}</h2>
    <button v-on:click="addItem(name, price)" class="add-button">
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

.add-button:hover {
  cursor: pointer;
}

@media (max-width: 1024px) {
  .agent-icon {
    width: 10rem;
  }
}

@media (max-width: 780px) {
  .agent-icon {
    width: 10rem;
  }
  .agent-card {
    margin: 15px;
    text-align: center;
  }
}

@media (max-width: 630px) {
  .agent-icon {
    width: 6rem;
  }
  .agent-card {
    text-align: center;
  }
}
</style>
