<script setup>
import { defineProps, defineEmits } from 'vue';

// Recebe de seu componente pai os itens selecionados pelo cliente 
// e também recebe o método de pagamento ainda não formatado 
const props = defineProps({
  paymentMethod: String,
  items: Array
});

// Pega a data atual
const currentDate = new Date();

// Calcula uma data para se referir ao prazo de entrega
const futureDate = new Date();
futureDate.setDate(currentDate.getDate() + 7);

// Responsável por formatar a string baseado no identificador do método de pagamento
function formatPaymentMethod(payment){
    if(payment === 'pix'){
        return "Pix";
    }
    if(payment === 'bankingBillet'){
        return "Boleto";
    }
    if(payment === 'creditCard'){
        return "Cartão de Crédito";
    }
    return payment;
}

</script>

<template>
    <div class="container">
        <h1 class="mb-4">Resumo da Compra</h1>
        <div class="d-flex flex-wrap">
            <div class="me-4" v-for="item in items" :key="item.id">
                <div class="card mb-2 fs-4" style="width: 20rem;">
                    <h3 class="card-header">{{ item.name }}</h3>
                    <img class="card-img-top mx-auto  w-75" :src="item.imageRef" alt="Imagem">
                    <div class="card-body">
                        <p class="card-text">{{ item.description }}</p>
                    </div>
                </div>
            </div>
        </div>
        
        <p class="mb-4"><strong class="fs-5">Método de Pagamento:</strong class="fs-5">  {{ formatPaymentMethod(paymentMethod) }}</p>
        <p class="mb-4"><strong class="fs-5">Data da Compra:</strong class="fs-5"> {{ currentDate.toLocaleDateString() }}</p>
        <p class="mb-4"><strong class="fs-5">Previsão de Entrega:</strong class="fs-5"> {{ futureDate.toLocaleDateString() }}</p>
    </div>
</template>