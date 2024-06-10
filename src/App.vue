<script>
import Products from "./components/Products.vue";
import Cart from "./components/Cart.vue";

export default {
  data() {
    return {
      pageView: 'products',
      products: [
        {
          id: 1,
          name: "Apple Watch Series 7 GPS, Aluminium Case, Starlight Sport",
          image: "/product-images/apple-watch-1.jpg",
          rating: 5.0,
          price: 599,
          isCart: false,
        },
        {
          id: 2,
          name: "Apple Watch Series 6 GPS, Aluminium Case, Red Sport",
          image: "/product-images/apple-watch-2.jpg",
          rating: 4.8,
          price: 499,
          isCart: false,
        },
        {
          id: 3,
          name: "Apple Watch SE GPS, Aluminium Case, Blue Sport",
          image: "/product-images/apple-watch-3.jpg",
          rating: 4.7,
          price: 399,
          isCart: true,
        },
        {
          id: 4,
          name: "Apple Watch Series 5 GPS, Aluminium Case, Black Sport",
          image: "/product-images/apple-watch-4.jpg",
          rating: 4.6,
          price: 349,
          isCart: false,
        },
        {
          id: 5,
          name: "Apple Watch Series 4 GPS, Aluminium Case, White Sport",
          image: "/product-images/apple-watch-5.jpg",
          rating: 4.5,
          price: 299,
          isCart: false,
        },
        {
          id: 6,
          name: "Apple Watch Series 3 GPS, Aluminium Case, Space Gray Sport",
          image: "/product-images/apple-watch-6.jpg",
          rating: 4.4,
          price: 199,
          isCart: true,
        },
      ],

      carts: [
         {
          id: 1,
          name: "Apple Watch Series 7 GPS, Aluminium Case, Starlight Sport",
          image: "/product-images/apple-watch-1.jpg",
          rating: 5.0,
          price: 599,
          isCart: false,
        },
        {
          id: 2,
          name: "Apple Watch Series 6 GPS, Aluminium Case, Red Sport",
          image: "/product-images/apple-watch-2.jpg",
          rating: 4.8,
          price: 499,
          isCart: false,
        },
      ]
    };
  },

  components: {
    Products,
    Cart
  },

methods: {
  changePageView() {
    this.pageView = 'cart';
  },
}

};

</script>

<template>
  <div class="container mx-auto mt-6"  v-if="pageView === 'products'">
    <h1 className="text-3xl font-bold underline text-center my-3">Watch Store</h1>
    <div class="flex justify-between flex-wrap">
      <template  v-for="(product, index) in products">
      <Products :product="product" @show-cart="changePageView"/>
      </template>
    </div>
  </div>

     <div class="container mx-auto mt-6"  v-else-if="pageView === 'cart'">
     <div class="flex justify-between items-center">
        <h1 className="text-3xl font-bold underline text-center my-3"></h1>
        <h1 className="text-3xl font-bold underline text-center my-3">Cart</h1>
        <button class="text-white bg-blue-700 hover:bg-blue-800 font-medium rounded text-sm px-3 py-1 text-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800" @click="pageView = 'products'">Back</button>
     </div>
    <Cart :carts="carts" @go-to-product="pageView = 'products'"/>
    
  </div>

</template>
