<script setup>
import { ref } from 'vue';
import Home from './components/Home.vue';
import Navbar from './components/Navbar.vue';
import PaymentSelect from './components/PaymentSelect.vue';
import BankingBilletInfo from './components/BankingBilletInfo.vue';
import BankingBilletFinishPayment from './components/BankingBilletFinishPayment.vue';
import CreditCardFinishPayment from './components/CreditCardFinishPayment.vue';
import CreditCardInfo from './components/CreditCardInfo.vue';
import PixFinishPayment from './components/PixFinishPayment.vue';
import Summary from './components/Summary.vue';

const currentPage = ref('home');
const paymentSelected = ref('');
const purchasedItems = ref([]);
const hasSelectedItems = ref(false);
const errorMessage = ref('');

// Responsável pelo transição entre as páginas
function navigateTo(page) {
  if (page === 'payment' && !hasSelectedItems.value) {
    errorMessage.value = 'Selecione pelo menos um item antes de prosseguir!';
    return;
  }
  
  errorMessage.value = '';
  currentPage.value = page;
}

// Armazena os itens selecionados que foram levados para a fase de pagamento
function getPurchasedItems(items){
  purchasedItems.value = items;
}

// Atualiza o sistema caso haja ou não itens selecionados
function updateHasSelectedItems(hasItems) {
  errorMessage.value = '';
  hasSelectedItems.value = hasItems;
}


// Armazena o método de pagamento selecionado
function updateSelectedPayment(payment){
  paymentSelected.value = payment;
}

// Pega o identificador do método de pagamento retornado pelo componente filho e 
// modulariza para o formato utilizado pelo sistema
function getPaymentPage(payment) {
  updateSelectedPayment(payment);
  const paymentPage = payment + "Info";
  if(payment === "pix"){
    const paymentPage = payment + "FinishPayment";
    navigateTo(paymentPage);
    return;
  }
  navigateTo(paymentPage);
}

</script>

<template>
  <div>
    <Navbar 
      :currentPage="currentPage" 
      @navigate="navigateTo" 
    />
    <div v-if="errorMessage" class="alert alert-danger text-center">
      {{ errorMessage }}
    </div>
    
    <Home 
      v-if="currentPage === 'home'" 
      @hasSelectedItems="updateHasSelectedItems" 
      @selectedItems="getPurchasedItems"
    />

    <PaymentSelect 
      v-if="currentPage === 'payment'" 
      @paymentSelected="getPaymentPage"
      @back="navigateTo"
    />

    <PixFinishPayment 
      v-if="currentPage === 'pixFinishPayment'"
    />

    <BankingBilletInfo 
      v-if="currentPage === 'bankingBilletInfo'"
      @bankingBilletInfoCompleted="navigateTo"
      @back="navigateTo"
    />

    <BankingBilletFinishPayment
      v-if="currentPage === 'bankingBilletFinishPayment'"  
    />

    <CreditCardInfo
      v-if="currentPage === 'creditCardInfo'"
      @creditCardInfoCompleted="navigateTo"
      @back="navigateTo"
    />

    <CreditCardFinishPayment
      v-if="currentPage === 'creditCardFinishPayment'"
    />

    <Summary 
      v-if="currentPage === 'summary'"
      :items="purchasedItems"
      :paymentMethod="paymentSelected"
    />
  </div>
</template>
