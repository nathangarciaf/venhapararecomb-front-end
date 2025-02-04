<script setup>
import { ref, defineEmits } from 'vue'

const emit = defineEmits(['bankingBilletInfoCompleted', 'back']);

const cpf = ref('');
const cpfError = ref('');

const name = ref('');
const nameError = ref('');

const lastName = ref('');
const lastNameError = ref('');

const email = ref('');
const emailError = ref('');

function submitBankingBilletInfo(){
    validateName(name, nameError);
    validateName(lastName, lastNameError);
    validateCPF(cpf, cpfError);
    validateEmail(email, emailError);

    if(!nameError.value && !lastNameError.value && !cpfError.value && !emailError.value){
        emit('bankingBilletInfoCompleted', 'bankingBilletFinishPayment');
    }
}

function validateName(name,errorMsg){
    if(!name.value){
        errorMsg.value = "É obrigatório inserir nome e sobrenome!";
        return;
    }
    errorMsg.value = '';
}

function validateCPF(cpf, errorMsg){
    if(!cpf.value){
        errorMsg.value = "CPF é obrigatório!"
        return;
    }
    errorMsg.value = '';
}

function validateEmail(email, errorMsg){
    if(!email.value){
        errorMsg.value = "Email é obrigatório!"
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

<style scoped>
    .backlink {
        text-decoration: none;
    }

    .backlink:hover {
        cursor: pointer;
    }

</style>