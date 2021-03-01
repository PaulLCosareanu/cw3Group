<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <ProductComponent v-bind:list="list" msg="Welcome to Your Vue.js App" v-on:checkout="checkoutUpdate" v-on:displaySettings="setDisplay"/>
   <!-- code to use the imported checkout -->
      <checkoutComponent v-bind:list="list" v-on:updatedCheckout="checkoutUpdate" v-on:displaySettings="setDisplay"/>
  </div>
</template>

<script>
import ProductComponent from './components/ProductsComponent.vue'
import checkoutComponent from './components/checkout.vue'


export default {
  name: 'App',
  components: {
    ProductComponent,
    checkoutComponent
  },
  data(){
    return{
    list:{
      lessons: [],
      showProducts: true,
      checkoutItems: [],
      showCheckoutButton: false,
      showCheckout: false,
      showSuccess: false,
      showError: false,
      showSubmitButton: false,
    },
    checkout:{
     
    
     
     
     
    },
    
    
    }
  },
  methods:{
    checkoutUpdate(checkout){
      this.checkout.checkoutItems=checkout;
      this.list.checkoutItems=checkout;
    },
    setDisplay(displaySettings){
      this.list.showProducts=displaySettings.showProducts;
      this.list.showCheckoutButton=displaySettings.showCheckoutButton;
      this.list.showCheckout=displaySettings.showCheckout;
      this.list.showSuccess=displaySettings.showSuccess;
      this.list.showError=displaySettings.showError;
      this.list.showSubmitButton=displaySettings.showSubmitButton;
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
