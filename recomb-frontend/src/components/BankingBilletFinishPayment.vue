<script setup>
import codigoBarras from '@/assets/codigoBarras.webp'

const dueDate = new Date();
dueDate.setDate(dueDate.getDate() + 14);

</script>

<template>
    <div class="d-flex justify-content-center align-items-center" style="height: 80vh;">
        <div class="card w-50">
            <h3 class="card-header">Pagamento por Boleto</h3>
            <img class="card-img-top mx-auto w-50 mt-2" :src="codigoBarras" alt="QR Code">
            <div class="card-body d-flex align-items-center justify-content-between">
                <h5>Vencimento em: <strong> {{ dueDate.toLocaleDateString() }}</strong></h5>
                <button class="btn btn-primary">Baixar PDF</button>
            </div>
        </div>
    </div>
</template>