<script setup>
import { ref, defineEmits } from 'vue';

const emit = defineEmits(['bankingBilletInfoCompleted', 'back']);

// Campos necessários
const cpf = ref('');
const name = ref('');
const lastName = ref('');
const email = ref('');

// Valida os dados preenchidos pelo usuário e caso não haja erros,
// emite ao pai que pode proceder para a finalização do pagamento daquele
// método selecionado.
function submitBankingBilletInfo() {
    let errors = [];
    
    validateName(name, errors);
    validateName(lastName, errors);
    validateCPF(cpf, errors);
    validateEmail(email, errors);
    
    if (errors.length > 0) {
        alert(errors.join(', '));
        errors = [];
        return;
    }
    
    emit('bankingBilletInfoCompleted', 'bankingBilletFinishPayment');
}

// Retorna para a seleção de método de pagamento caso o usuário deseje.
function backToSelectPayment() {
    emit('back', 'payment');
}

// Todas as funções abaixo são responsáveis pela validação superficial dos dados
// preenchidos pelo usuário
function validateName(name, errors) {
    if (!name.value) {
        errors.push("É obrigatório inserir nome e sobrenome!");
    }
}

function validateCPF(cpf, errors) {
    if (!cpf.value) {
        errors.push("CPF é obrigatório!");
        return;
    }
    if (cpf.value.length !== 11 && cpf.value.length !== 14) {
        errors.push("CPF/CNPJ de tamanho incorreto!");
    }
}

function validateEmail(email, errors) {
    if (!email.value) {
        errors.push("Email é obrigatório!");
    }
}
</script>


<template>
    <a class="ms-5 fs-5 backlink" @click="backToSelectPayment">Voltar</a>
    <div class="d-flex justify-content-center align-items-center" style="height: 75vh;">
        <div class="card">
            <h3 class="card-header">Pagamento por boleto</h3>
            <div class="card-body">
                <div class="mb-2 align-items-center">
                    <label class="fw-bold me-3 fs-5" for="name">Nome:</label>
                    <input class="form-control" type="text" id="name" v-model="name"/>
                </div>

                <div class="mb-2 align-items-center">
                    <label class="fw-bold me-3 fs-5" for="lastName">Sobrenome:</label>
                    <input class="form-control" type="text" id="lastName" v-model="lastName"/>
                </div>
                <div class="mb-2 align-items-center">
                    <label class="fw-bold me-3 fs-5" for="cpf">CPF:</label>
                    <input class="form-control" type="text" id="cpf" v-model="cpf"/>
                </div>
                <div class="mb-2 align-items-center">
                    <label class="fw-bold me-3 fs-5" for="email">Email:</label>
                    <input class="form-control" type="text" id="email" v-model="email"/>
                </div>

                <div class="mt-3">
                    <button class="btn btn-primary" @click="submitBankingBilletInfo">Confirmar</button>
                </div>
            </div>
        </div>
    </div>
</template>