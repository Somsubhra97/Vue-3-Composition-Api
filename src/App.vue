<template>
  <div id="app">
    <div id="user-info">
      <button @click="toggleUserInfo">{{ showUserInfo ? 'Hide' : 'Show'}} User Details</button>
      <p v-if="showUserInfo">You're logged in!</p>
    </div>
    <ProductForm :createProduct="createProduct" />
    <Products :items="products" :remove="deleteProduct" />
  </div>
</template>

<script>
import { reactive, ref,provide } from '@vue/composition-api';

import ProductForm from './components/ProductForm.vue';
import Products from './components/Products.vue';
import { useToggle } from './cmp-functions/toggle';

export default {
  name: 'app',
  components: {
    ProductForm,
    Products
  },
  // mixins: [toggleMixin],
  setup() {
    const products = ref([]);
    //provide('data',products);

    const createProduct = (title, price) => {
      products.value.push( {
        id: Math.random(),
        title: title,
        price: price
      });
    };

    const deleteProduct = productId => {
      products.value = products.value.filter(p => p.id !== productId);
    };

    //let showUserInfo = reactive({
       //  show: false
    // });
    // const showUserInfo = ref(false);
    // const toggleUserInfo = () => {
       //   showUserInfo.value = !showUserInfo.value;
      // };
    

    const { show: showUserInfo, toggle: toggleUserInfo } = useToggle();

    return {
      products,
      showUserInfo,
      createProduct,
      deleteProduct,
      toggleUserInfo
    };
  }
  // data() {
  //   return {
  //     products: [],
  //     showUserInfo: false
  //   };
  // },
  // methods: {
  //   createProduct(title, price) {
  //     const newProduct = {
  //       id: Math.random(),
  //       title: title,
  //       price: price
  //     };

  //     this.products.push(newProduct);
  //   },
  //   deleteProduct(productId) {
  //     this.products = this.products.filter(p => p.id !== productId);
  //   },
  //   toggleUserInfo() {
  //     this.showUserInfo = !this.showUserInfo;
  //   }
  // }
};
</script>

<style>
* {
  box-sizing: border-box;
}

html {
  font-family: sans-serif;
}

body {
  margin: 0;
}

#user-info {
  margin: 2rem auto;
  width: 90%;
  max-width: 40rem;
}
</style>
