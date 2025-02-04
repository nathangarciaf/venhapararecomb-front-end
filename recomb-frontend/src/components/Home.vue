<script setup>
import { ref, watchEffect, defineEmits } from 'vue'

import microondas from '@/assets/microndas.avif'
import geladeira from '@/assets/geladeira.webp'
import cafeteira from '@/assets/cafeteira.jpg'
import fogao from '@/assets/fogao.webp'

let id = 0

const items = ref([
  { id: id++, name: 'Geladeira Electrolux', description: 'Frost Free Inverter 431L AutoSense Duplex Cor Black Inox Look (IF55B)', selected: false, imageRef: geladeira},
  { id: id++, name: 'Micro-ondas LG', description: '30 litros Prata Limpa Fácil (MS3043BR)', selected: false, imageRef: microondas },
  { id: id++, name: 'Cafeteira Oster', description: 'Espresso PrimaLatte Touch Red, 110V, BVSTEM6801R', selected: false, imageRef: cafeteira},
  { id: id++, name: 'Fogão Eletrolux', description: '5 bocas Cinza Efficient com Mesa Inox, PerfectCook e VaporBake (FE5IC) - Bivolt', selected: false, imageRef: fogao}
])

const selectedItems = ref([])

const emit = defineEmits(['hasSelectedItems', 'selectedItems']);

function selectItems(item) {
  item.selected = !item.selected
  if (item.selected) {
    selectedItems.value.push(item)
  } 
  else {
    selectedItems.value = selectedItems.value.filter(selectedItem => selectedItem.id !== item.id)
  }
  emit('hasSelectedItems', selectedItems.value.length > 0);
  emit('selectedItems', selectedItems.value);
}

watchEffect(() => {
  emit('hasSelectedItems', selectedItems.value.length > 0);
  emit('selectedItems', selectedItems.value);
});

</script>

<template>
  <div class="d-flex flex-wrap">
    <div class="ms-5" v-for="item in items" :key="item.id">
        <div class="card card-size mb-2 fs-4">
          <img class="card-img-top" :src="item.imageRef" alt="Card image cap">
          <div class="card-body">
            <h5 class="card-title">{{ item.name }}</h5>
            <p class="card-text">{{ item.description }}</p>
            <div class="d-flex align-items-center">
              <input type="checkbox" :checked="item.selected" @click="selectItems(item)" />
              <span v-if="!item.selected" class="ms-2 fw-bold fs-5">Adicionar ao carrinho</span>
              <span v-else class="ms-2 fw-bold fs-5">No carrinho</span>
            </div>
          </div>
        </div>
    </div>
  </div>
</template>

<style scoped>
    .card-size{
        width: 25rem;
    }
</style>