<script setup>
import { ref, computed, onBeforeUpdate  } from 'vue';
const name = 'vue dinámico';
const styleColor = "color: blue";
const arrayColores = ["blue", "red", "peru"]
const activo = true;

const arrayFrutas = ["🍎", "🍌", "🍉", "🍓", "🍒"];
const arrayFrutas2 = [
    {
        name: "Manzana",
        price: "$1.00",
        description: "Una manzana",
    },
    {
        name: "Pera",
        price: "$2.00",
        description: "Una pera",
    },
    {
        name: "Naranja",
        price: "$3.00",
        description: "Una naranja",
    },
];
const objetoFruta =  {
    name: "Manzana",
    price: "$1.00",
    description: "Una manzana",
}

const arrayFrutas3 = [
    {
        name: "Manzana",
        price: "$1.00",
        description: "Una manzana",
        stock: 0,
    },
    {
        name: "Pera",
        price: "$2.00",
        description: "Una pera",
        stock: 10,
    },
    {
        name: "Naranja",
        price: "$3.00",
        description: "Una naranja",
        stock: 20,
    },
];

//metodo - methods
const handleClick = (message) => {
  console.log(message);
}

const counter = ref(0);
const colorCounter = ref('green');
const increment = () => {
  counter.value++;
}
const decrement = () => {
  counter.value--;
}
const classCounter = computed(() => {
  if (counter.value === 0) {
    return "zero";
  }
  return counter.value > 0 ? "positive" : "negative";
});


const favoritesArray = ref([]);
const addToArray = () =>{
  favoritesArray.value.push(counter.value);
}
const enabledButton = computed(() => {
  for(let item of favoritesArray.value){
    if(favoritesArray.value.includes(counter.value)){
      return true;
    }
  }
  return false;
});

</script>

<template>
  <h1>{{ name.toUpperCase() }}</h1>
  <h2> {{arrayColores}}</h2>
  <h2 :style="`color: ${arrayColores[2]}`">Soy Perú</h2>
  <h2 v-if="activo === true">Estoy activo</h2>
  <p v-else-if="activo === false">Estoy inactivo</p>
  <p v-else>Estoy indeciso</p>
  <h2 v-show="activo">Estoy activo v-show</h2> <!--Usar para elementos que tienen alta alternancia en ejecución-->

  <ul>
    <li v-for="(fruta, index) in arrayFrutas" :key="index">
      {{ index }} - {{ fruta }}
    </li>
  </ul>
  <ul>
    <li v-for="fruta in arrayFrutas2" :key="fruta.name">
      {{ `${fruta.name} - ${fruta.price} - ${fruta.description}` }}
    </li>
  </ul>
  <ul>
    <li v-for="(value, propiedad, index) in objetoFruta" :key="index">
      {{ index }} - {{ propiedad }} : {{ value }}
    </li>
  </ul>

  <!-- <ul> -->
    <!-- <li v-for="item in arrayFrutas3" :key="item.name" v-if="item.stock > 0" > Este código se rompe porque v-if intenta entrar primero que v-for -->
      <!-- {{ item.name }} - {{ item.price }} -->
    <!-- </li> -->
  <!-- </ul> -->
   <ul> <!--Forma correcta de hacerlo-->
      <template v-for="item in arrayFrutas3" :key="item.name">
        <li v-if="item.stock >= 10">
          {{ item.name }}
        </li>
      </template>
   </ul>

   <button v-on:click.right.prevent="handleClick('Actívame right')">Actívame right</button>
   <button @click.middle="handleClick('Actívame middle')">Actívame middle</button>
   <button @click.left="handleClick('Actívame left')">Actívame left</button>
   <br><br><br>
   <div class="container">
    <h2>Practica 1</h2>
    <h1>Practica 2</h1>
   </div>
   <br><br><br>
   <div class="container text-center">
     <h2 :class="classCounter">{{ counter }}</h2>
     <div class="btn-group">
       <button @click="increment()" class="btn btn-success">Aumentar contador</button>
       <button @click="decrement()" class="btn btn-danger">Disminuir contador</button>
       <button @click="addToArray()" :disabled="enabledButton" class="btn btn-primary">Agregar al array de favoritos</button>
     </div>
     <div>
        <ul class="list-group">
          <template v-for="item in favoritesArray">
            <li class="list-group-item">
              {{ item }}
            </li>
          </template>
        </ul>
     </div>
   </div>
   <footer>
    <br><br><br><br><br><br><br><br><br><br>
   </footer>
</template>

<style>
h1 {
  color: red;
}

.positive {
  color: green;
}

.negative {
  color: red;
}

.zero {
  color: peru;
}
</style>