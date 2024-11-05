<template>
  <div>
    <h1>Enviar Arquivo EDIFACT</h1>
    
    <!-- Formulário para upload do arquivo -->
    <form @submit.prevent="uploadFile">
      <input type="file" @change="handleFileChange" />
      <button type="submit">Enviar Arquivo</button>
    </form>

    <!-- Exibição do JSON retornado pela API -->
    <div v-for="(values, key) in jsonData" :key="key">
  <h3>{{ key }}</h3>
  <ul>
    <li v-for="(value, index) in values" :key="index">
      {{ value }}
    </li>
  </ul>
</div>

  </div>
</template>

<script setup>
import { ref } from 'vue';

// Estados para o arquivo e dados JSON
const file = ref(null);
const jsonData = ref(null);

// Captura o arquivo quando o usuário o seleciona
const handleFileChange = (event) => {
  file.value = event.target.files[0];
};

// Função para enviar o arquivo para a API FastAPI
const uploadFile = async () => {
  try {
    if (!file.value) {
      alert("Por favor, selecione um arquivo.");
      return;
    }

    // Cria o FormData e anexa o arquivo
    const formData = new FormData();
    formData.append("file", file.value);

    // Faz a requisição para a API usando $fetch
    jsonData.value = await $fetch('http://127.0.0.1:8000/parse-edifact', {
      method: 'POST',
      body: formData
    });
  } catch (error) {
    console.error("Erro ao enviar o arquivo:", error);
  }
};
</script>

<style>
/* Estilização opcional */
h1, h2 {
  color: #333;
}
pre {
  background: #f4f4f4;
  padding: 10px;
  border-radius: 5px;
}
</style>
