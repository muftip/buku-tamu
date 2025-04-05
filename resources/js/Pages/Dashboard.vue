<template>
    <div class="max-w-xl mx-auto">
      <h1 class="text-2xl font-bold mb-4">Buku Tamu</h1>
  
      <form @submit.prevent="submitForm" class="space-y-3 mb-6">
        <input v-model="form.name" type="text" placeholder="Nama" class="w-full p-2 border rounded" />
        <textarea v-model="form.message" placeholder="Pesan" class="w-full p-2 border rounded"></textarea>
        <button type="submit" class="bg-blue-500 text-white px-4 py-2 rounded">Kirim</button>
      </form>
  
      <div v-for="entry in entries" :key="entry.id" class="border-b py-2">
        <strong>{{ entry.name }}</strong> <small class="text-gray-500">{{ entry.created_at }}</small>
        <p>{{ entry.message }}</p>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue'
  import axios from 'axios'
  
  const form = ref({ name: '', message: '' })
  const entries = ref([])
  
  const fetchEntries = async () => {
    const res = await axios.get('/api/guestbook')
    entries.value = res.data
  }
  
  const submitForm = async () => {
    await axios.post('/api/guestbook', form.value)
    form.value.name = ''
    form.value.message = ''
    fetchEntries()
  }
  
  onMounted(fetchEntries)
  </script>
  