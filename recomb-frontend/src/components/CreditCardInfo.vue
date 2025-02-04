<script setup>
import { ref, defineEmits } from 'vue'

const emit = defineEmits(['creditCardInfoCompleted', 'back']);

// Campos necessários
const creditCardBrand = ref('');
const cpfOrCnpj = ref('');
const name = ref('');
const securityCode = ref('');
const cardNumber = ref('');
const installments = ref(null);

const dueDate = ref({
  month: new Date().getMonth(),
  year: new Date().getFullYear()
});

// Valida os dados preenchidos pelo usuário e caso não haja erros,
// emite ao pai que pode proceder para a finalização do pagamento daquele
// método selecionado.
function submitCreditCardInfo(){
    let errors = [];

    validateName(name, errors);
    validateCPFOrCNPJ(cpfOrCnpj, errors);
    validateCVV(securityCode, errors);
    validateCreditCardBrand(creditCardBrand, errors);
    validateDueDate(dueDate, errors);
    validateInstallments(installments, errors);
    
    if (errors.length > 0) {
        alert(errors.join(', '));
        return;
    }

    emit('creditCardInfoCompleted', 'creditCardFinishPayment');
}

// Retorna para a seleção de método de pagamento caso o usuário deseje.
function backToSelectPayment() {
    emit('back', 'payment');
}

// Todas as funções abaixo são responsáveis pela validação superficial dos dados
// preenchidos pelo usuário
function validateName(name, errors) {
    if (!name.value) {
        errors.push("É obrigatório inserir o nome!");
    }
}

function validateCPFOrCNPJ(cpf, errors) {
    if (!cpf.value) {
        errors.push("CPF é obrigatório!");
        return;
    }
    if (cpf.value.length !== 11 && cpf.value.length !== 14) {
        errors.push("CPF/CNPJ de tamanho incorreto!");
    }
}

function validateCVV(cvv, errors) {
    if (!cvv.value) {
        errors.push("CVV é obrigatório!");
        return;
    }
    if (!/^[0-9]+$/.test(cvv.value)) {
        errors.push("CVV deve conter apenas números!");
    }
    if (cvv.value.length !== 3) {
        errors.push("CVV possui tamanho incorreto!");
    }
}

function validateCardNumber(cardNumber, errors) {
    if (!cardNumber.value) {
        errors.push("Número do cartão é obrigatório!");
    }
}

function validateCreditCardBrand(brand, errors) {
    if (!brand.value) {
        errors.push("Bandeira do cartão é obrigatória!");
    }
}

function validateDueDate(dueDate, errors) {
    if (!dueDate.value || !dueDate.value.month || !dueDate.value.year) {
        errors.push("Data de vencimento inválida!");
    }
}

function validateInstallments(installments, errors) {
    if (!installments.value) {
        errors.push("É obrigatório selecionar o número de parcelas!");
    }
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

                <div class="mb-2 align-items-center">
                    <label class="fw-bold me-3 fs-5" for="installments">Número de Parcelas:</label>
                    <select class="form-control" id="installments" v-model="installments">
                        <option value="" disabled>Selecione</option>
                        <option v-for="i in 12" :key="i" :value="i">{{ i }}x</option>
                    </select>
                </div>

                <div class="mt-3">
                    <button class="btn btn-primary" @click="submitCreditCardInfo">Confirmar</button>
                </div>
            </div>
        </div>
    </div>
</template>
