<script setup>
import { ref } from 'vue';
import Home from './components/Home.vue';
import Navbar from './components/Navbar.vue';
import PaymentSelect from './components/PaymentSelect.vue';
import BankingBilletInfo from './components/BankingBilletInfo.vue';
import CreditCardInfo from './components/CreditCardInfo.vue';
import PixFinishPayment from './components/PixFinishPayment.vue';
import Summary from './components/Summary.vue';

const currentPage = ref('home');
const paymentSelected = ref('');
const purchasedItems = ref([]);
const hasSelectedItems = ref(false);
const errorMessage = ref('');

function navigateTo(page) {
  if (page === 'payment' && !hasSelectedItems.value) {
    errorMessage.value = 'Selecione pelo menos um item antes de prosseguir!';
    return;
  }
  
  errorMessage.value = '';
  currentPage.value = page;
}

function getPurchasedItems(items){
  purchasedItems.value = items;
}

function updateSelectedItems(hasItems) {
  errorMessage.value = '';
  hasSelectedItems.value = hasItems;
}

function updateSelectedPayment(payment){
  paymentSelected.value = payment;
}

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
    <Navbar :currentPage="currentPage" @navigate="navigateTo" />
    <div v-if="errorMessage" class="alert alert-danger text-center">
      {{ errorMessage }}
    </div>
    
    <Home v-if="currentPage === 'home'" @hasSelectedItems="updateSelectedItems" @selectedItems="getPurchasedItems"/>

    <PaymentSelect v-if="currentPage === 'payment'" @paymentSelected="getPaymentPage"/>

    <PixFinishPayment v-if="currentPage === 'pixFinishPayment'"/>

    <Summary 
    v-if="currentPage === 'summary'"
    :items="purchasedItems"
    :paymentMethod="paymentSelected"
    />
  </div>
</template>
