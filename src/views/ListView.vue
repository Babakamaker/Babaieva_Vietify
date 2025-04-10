<template>
  <v-text-field
    v-model="newItemTitle"
    label="Нова покупка"
    variant="outlined"
    append-icon="mdi-plus"
    clearable
    @click:append="addNewItem"
    @keyup.enter="addNewItem"
          ></v-text-field>
  <v-list>
    <div
      v-for="(item, index) in shoppingList"
      :key="item.id"
    >
<v-list-item
  @click="doneBought(item.id)"
  :class="item.bought ? 'bg-blue-lighten-5' : ''"
>

        <template #prepend>
          <v-checkbox
            :model-value="item.bought"
            @click.stop="doneBought(item.id)"
          />
        </template>

        <v-list-item-title>
          <span :class="item.bought ? 'text-decoration-line-through' : ''">
            {{ item.title }}
          </span>
        </v-list-item-title>

        <template #append>
          <v-list-item-action>
            <v-icon
              icon="mdi-delete"
              class="text-red"
              @click.stop="deleteItem(item.id)"
            />
          </v-list-item-action>
        </template>
      </v-list-item>

      <v-divider v-if="index < shoppingList.length - 1" />
    </div>
  </v-list>
</template>

<script setup>
import { ref } from 'vue'

const shoppingList = ref([
  { id: 1, title: 'Молоко', bought: false },
  { id: 2, title: 'Хліб', bought: false },
  { id: 3, title: 'Яйця', bought: false },
  { id: 4, title: 'Сир', bought: false }
])

const newItemTitle = ref('')

function doneBought(id) {
  const item = shoppingList.value.find(x => x.id === id)
  if (item) {
    item.bought = !item.bought
  }
}

function deleteItem(id) {
  shoppingList.value = shoppingList.value.filter(x => x.id !== id)
}

function addNewItem() {
  const title = newItemTitle.value.trim()
  if (title) {
    const newId = Math.max(...shoppingList.value.map(x => x.id)) + 1
    shoppingList.value.push({
      id: newId,
      title,
      bought: false
    })
    newItemTitle.value = ''
  }
}
</script>
