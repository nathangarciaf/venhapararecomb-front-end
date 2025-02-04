<script setup>
import { ref, defineEmits } from 'vue'

const emit = defineEmits(['creditCardInfoCompleted', 'back']);

const creditCardBrand = ref('');
const creditCardBrandError = ref('');

const cpfOrCnpj = ref('');
const cpfOrCnpjError = ref('');

const name = ref('');
const nameError = ref('');

const securityCode = ref('');
const securityCodeError = ref('');

const cardNumber = ref('');
const cardNumberError = ref('');

const dueDate = ref({
  month: new Date().getMonth(),
  year: new Date().getFullYear()
});

function submitCreditCardInfo(){
    validateName(name, nameError);
    validateCPFOrCNPJ(cpfOrCnpj, cpfOrCnpjError);
    validateCVV(securityCode, securityCodeError);
    validateCreditCardBrand(creditCardBrand,creditCardBrandError);

    if(!nameError.value && !cpfOrCnpjError.value && !securityCodeError.value && !creditCardBrandError.value){
        emit('creditCardInfoCompleted', 'creditCardFinishPayment');
    }
}

function validateName(name,errorMsg){
    if(!name.value){
        errorMsg.value = "É obrigatório inserir o nome!";
        return;
    }
    errorMsg.value = '';
}

function validateCPFOrCNPJ(cpf, errorMsg){
    if(!cpf.value){
        errorMsg.value = "CPF é obrigatório!"
        return;
    }
    errorMsg.value = '';
}

function validateCVV(cvv, errorMsg){
    if(!cvv.value){
        errorMsg.value = "CVV é obrigatório!";
        return; 
    }
    if(cvv.value.length != 3){
        errorMsg.value = "CVV possui tamanho incorreto!";
        return;
    }

    errorMsg.value = '';
}

function validateCreditCardBrand(brand, errorMsg){
    if(!brand.value){
        errorMsg.value = "Bandeira do cartão é obrigatória!"
        return; 
    }
    errorMsg.value = '';
}

function backToSelectPayment(){
    emit('back', 'payment');
}

</script>

<template>
    <a class="ms-5 fs-5 backlink" @click="backToSelectPayment">Voltar</a>
    <div class="d-flex justify-content-center align-items-center" style="height: 75vh;">
        <div class="card">
            <h3 class="card-header">Pagamento por cartão de crédito</h3>
            <div class="card-body">
                <div class="mb-2 align-items-center">
                    <label class="fw-bold fs-5 me-3" for="cardNumber">Selecione a bandeira:</label>
                    <div class="d-flex align-items-center">
                        <div class="mb-2 align-items-center d-flex me-3">
                            <input class="form-check-input" type="radio" id="visa" value="visa" v-model="creditCardBrand"/>
                            <label class="ms-2" for="visa">Visa</label>
                        </div>
                        <div class="mb-2 align-items-center d-flex me-3">
                            <input class="form-check-input" type="radio" id="elo" value="elo" v-model="creditCardBrand"/>
                            <label class="ms-2" for="elo">Elo</label>
                        </div>
                        <div class="mb-2 align-items-center d-flex me-3">
                            <input class="form-check-input" type="radio" id="mastercard" value="mastercard" v-model="creditCardBrand"/>
                            <label class="ms-2" for="mastercard">Mastercard</label>
                        </div>
                        <div class="mb-2 align-items-center d-flex ">
                            <input class="form-check-input" type="radio" id="amex" value="amex" v-model="creditCardBrand"/>
                            <label class="ms-2" for="amex">American Express</label>
                        </div>
                    </div>
                    
                </div>

                <div class="mb-2 align-items-center">
                    <label class="fw-bold me-3 fs-5" for="cardNumber">Número do Cartão:</label>
                    <input class="form-control" type="text" id="cardNumber" v-model="cardNumber"/>
                </div>

                <div class="mb-2 align-items-center">
                    <label class="fw-bold me-3 fs-5" for="name">Nome do titular:</label>
                    <input class="form-control" type="text" id="name" v-model="name"/>
                </div>

                <div class="mb-2 align-items-center ">
                    <label class="fw-bold me-3 fs-5" for="cpfOrCnpj">CPF/CNPJ:</label>
                    <input class="form-control" type="text" id="cpfOrCnpj" v-model="cpfOrCnpj"/>
                </div>

                <div class="d-flex">
                    <div class="mb-2 align-items-center w-50">
                        <label class="fw-bold me-3 fs-5" for="dueDate">Data de Vencimento:</label>
                        <VueDatePicker v-model="dueDate" month-picker />
                    </div>

                    <div class="mb-2 align-items-center ms-1">
                        <label class="fw-bold me-3 fs-5" for="securityCode">CVV:</label>
                        <input class="form-control" type="text" id="securityCode" v-model="securityCode"/>
                    </div>
                </div>

                <div class="mt-3">
                    <button class="btn btn-primary" @click="submitCreditCardInfo">Confirmar</button>
                </div>
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