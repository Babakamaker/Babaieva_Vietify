<template>
  <v-sheet border rounded>
    <v-toolbar flat>
      <v-toolbar-title>
        <v-icon color="medium-emphasis" icon="mdi-account-group" size="x-small" start></v-icon>
        Моя група
      </v-toolbar-title>
    </v-toolbar>

    <v-form @submit.prevent="save" class="pa-4">
      <v-row>
        <v-col cols="12" md="6">
          <v-text-field v-model="record.fullName" label="ПІП" required></v-text-field>
        </v-col>
        <v-col cols="12" md="6">
          <v-text-field v-model="record.birthYear" label="Рік народження" type="number" required></v-text-field>
        </v-col>
        <v-col cols="12" md="6">
          <v-text-field v-model="record.phone" label="Телефон" required></v-text-field>
        </v-col>
        <v-col cols="12" md="6">
          <v-text-field v-model="record.email" label="Email" type="email" required></v-text-field>
        </v-col>
        <v-col cols="12" class="text-end">
          <v-btn
            type="submit"
            color="primary"
            :prepend-icon="isEditing ? 'mdi-check' : 'mdi-plus'"
          >
            {{ isEditing ? 'Оновити' : 'Додати' }}
          </v-btn>

        </v-col>
      </v-row>
    </v-form>

    <v-text-field
      v-model="search"
      label="Пошук"
      prepend-inner-icon="mdi-magnify"
      class="mx-4"
    ></v-text-field>

    <v-data-table
      :headers="headers"
      :items="filteredStudents"
      :search="search"
      class="px-4"
    >
      <template v-slot:item.actions="{ item }">
        <v-icon size="small" color="primary" @click="edit(item)">
          mdi-pencil
        </v-icon>
        <v-icon size="small" color="error" @click="remove(item.id)">
          mdi-delete
        </v-icon>
      </template>

      <template v-slot:no-data>
        <v-alert type="info" text="Немає даних"></v-alert>
      </template>
    </v-data-table>
  </v-sheet>
</template>

<script setup>
import { ref, computed } from 'vue'

const DEFAULT_RECORD = {
  id: null,
  fullName: '',
  birthYear: '',
  phone: '',
  email: ''
}

const record = ref({ ...DEFAULT_RECORD })
const isEditing = ref(false)
const search = ref('')
const students = ref([
  { id: 1, fullName: 'Бабаєва Вікторія Вікторівна', birthYear: 2005, phone: '+380988885340', email: 'viktoriia.babaieva@oa.edu.ua' },
  { id: 2, fullName: 'Коваль Ольга Валеріївна', birthYear: 2003, phone: '+380931112233', email: 'olga@example.com' },
  { id: 3, fullName: 'До Джон Доович', birthYear: 1999, phone: '+38095554448', email: 'john@example.com' }

])

const headers = [
  { title: 'ПІП', key: 'fullName' },
  { title: 'Рік народження', key: 'birthYear' },
  { title: 'Телефон', key: 'phone' },
  { title: 'Email', key: 'email' },
  { title: 'Дії', key: 'actions', sortable: false }
]

const filteredStudents = computed(() => {
  if (!search.value) return students.value
  const term = search.value.toLowerCase()
  return students.value.filter(student =>
    Object.values(student).some(val =>
      String(val).toLowerCase().includes(term)
    )
  )
})

function save() {
  if (isEditing.value) {
    const index = students.value.findIndex(s => s.id === record.value.id)
    students.value[index] = { ...record.value }
  } else {
    record.value.id = students.value.length + 1
    students.value.push({ ...record.value })
  }
  reset()
}

function edit(item) {
  isEditing.value = true
  record.value = { ...item }
}

function remove(id) {
  const index = students.value.findIndex(s => s.id === id)
  students.value.splice(index, 1)
}

function reset() {
  record.value = { ...DEFAULT_RECORD }
  isEditing.value = false
}
</script>
