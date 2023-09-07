<script setup>
import { customerStore } from "../store";
const props = defineProps({
  name: String,
  cost: Number,
  expiry: String,
  size: String,
  id: Number,
  imgid:Number,
  units: Number,
})

function outofstock(){
  if(props.units<1) return true
  else
    return false
}

function add() {
  if (props.id in customerStore.cart)
    {
      if(props.units>customerStore.cart[props.id])
        customerStore.cart[props.id]++
    }
  else
    customerStore.cart[props.id] = 1
  customerStore.total+=props.cost
}

function remove() {
  if (customerStore.cart[props.id]>1)
    customerStore.cart[props.id]--
  else
    delete customerStore.cart[props.id]
  customerStore.total-=props.cost
}

function getImg(){
  return `http://localhost:5000/image/${props.imgid}`
}

</script>
<template>
  <div class="card me-3 mb-3 p-0 border" style="width: 12rem; height: 18rem;">
    <span class="position-absolute m-2 badge rounded-pill bg-primary">
      {{ props.expiry }}
    </span>
    <img :src="getImg()" style="height:11rem;object-fit: cover;" class="card-img-top" alt="..." />
    <div class="card-body p-2">
      <h6 class="card-title p-0 m-0 ms-1">{{ props.name }}</h6>
      <p class="card-text p-0 m-1">
      <div class="d-flex">
        <div class="flex-fill">
          <div class="text-secondary">
            {{ props.size }}
          </div>
          &#8377;{{ props.cost }}
        </div>
        <div>
          <div v-if="outofstock()">
            Out of stock
          </div>
          <div v-else-if="id in customerStore.cart && customerStore.cart[id]" class="btn-group" role="group" aria-label="Basic outlined example">
            <button @click="remove" type="button" class="btn btn-outline-primary">-</button>
            <button type="button" class="btn btn-outline-primary">{{ id in customerStore.cart ? customerStore.cart[id] : 0 }}</button>
            <button @click="add" type="button" class="btn btn-outline-primary">+</button>
          </div>
          <button v-else @click="add" class="btn btn-success">
            Add
          </button>
        </div>
      </div>
      </p>
    </div>
  </div>
</template>
