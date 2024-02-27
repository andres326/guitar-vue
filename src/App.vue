<script setup>
import { ref, onMounted } from 'vue'
import { db } from './data/guitars'
import Guitar from './components/Guitar.vue';
import Header from './components/Header.vue'
import Footer from './components/Footer.vue'

const guitars = ref([])
const cart = ref([])
const guitarHeader = ref({})

onMounted(() => {
  guitars.value = db
  guitarHeader.value = db[4]
})

const addToCart = (guitar) => {
  const isInCart = cart.value.findIndex(item => item.id === guitar.id)
  if (isInCart >= 0) {
    cart.value[isInCart].quantity++
  } else {
    guitar.quantity = 1
    cart.value.push(guitar)
  }
}

const decreaseQuantity = (id) => {
  const index = cart.value.findIndex(item => item.id === id)
  if (cart.value[index].quantity <= 1) return
  cart.value[index].quantity--
}

const increaseQuantity = (id) => {
  const index = cart.value.findIndex(item => item.id === id)
  if (cart.value[index].quantity >= 5) return
  cart.value[index].quantity++
}

const deleteItem = (id) => {
  cart.value = cart.value.filter(item => item.id !== id)
}

const deleteAllItems = () => {
  cart.value = []
}

</script>

<template>
  <Header :cart="cart" :guitar="guitarHeader" @add-to-cart="addToCart" @increase-quantity="increaseQuantity"
    @decrease-quantity="decreaseQuantity" @delete-item="deleteItem" @delete-all="deleteAllItems" />
  <main class="container-xl mt-5">
    <h2 class="text-center">Our Collection</h2>

    <div class="row mt-5">
      <Guitar v-for="guitar in guitars" :guitar="guitar" @add-cart="addToCart" />
    </div>
  </main>

  <Footer />
</template>

<style scoped></style>
