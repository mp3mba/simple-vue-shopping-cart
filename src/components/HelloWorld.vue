<template>
  <div class="container">
    <div class="navbar">
      <nav class="nav">
        <h1>product catalog</h1>
        <h3 @click="isCart = !isCart"><span><i class="fa-solid fa-cart-shopping"></i></span> cart({{totalItems}})</h3>
      </nav>
    </div>

    <div class="product-container">
      <h1>Product Catalog</h1>
      <div class="product-list">
        <div v-for="product in products" :key="product.id" class="product">
          <div class="image">
            <img :src="product.image" alt="Product Image" />
          </div>
          <h3>{{ product.name }}</h3>
          <p>{{ product.price }}</p>
          <button class="btn-add" @click="addToCart(product)">Add to Cart</button>
        </div>
      </div>
    </div>
    
   <div class="shipping-cart" v-show="isCart">
    <h1><span>cart <i class="fa-solid fa-cart-shopping"></i></span><span id="xmark" @click="showCart"><i class="fa-solid fa-xmark"></i></span></h1>
    <div class="cart">
      <table class="table">
        <thead>
          <tr>
            <th>Name</th>
            <th>Price</th>
            <th>Action</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in cartItems" :key="item.id">
            <td>{{item.name}}</td>
            <td>{{item.price}}</td>
            <td><span id="trash-can" @click="deleteItem(item.id)"><i class="fa-solid fa-trash-can"></i></span></td>
          </tr>
        </tbody>
      </table>

      <div class="cart-total">
        <h3>Total: {{ cartTotal }}</h3>
        <button @click="checkout">Checkout</button>
      </div>
    </div>
   </div>

   <div class="footer">
    <footer>
      MyFooter
    </footer>
   </div>

  </div>
</template>

<script>
export default {
  data() {
    return {
      products: [
        { id: 1, name: 'Janja Cup', price: '$10', image: 'https://plus.unsplash.com/premium_photo-1675431443185-9d40521c8d5c?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxzZWFyY2h8MXx8cHJvZHVjdHxlbnwwfHwwfHw%3D&auto=format&fit=crop&w=500&q=60' },
        { id: 2, name: 'Watches', price: '$20', image: 'https://images.unsplash.com/photo-1523275335684-37898b6baf30?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxzZWFyY2h8Mnx8cHJvZHVjdHxlbnwwfHwwfHw%3D&auto=format&fit=crop&w=500&q=60' },
        { id: 3, name: 'Sun Glasses', price: '$30', image: 'https://images.unsplash.com/photo-1572635196237-14b3f281503f?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxzZWFyY2h8NXx8cHJvZHVjdHxlbnwwfHwwfHw%3D&auto=format&fit=crop&w=500&q=60' },
        // Add more products as needed
      ],
      cartItems: [],
      isCart: false,
      totalItems: '',
    };
  },
  computed: {
    cartTotal() {
      return this.cartItems.reduce((total, item) => total + parseFloat(item.price.replace('$', '')), 0);
    },
  },
  methods: {
    addToCart(product) {
      this.cartItems.push(product);
      this.totalItems = this.cartItems.length;
      localStorage.setItem('items', JSON.stringify(this.cartItems));
      console.log(this.totalItems)
    },
    checkout() {
      // Implement checkout logic here
      alert(`Total: $${this.cartTotal}. Check in your Email inbox for details`);
    },
    showCart(){
      this.isCart = !this.isCart
    },
    deleteItem(id){
      this.cartItems = this.cartItems.filter((data) => data.id != id);
      localStorage.setItem('items', JSON.stringify(this.cartItems));
      console.log(id)
    },
  },
  mounted() {
    if (localStorage.getItem("items")) {
      this.cartItems = JSON.parse(localStorage.getItem("items"));

    }
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container{
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.navbar{
  width: 100%
}

.product-container{
  background-color: rgb(250, 242, 230);
  width: 100%;
  /* height: 300px; */
}

.product-list {
  display: flex;
  justify-content: space-between;
  gap: 10px;
  padding: 40px 0 160px;
  padding: 0 30px 90px;
  /* flex-wrap: wrap; */
}
.nav{
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: rgb(250, 186, 89);
  width: 100%;
  height: 50px;
}

.nav h1, .nav h3{
  padding: 0 30px;
  cursor: pointer;
}

.product {
  width: 300px;
  padding: 5px 5px 15px;
  background-color: rgb(250, 235, 217);
  box-shadow: 0 0 2px rgba(0,0,0,.4);
}

.image{
  width: 100%;
  height: 200px;
}

img{
  width: 100%;
  height: 100%;
}

.shipping-cart{
  position: fixed;
  top: 0;
  right: 0;
  background-color: rgba(0,0,0,.8);
  color: white;
  width: 30%;
  height: 100%;
}

.shipping-cart h1{
  margin-top: 0;
  background-color: gray;
  height: 50px;
  border-bottom: 1px solid white;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 14px;
  font-size: 18px;
}

#xmark{
  cursor: pointer;
  font-size: 20px;
}
.btn-add{
  background-color: darkgreen;
  padding: 7px;
  border: none;
  border-radius: 4px;
  color: white;
  font-weight: 900;
  cursor: pointer;
}

.table {
  width: 100%;
  padding: 0 14px;
  border-collapse: collapse;
}

.cart-total{
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.cart-total h3, .cart-total button{
  margin: 15px 14px;
}
.cart-total button{
  border: none;
  background-color:rgb(250, 186, 89);
  padding: 5px;
  font-weight: 900;
  cursor: pointer;
}
#trash-can{
  color: rgb(187, 187, 187);
  cursor: pointer;
  transition: color .5s;
}

#trash-can:hover{
  color: #fff;
}

.footer{
  width: 100%;
  padding: 50px;
  background-color: rgb(250, 186, 89);
  display: flex;
  justify-content: center;
  align-items: center;
}

@media (max-width: 768px){
  .product-list{
    flex-direction: column;
    align-items: center;
  }
  .product{
    width: 100%;
  }
}

</style>
