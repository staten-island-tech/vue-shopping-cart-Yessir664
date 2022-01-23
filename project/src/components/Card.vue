<template>
  <div class="agent-card">
    <h1 class="agent-name">{{ name }}</h1>
    <img class="agent-icon" :src="image" />
    <h2 class="agent-price">${{ price }}</h2>
  </div>
</template>

<script>
export default {
  name: "Card",
  props: {
    name: String,
    image: String,
    price: Number,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.agent-icon {
  width: 20rem;
}
.agent-card {
  margin: 40px;
  text-align: center;
}
</style>
