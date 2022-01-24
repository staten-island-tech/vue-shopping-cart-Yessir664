<template>
  <div class="about">
    <h1>{{ counter }}</h1>
  </div>
</template>

<script>
import Home from './Home.vue'

export default {
  name: "About",
  data(){
    return{
      counter: Home.counter
    }
  }
}
</script>
