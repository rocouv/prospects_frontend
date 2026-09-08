<template>
  <q-form @submit="onSubmit" class="q-gutter-md">
    <q-input v-model="name" label="Nombre" outlined :rules="[ val => !!val || 'El nombre es obligatorio']"/>
    <q-input v-model="phone" label="Teléfono" outlined maxlength="10" :rules="[ 
      val => !!val || 'El teléfono es obligatorio', 
      val => /^\d{10}$/.test(val) || 'El teléfono debe contener exactamente 10 digitos']"/>
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
      body: JSON.stringify({
        name: name.value,
        phone: phone.value,
      }),
    })

    const data = await response.json()

    if (!response.ok) {
      const phoneError = data.errors?.phone?.[0]

      $q.notify({
        type: 'negative',
        message: phoneError || data.message || 'Ocurrió un error',
      })
      
      return
    }

    $q.notify({
      type: 'positive',
      message: data.message,
    })

    name.value = ''
    phone.value = ''

  } catch (error) {
    console.error(error)

    $q.notify({
      type: 'negative',
      message: 'No se pudo conectar con el servidor',
      timeout: 4000,
    })
  } finally {
    loading.value = false
  }
}
</script>
