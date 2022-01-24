<template>
  <div class="about">
    <h1>{{ $route.params.items }}</h1>
    <table class="item-table">
          <tr class="table-headers">
            <th></th>
            <th class="item-column">Name</th>
            <th class="item-column">Price Per</th>
            <th class="item-column">Quantity</th>
            <th class="item-column">Total</th>
          </tr>
          <tr class="item-row" v-for="item in $route.params.items" :key="item.name">
            <th class="item-image-column"><img class="cart-item-img" :src="item.image" /> </th>
            <th class="item-column">{{ item.name }}</th>
            <th class="item-column">${{ item.price }}</th>
            <th class="item-column">{{ item.counter }}</th>
            <th class="item-column">${{ item.total }}</th>
          </tr>
          <tr>
            <th></th>
            <th></th>
            <th></th>
            <th></th>
          </tr>
          <tr>
            <th></th>
            <th></th>
            <th></th>
            <th></th>
            <th class="item-column"><router-link class="checkout-button" :to="{
              name: 'Checkout',
              params: { items:items }
            }">Checkout</router-link></th>
          </tr>
        </table>
  </div>
</template>

<script>

export default {
  name: "About",
  data(){
    return{
    }
  },
  methods:{
  }
}
</script>
