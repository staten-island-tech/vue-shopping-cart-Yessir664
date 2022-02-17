<template>
  <div class="buy-container">
    <button v-on:click="homeShow()" class="back-button">&lt;&lt;Back</button>
    <div class="agent-container">
      <img class="agent-img" :src="agent.portrait" :alt="agent.name"/>
      <div class="agent-abilities">
        <div class="agent-role">
          <h4>{{ agent.role.displayName }}</h4>
        </div>
        <div class="agent-name">{{ agent.name }}</div>
        <div class="buttons">
          <button v-on:click="infoClick()" class="button">
            <img class="button-img" :src="agent.role.displayIcon" :alt="agent.role.displayName"/>
          </button>
          <button
            class="button"
            v-for="ability in agent.abilities"
            :key="ability.displayName"
            v-on:click="abilClick(ability)"
          >
            <img class="button-img" :src="ability.displayIcon" :alt="ability.displayName"/>
          </button>
        </div>
        <div class="agent-info-display">
          <p v-if="agentDescription === true" class="agent-description">
            {{ agent.description }}
          </p>
          <p :key="abilName" class="abil-name">{{ abilName }}</p>
          <p :key="displayInfo">{{ displayInfo }}</p>
        </div>
      </div>
      <div class="cart-add">
        <div class="buy-area">
          <div class="price-show">
            <p class="small-price">$</p>
            <p class="dollar-amount">{{ agent.price }}</p>
            <p class="small-price">00</p>
          </div>
          <button
            v-on:click="addItem(agent.name, agent.price, agent.icon, quantity), homeShow(), alertCart()"
            class="add-button"
          >
            Add to Cart
          </button>

          <div class="quantity-form">
            <label :for="agent.name">Quantity: </label>
            <input
              class="quantity-input"
              :id="agent.name"
              type="number"
              min="1"
              v-model="quantity"
            />
          </div>
          <p class="total-show">Total: ${{ agent.price * quantity }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Buy",
  props: {
    agent: Object,
    homeShow: Function,
    addItem: Function,
  },
  data() {
    return {
      agentDescription: true,
      abilName: this.agent.role.displayName,
      displayInfo: this.agent.role.description,
      quantity: 1,
    };
  },
  methods: {
    infoClick: function () {
      this.agentDescription = true;
      this.abilName = this.agent.role.displayName;
      this.displayInfo = this.agent.role.description;
    },
    abilClick: function (ability) {
      this.agentDescription = false;
      this.abilName = ability.displayName;
      this.displayInfo = ability.description;
    },
    alertCart: function(){
      alert("Item added to cart")
    }
  },
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Oswald&display=swap");

:root {
  --h1: 7.594rem;
  --h2: 5.063rem;
  --h3: 3.375rem;
  --h4: 2.25rem;
  --h5: 1.5rem;
}

h1 {
  font-size: var(--h1);
}
h2 {
  font-size: var(--h2);
}
h3 {
  font-size: var(--h3);
}
h4 {
  font-size: var(--h4);
}
h5 {
  font-size: var(--h5);
}

.add-button {
  border: none;
  background-color: black;
  color: white;
  font-size: 75%;
  padding: 1rem 2rem;
  border-radius: 12px;
  margin: 3rem;
  margin-top: 1rem;
}

.back-button {
  border: none;
  background-color: transparent;
  font-size: 3em;
  font-family: "Noto Serif", serif;
  color: white;
  position: absolute;
  margin: 1rem;
}

.price-show {
  display: flex;
  margin-left: 1rem;
}

.dollar-amount {
  font-weight: bold;
  font-size: 2em;
}
.total-show {
  display: flex;
  justify-content: center;
}

.small-price {
  margin-top: 2rem;
}
.buy-area {
  border: solid white 1px;
  border-radius: 12px;
  background-color: white;
  display: flex;
  flex-direction: column;
  justify-content: center;
  margin: 4rem;
}

.quantity-input {
  margin-bottom: 4rem;
  margin-left: 3px;
  width: 33%;
}

.quantity-form {
  display: flex;
  justify-content: center;
}

.agent-name {
  color: white;
  font-family: Impact, Haettenschweiler, "Arial Narrow Bold", sans-serif;
  font-size: 6em;
}

.agent-container {
  display: flex;
}
.agent-img {
  width: 40%;
  height: 40%;
  margin-top: 4rem;
}

.cart-add {
  width: 33%;
}
.buttons {
  display: flex;
}

.agent-abilities {
  width: 33%;
  margin-left: 0px;
}

.button {
  background-color: black;
}
.button-img {
  width: 4rem;
}

.agent-info-display {
  color: white;
  width: 100%;
  font-size: 1.5em;
  font-family: "Oswald", sans-serif;
}

.agent-role {
  color: white;
  font-family: "Oswald", sans-serif;
  font-size: 2em;
}

.agent-description {
  color: #fdf6e3;
}

.abil-name {
  font-size: 3em;
  font-family: Impact, Haettenschweiler, "Arial Narrow Bold", sans-serif;
}

@media (max-width: 1024px) {
  .agent-img {
    width: 40%;
    margin-top: 10rem;
  }
  .agent-abilities {
    width: 33%;
    font-size: 0.75em;
  }
  .button-img {
    width: 2.5rem;
  }

  .add-button{
    font-size: 0.6em;
    margin: 1rem;
  }

  .buy-area{
    margin: 0;
    margin-top: 3rem;
  }

}
@media (max-width: 800px) {
  .agent-container {
    flex-direction: column;
  }
  .agent-img {
    width: 100%;
    margin-top: 4rem;
  }
  .button-img {
    width: 2rem;
  }

  h1 {
    font-size: 6em;
  }
  .agent-name {
    color: white;
    font-family: Impact, Haettenschweiler, "Arial Narrow Bold", sans-serif;
  }
  .agent-abilities {
    width: 100%;
    margin-left: auto;
    margin: 20px;
  }

  .cart-add{
    width: 66%;
    margin: auto;
    margin-bottom: 1rem;
  }

}
</style>
