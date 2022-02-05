<template>
  <div class="buy-container">
    <button v-on:click="homeShow()" class="back-button">
      &lt;&lt;Back
    </button>
    <div class="agent-container">
        <img class="agent-img" :src="agent.portrait">
        <div class="agent-abilities">
          <div class="agent-role"><h4>{{ agent.role.displayName}}</h4></div>
          <div class="agent-name">{{ agent.name }}</div>
          <div class="buttons">
              <button v-on:click="infoClick()" class="button"><img class="button-img" :src="agent.role.displayIcon"></button>
              <button  class="button" v-for="ability in agent.abilities" :key="ability" v-on:click="abilClick(ability)"><img class="button-img" :src="ability.displayIcon"></button>
          </div>
          <div class="agent-info-display">
              <p v-if="agentDescription === true" class="agent-description">{{ agent.description }}</p>
              <p :key="abilName" class="abil-name">{{ abilName }}</p>
              <p :key=" displayInfo">{{ displayInfo }}</p>
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
  },
  data() {
      return{
          agentDescription: true,
            abilName: this.agent.role.displayName,
            displayInfo: this.agent.role.description
      }
  },
  methods: {
      infoClick: function(){
        this.agentDescription = true
        this.abilName = this.agent.role.displayName
        this.displayInfo = this.agent.role.description
      },
      abilClick: function(ability){
        this.agentDescription = false
        this.abilName = ability.displayName
        this.displayInfo = ability.description
      }
  }
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Oswald&display=swap');

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


.back-button{
    border: none;
    background-color: transparent;
    font-size: 3em;
    font-family: 'Noto Serif', serif;
    color: white;
    position: absolute;
    margin: 1rem;
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
  width: 50%;
  height: 50%;
}

.buttons {
  display: flex;
}

.agent-abilities {
  width: 50%;
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
  font-family: 'Oswald', sans-serif;
}

.agent-role{
  color: white;
  font-family: 'Oswald', sans-serif;
  font-size: 2em;
}

.agent-description{
  color: #fdf6e3;
}

.abil-name{
  font-size: 3em;
  font-family: Impact, Haettenschweiler, "Arial Narrow Bold", sans-serif;
}
@media (max-width: 1024px) {
  .agent-img {
    width: 70%;
  }
}
@media (max-width: 650px) {
  .agent-container {
    flex-direction: column;
  }
  .agent-img {
    width: 100%;
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
}

</style>
