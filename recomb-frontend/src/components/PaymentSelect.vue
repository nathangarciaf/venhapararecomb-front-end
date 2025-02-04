<script setup>
import { ref, defineEmits } from 'vue';

const selectedPayment = ref('');
const emit = defineEmits(['paymentSelected', 'back']);

function proceedToPayment() {
  if (!selectedPayment.value) {
    alert('Selecione um método de pagamento antes de continuar!');
    return;
  }
  emit('paymentSelected', selectedPayment.value);
}

function backToHome(){
    emit('back', 'home');
}
</script>

<template>
    <a class="ms-5 fs-5 backlink" @click="backToHome">Voltar</a>
    <div class="d-flex justify-content-center align-items-center" style="height: 75vh;">
        <div class="card w-35">
            <h3 class="card-header">Selecione a forma de pagamento</h3>
            <div class="card-body">
                <div class="form-check mb-2 align-items-center d-flex">
                    <input class="form-check-input" type="radio" id="cartao" value="creditCard" v-model="selectedPayment"/>
                    <label class="form-check-label fs-5 ms-2" for="cartao">Cartão de Crédito</label>
                </div>
                <div class="form-check mb-2 align-items-center d-flex">
                    <input class="form-check-input" type="radio" id="pix" value="pix" v-model="selectedPayment"/>
                    <label class="form-check-label fs-5 ms-2" for="pix">Pix</label>
                    </div>
                <div class="form-check mb-2 align-items-center d-flex">
                    <input class="form-check-input" type="radio" id="boleto" value="bankingBillet" v-model="selectedPayment"/>
                    <label class="form-check-label fs-5 ms-2" for="boleto">Boleto</label>
                </div>
                <button class="btn btn-primary mt-3" @click="proceedToPayment">Confirmar</button>
            </div>
        </div>
    </div>
</template>

<style scoped>
    .backlink {
        text-decoration: none;
    }

    .backlink:hover {
        cursor: pointer;
    }

</style>