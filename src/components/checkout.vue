<template>
    <div v-if="this.list.showCheckout">
           <div class="productList">
        <div
          v-for="element in this.list.checkoutItems"
          :key="element.id"
          class="productBought"
        >
          <div class="productTop">
            <img class="image" v-bind:src="element.img" />
            <div class="productText">
              <h3>{{ element.subject }}</h3>
              <p>{{ element.location }}</p>
              <p>price:{{ element.price }}</p>
              <p>Quantity: {{ element.quantity }}</p>
            </div>
          </div>
          <button v-on:click="deleteItem(element.id)" class="button">
            Cancel item
          </button>
        </div>
      </div>
      <button type="button" v-on:click="displayProducts()">Go back</button>
      <h3>Please specify your name:</h3>
      <input
        type="text"
        name="name"
        v-model="name"
        id="name"
        v-on:keypress="
          
          checkForm();
        "
        v-on:keyup="checkForm()"
      />
      <h3>Please specify your number:</h3>
      <input
        type="text"
        name="number"
        id="number"
        v-model="number"
        v-on:keypress="
          
          checkForm();
        "
        v-on:keyup="checkForm()"
        maxlength="11"
      />
      <p v-if="this.list.showError" style="color:red">
        Incomplete form or invalid characters entered!
      </p>
      <div v-if="this.list.showSubmitButton">
        <button type="button" v-on:click="checkout()">Submit Order</button>
      </div>
      <h1 v-if="this.list.showSuccess" style="color:green">
        Thank you for buying these products! Have a nice day
      </h1>
    </div>
</template>
<script>
export default {
  props:["list"],
  data() {
    return {
      name: "",
      number: "",
    };
  },
    methods:{
      checkForm() {
      if (this.number != "" && this.name != "") {
        this.list.showSubmitButton = true;
        console.log(this.number);
        this.list.showError = false;
      } else {
        this.list.showSubmitButton = false;
        this.list.showError = true;
      }
    },
    deleteItem(id) {
      for (let i = 0; i < this.list.checkoutItems.length; i++) {
        if (this.list.checkoutItems[i].id == id) {
          this.list.checkoutItems[i].quantity--;
          this.list.lessons.forEach((element) => {
            if (element._id == id) {
              element.quantity++;
            }
          });
          if (this.list.checkoutItems[i].quantity == 0) {
            this.list.checkoutItems.splice(i, 1);
          }
        }
      }
      localStorage.product = JSON.stringify(this.list.checkoutItems);
      this.$emit("updatedCheckout",this.list.checkoutItems)
    },
    displayProducts(){
      this.list.showCheckout = false;
      this.list.showProducts = true;
      
     
      this.$emit("displaySettings",this.list);
    },
      checkout(){
      let bodyUpdate={}
      this.list.checkoutItems.forEach(element => {
        const body={
          id:element.id,
          quantity:element.quantity,
          price:element.price,
          user:this.name,
          number:this.number
        }
        this.list.lessons.forEach(el => {
          if(el._id==element.id){
            bodyUpdate={
            id:element.id,
            $set:{quantity:el.quantity}
        }
          }
        });
        
        fetch("https://cw2heroku.herokuapp.com/api/products/order",{
          method:"post",
          body:JSON.stringify(body),
          headers:{"Content-Type":"application/json"}
  
        })
          .then((response) => {if(response.status==200){
            this.list.showSuccess=true;
            localStorage.clear();
            this.list.checkoutItems=[];
            

          }})
             try {
               
               fetch("https://cw2heroku.herokuapp.com/api/products/order",{
              method:"put",
              body:JSON.stringify(bodyUpdate),
              headers:{"Content-Type":"application/json"}
            })
              console.log("success")
             } catch (error) {
               console.log(error)
             }
          // .then((response)=>{if(response.status==200){
          //   this.showSuccess=true;
          // }})
          // .then((data) => {
            // console.log(data)
          // });
        
      });
    },
    }
}
</script>
<style scoped>
.productList {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 80%;
  margin: 0 auto;
  flex-wrap: wrap;
}
.product {
  display: flex;
  flex-direction: column;
  margin-bottom: 100px;
}
.productTop {
  width: 400px;
  height: 200px;
  display: flex;
  margin-bottom: 10px;
}
.image {
  width: 50%;
}
.productText {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 50%;

  justify-content: space-between;
  height: 100%;
}
.button {
  width: 100%;
  cursor: pointer;
  background-color: lightseagreen;
}
.checkoutButton {
  width: 80%;
  /* margin-left: 10%; */
  background-color: lightgreen;
  cursor: pointer;
}
.sortBy {
  margin: 0 auto;
  width: 80%;
  background-color: grey;
  margin-bottom: 20px;
  display: flex;
  justify-content: space-evenly;
  align-items: center;
  font-size: x-large;
  flex-wrap: wrap;
}
.sortBy select {
  width: 350px;
}
.sortBy label {
  width: 350px;
}
.productBought {
  display: flex;
  flex-direction: column;
  margin-top: 100px;
  justify-content: space-between;
}
</style>