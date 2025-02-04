<script setup>
import qrcode from '@/assets/qrCode.webp'
import { ref, onMounted } from 'vue'

const time = ref(3600)
const formattedTime = ref('');

// Ao iniciar o componente, inicia uma contagem regressiva de 
// 3600 segundos (1 hora).
onMounted(() => {
    const interval = setInterval(() => {
        if (time.value > 0) {
            time.value--;
            formattedTime.value = formatTime(time.value);
        } 
        else {
            clearInterval(interval);
        }
    }, 1000);
});

// Formata o tempo atual para mostrar para o usuário
function formatTime(currentTime){
    const minutes = Math.floor(currentTime/60);
    const seconds = currentTime % 60;

    return `00:${String(minutes).padStart(2, '0')}:${String(seconds).padStart(2, '0')}`;
}

</script>

<template>
    <div class="d-flex justify-content-center align-items-center" style="height: 80vh;">
        <div class="card w-50">
            <h3 class="card-header">Pagamento por Pix</h3>
            <img class="card-img-top mx-auto w-50 mt-2" :src="qrcode" alt="QR Code">
            <div class="card-body text-center">
                <h5 v-if="time > 0">Expira em: <strong>{{ formattedTime }}</strong></h5>
                <h5 v-else class="text-danger">QR Code Expirado</h5>
            </div>
        </div>
    </div>
</template>