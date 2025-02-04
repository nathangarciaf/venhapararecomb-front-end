<script setup>
import { defineProps, defineEmits } from 'vue';

// Recebe do pai a página atual
const props = defineProps({
  currentPage: String
});

const emit = defineEmits(['navigate']);

// Envia para o pai que a próxima página é a página de pagamento(Função usada pelo botão de pagar)
function goToPayment() {
  emit('navigate', 'payment');
}

// Envia para o pai que a próxima página é a página de resumo (Função usada pelo botão de resumo)
function goToSummary() {
    emit('navigate', 'summary');
}

</script>

<template>
    <nav class="navbar navbar-expand-lg navbar-light mb-3 d-flex nav-style">
        <a class="navbar-brand nav-title fs-1 ms-5" href="">Recomb Store</a>
      
        <div class="collapse navbar-collapse justify-content-end me-5">
            <ul class="navbar-nav d-flex align-items-center">
                <!-- Botões que só aparecem nas páginas especificadas -->
                <button v-if="currentPage === 'home'" @click="goToPayment" class="btn btn-success">Pagar</button>
                <button v-if="currentPage === 'pixFinishPayment' || currentPage === 'creditCardFinishPayment' || currentPage === 'bankingBilletFinishPayment'" 
                    @click="goToSummary" 
                    class="btn btn-success">
                    Resumo da Compra
                </button>
                <span class="fs-5 ms-3" style="text-align: center;">Usuário</span>
            </ul>
        </div>
    </nav>
</template>

<style scoped>
    .nav-style{
        background-color: blue;
        color: white;
    }

    .nav-title{
        color: white;
        text-decoration: none;
    }
</style>