<template>
  <div class="checkout">
      <button v-on:click="homeShow()" class="back-button">
        &lt;&lt;Back
      </button>
    <table class="item-table">
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
          </tr>
          <tr>
            <th></th>
            <th></th>
            <th></th>
            <th></th>
            <th class="item-column">${{ grandTotal() }}</th>
          </tr>
        </table>
  </div>
</template>

<script>

export default {
  name: "About",
  props: {
    items: Array,
    homeShow: Function,
    grandTotal: Function
  }
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Noto+Serif&display=swap');
  .item-table{
    margin: auto;
  }

  .item-column{
    font-size: 1.5em;
    padding: 2rem;
  }
  .cart-item-img{
    width: 8rem;
  }
  .back-button{
    border: none;
    background-color: transparent;
    font-size: 3em;
    font-family: 'Noto Serif', serif;
  }

  .back-button:hover{
    cursor: pointer;
  }


</style>
