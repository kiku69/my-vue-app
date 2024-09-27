<script setup>
import axios from 'axios';
import { onMounted, ref } from 'vue';

const products = ref([]);
const cart = ref([]);
const isLoading = ref(true);
const error = ref(null);


const fetchAllProducts = async () => {
  try {
    const response = await axios.get('https://fakestoreapi.com/products');
    products.value = response.data;
  } catch (err) {
    error.value = 'Failed to load products';
  } finally {
    isLoading.value = false;
  }
};


const addToCart = (product) => {
  cart.value.push(product);
};

onMounted(fetchAllProducts);
</script>

<template>
  <section class="container mx-auto py-8">
 
    <header class="flex justify-between items-center mb-8">
      <h1 class="text-4xl font-bold">Welcome to Our Fake Store</h1>
      <div class="relative">
        
        <button class="relative text-gray-600 hover:text-gray-800 focus:outline-none">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 3h2l.4 2M7 13h10l4-8H5.4M7 13L5 6H3M7 13l-2 9h12l-2-9M5 6h16"></path>
          </svg>
         
          <span v-if="cart.length" class="absolute top-0 right-0 rounded-full bg-red-600 text-white text-xs w-5 h-5 flex items-center justify-center">{{ cart.length }}</span>
        </button>
      </div>
    </header>

    
    <div v-if="isLoading" class="text-center text-lg text-gray-500">Loading products...</div>

    
    <div v-else-if="error" class="text-center text-red-500 text-lg">{{ error }}</div>

    
    <div v-else class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-8">
      <article v-for="product in products" :key="product.id" class="bg-white rounded-lg shadow-md overflow-hidden hover:shadow-xl transition-shadow duration-300">
        <img class="w-full h-64 object-cover" :src="product.image" :alt="product.title">
        <div class="p-4">
          <h4 class="text-lg font-semibold text-gray-800 truncate">{{ product.title }}</h4>
          <p class="text-gray-600 mt-2">$ {{ product.price.toFixed(2) }}</p>

         
          <button @click="addToCart(product)" class="mt-4 w-full bg-blue-600 text-white py-2 rounded-lg hover:bg-blue-700 transition-colors duration-300">
            Buy
          </button>
        </div>
      </article>
    </div>
  </section>
</template>

