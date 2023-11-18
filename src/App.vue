<template>
  <div class="h-[185px] bg-gray-500"></div>
  <header class="mx-auto max-w-3xl -mt-24 flex flex-col gap-8 px-4">
    <h1 class="text-gray-100 text-heading1 font-bold">Lista de compras</h1>
    <form
      class="flex flex-col sm:flex-row sm:items-end w-full gap-3"
      @submit.prevent="submit"
    >
      <label
        for="item"
        class="flex flex-col gap-2 text-gray-200 text-body font-normal w-full sm:max-w-xs"
      >
        Item
        <input
          id="item"
          v-model="item"
          type="text"
          class="p-3 text-gray-100 h-10 rounded-md bg-gray-500 border border-gray-300 outline-0 focus:border-purple focus:border-2 transition-all"
        />
      </label>
      <div class="flex gap-3 items-end w-full">
        <label
          for="quantidade"
          class="flex flex-col gap-2 text-gray-200 text-body font-normal"
        >
          Quantidade
          <div class="flex h-10">
            <input
              id="quantidade"
              type="number"
              v-model="quantidade"
              class="p-3 text-gray-100 rounded-l-md bg-gray-500 border border-gray-300 outline-0 max-w-[66px] sm:max-w-[80px] focus:border-purple focus:border-2 transition-all"
            />
            <select
              v-model="metric"
              class="h-10 bg-gray-400 rounded-r-md border border-y-gray-300 border-l-0 focus:outline focus:outline-purple focus:outline-2 border-r-gray-300 px-3 cursor-pointer transition-all"
            >
              <option
                selected
                class="py-3 text-gray-100 font-normal text-button cursor-pointer"
              >
                Un.
              </option>
              <option
                selected
                class="py-3 text-gray-100 font-normal text-button cursor-pointer"
              >
                L
              </option>
              <option
                selected
                class="py-3 text-gray-100 font-normal text-button cursor-pointer"
              >
                Kg
              </option>
            </select>
          </div>
        </label>
        <label
          for="categoria"
          class="flex flex-col gap-2 text-gray-200 text-body font-normal flex-1"
        >
          Categoria
          <select
            id="categoria"
            v-model="categoria"
            class="h-10 rounded-md bg-gray-400 border border-gray-300 px-3 outline-none focus:border-purple focus:border-2 transition-all"
          >
            <option disabled selected value="">Selecione</option>
            <option class="text-gray-100 bg-gray-400 text-button">Fruta</option>
            <option class="text-gray-100 bg-gray-400 text-button">
              Bebida
            </option>
            <option class="text-gray-100 bg-gray-400 text-button">
              Legume
            </option>
            <option class="text-gray-100 bg-gray-400 text-button">
              Padaria
            </option>
            <option class="text-gray-100 bg-gray-400 text-button">Carne</option>
          </select>
        </label>
        <button
          class="rounded-full flex items-center justify-center h-10 w-10 bg-purple hover:bg-purple-dark transition-all"
        >
          <img class="h-6 w-6 block" src="./assets/icons/plus.svg" alt="" />
        </button>
      </div>
    </form>
  </header>
  <main class="mx-auto max-w-3xl mt-10 flex flex-col gap-2 px-4">
    <div
      v-for="product in products"
      :key="item.id"
      class="bg-gray-400 p-4 rounded-md flex justify-between items-center border border-gray-300"
    >
      <div class="flex items-center gap-4">
        <input
          type="checkbox"
          class="checkbox rounded-sm checkbox-primary"
          v-model="product.done"
        />
        <div class="flex flex-col gap-0.5">
          <strong class="text-gray-100 font-bold text-heading2">{{
            product.title
          }}</strong>
          <p class="text-gray-200 text-body">
            {{ product.unidades }}
            <template v-if="product.metric === 'L'"> Litros </template>
            <template v-else-if="product.metric === 'Un.'"> Unidades </template>
            <template v-else-if="product.metric === 'Kg'"> Kilos </template>
          </p>
        </div>
      </div>
      <div class="flex items-center gap-3">
        <div
          :class="[
            setBackgroundTagColor(product.categoria),
            'rounded-full p-2 sm:px-4 flex items-center gap-1.5',
          ]"
        >
          <img :src="setIconTag(product.categoria)" alt="" />
          <span
            :class="[
              setTextTagColor(product.categoria),
              'hidden sm:block font-semibold text-tag',
            ]"
            >{{ product.categoria }}</span
          >
        </div>
        <div
          @click="deleteProduct(product.id)"
          class="p-2 rounded-full cursor-pointer hover:bg-gray-300 transition-all"
        >
          <img class="w-5 h-5" src="./assets/icons/trash.svg" alt="" />
        </div>
      </div>
    </div>
  </main>
</template>

<script setup>
import { reactive, ref } from 'vue'
const item = ref('')
const quantidade = ref(1)
const metric = ref('Un.')
const categoria = ref('')
import appleIcon from './assets/icons/apple.svg'
import milkIcon from './assets/icons/milk.svg'
import carrotIcon from './assets/icons/carrot.svg'
import beefIcon from './assets/icons/beef.svg'
import sandwichIcon from './assets/icons/sandwich.svg'

const backgroundTagColor = ref({
  Fruta: 'bg-orange-dark',
  Bebida: 'bg-blue-dark',
  Legume: 'bg-green-dark',
  Carne: 'bg-pink-dark',
  Padaria: 'bg-yellow-dark',
})
const textTagColor = ref({
  Fruta: 'text-orange',
  Bebida: 'text-blue',
  Legume: 'text-green',
  Carne: 'text-pink',
  Padaria: 'text-yellow',
})
const iconTag = ref({
  Fruta: appleIcon,
  Bebida: milkIcon,
  Legume: carrotIcon,
  Carne: beefIcon,
  Padaria: sandwichIcon,
})

const setBackgroundTagColor = category => {
  return backgroundTagColor.value[category] || ''
}

const setTextTagColor = category => {
  return textTagColor.value[category] || ''
}

const setIconTag = category => {
  return iconTag.value[category] || ''
}

const products = reactive([
  {
    id: 1,
    title: 'Leite',
    unidades: 2,
    done: false,
    categoria: 'Bebida',
    metric: 'Un.',
  },
  {
    id: 2,
    title: 'Maça',
    unidades: 4,
    done: false,
    categoria: 'Fruta',
    metric: 'Un.',
  },
])

function submit() {
  if (item.value === '' || categoria.value === '') {
    console.log('Insira um item e selecione a categoria')
    return
  }
  const newItem = {
    id: products.length + 1,
    title: item.value,
    unidades: quantidade.value,
    categoria: categoria.value,
    metric: metric.value,
  }
  products.push(newItem)
  item.value = ''
  quantidade.value = 1
  metric.value = 'Un.'
  categoria.value = ''
}

function deleteProduct(id) {
  const product = products.findIndex(product => product.id === id)

  if (product !== -1) {
    products.splice(product, 1)
  }
}
</script>