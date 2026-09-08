<template>
  <q-form @submit="onSubmit" class="q-gutter-md">
    <q-input v-model="name" label="Nombre" outlined :rules="[ val => !!val || 'El nombre es obligatorio']"/>
    <q-input v-model="phone" label="Teléfono" outlined maxlength="10" :rules="[ 
      val => !!val || 'El teléfono es obligatorio', 
      val => val.length === 10 || 'El teléfono debe contener exactamente 10 digitos']"/>
    <q-btn type="submit" label="Guardar prospecto" color="primary" :loading="loading" :disable="loading"/>
  </q-form>
</template>

<script setup>
//
import { ref } from 'vue';
import { useQuasar } from 'quasar';
const apiUrl = import.meta.env.QCLI_API_URL;
const name = ref('')
const phone = ref('')
const loading = ref(false)
const $q = useQuasar()
const onSubmit = async () => {
  loading.value = true
  try {
    const response = await fetch(`${apiUrl}/prospects`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Accept': 'application/json',
      },
      body: JSON.stringify({ name: name.value, phone: phone.value }),
    })
    const data = await response.json()
    $q.notify({
      type: response.ok ? 'positive' : 'negative',
      message: data.message,
    })
    if (response.ok) {
      name.value = ''
      phone.value = ''
    }
  } catch (error) {
    $q.notify({
      type: 'negative',
      message: `No se pudo conectar con el servidor`,
      timeout: 4000,
    })
    console.log(error)
  } finally {
    loading.value = false;
  }
};
</script>
