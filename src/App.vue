<script setup>
import {ref} from 'vue'
import axios from 'axios'
import { RouterLink, RouterView } from 'vue-router'
const categories = ref({});

let showModal = ref(false)
let baseUrl = ref(localStorage.getItem('baseUrl'))

if(baseUrl.value == null || baseUrl.value == ''){
  localStorage.setItem('baseUrl', 'https://mehedipata.com/wp-json/wp/v2')
  baseUrl.value = localStorage.getItem('baseUrl')
}

function changeAPI(){
  localStorage.setItem('baseUrl', baseUrl.value)
  showModal.value = false
  fetchCategories()
}

async function fetchCategories(){
    try {
        const url = `${baseUrl.value}/categories`
        const res = await axios.get(url)
        Object.assign(categories.value, res.data)
    } catch (error) {
        console.error(error)
        return []
    }
}
fetchCategories()
</script>

<template>
    <nav class="bg-white dark:bg-gray-900 fixed w-full z-20 top-0 left-0 border-b border-gray-200 dark:border-gray-600">
      <div class="max-w-screen-xl flex flex-wrap items-center justify-between mx-auto p-4">
      <RouterLink to="/" class="flex items-center">
          <img src="../public/logo.svg" class="h-8 mr-3 rounded-full" alt="MehediPata Logo">
          <span class="self-center text-2xl font-semibold whitespace-nowrap dark:text-white">Simple Blog</span>
      </RouterLink>
      <div class="flex md:order-2">

      </div>
      <div class="items-center justify-between hidden w-full md:flex md:w-auto md:order-1" id="navbar-sticky">
        <ul class="flex items-center p-4 md:p-0 mt-4 font-medium border border-gray-100 rounded-lg bg-gray-50 md:flex-row md:space-x-8 md:mt-0 md:border-0 md:bg-white dark:bg-gray-800 md:dark:bg-gray-900 dark:border-gray-700">
          <li>
              <RouterLink to="/" class="block py-2 pl-3 pr-4 text-white bg-blue-700 rounded md:bg-transparent md:text-blue-700 md:p-0 md:dark:text-blue-500" aria-current="page">Home</RouterLink>
          </li>
          <li>
            <RouterLink to="/contact" class="block py-2 pl-3 pr-4 text-gray-900 rounded hover:bg-gray-100 md:hover:bg-transparent md:hover:text-blue-700 md:p-0 md:dark:hover:text-blue-500 dark:text-white dark:hover:bg-gray-700 dark:hover:text-white md:dark:hover:bg-transparent dark:border-gray-700">Contact</RouterLink>
          </li>
        </ul>
      </div>
      </div>
    </nav>

    <div class="grid grid-cols-12">
      <div class="col-span-9 p-10 mt-10 ">
        <RouterView />
      </div>
      <div class="col-span-3 pt-10 mt-10">
        <ul v-for="(category, index) in categories" :key="index" class="w-48 text-sm font-medium text-gray-900 bg-white border border-gray-200 rounded-lg dark:bg-gray-700 dark:border-gray-600 dark:text-white">
            <li class="w-full px-4 py-2 border-b border-gray-200 rounded-t-lg dark:border-gray-600">
              <RouterLink :to="`posts?categories=${category.id}`">{{ category.name }} ({{ category.count }})</RouterLink>
            </li>
        </ul>
      </div>
    </div>
  
    <footer class="bg-white rounded-lg shadow m-4 dark:bg-gray-800">
        <div class="w-full mx-auto max-w-screen-xl p-4 md:flex md:items-center md:justify-between">
          <span class="text-sm text-gray-500 sm:text-center dark:text-gray-400">© 2023 <a href="/" class="hover:underline">Simple Blog</a>. All Rights Reserved.
        </span>
        </div>
    </footer>

    <!-- component -->
    <div v-show="showModal" class="fixed left-0 top-0 flex h-full w-full items-center justify-center bg-black bg-opacity-50 py-10">
      <div class="max-h-full w-full max-w-xl overflow-y-auto sm:rounded-2xl bg-white">
        <div class="w-full">
          <div class="m-8 my-20 max-w-[400px] mx-auto">
            <div class="mb-8">
              <h1 class="mb-4 text-3xl font-extrabold">Change Wordpress API</h1>
              <input v-model="baseUrl" type="text" class="w-full py-2 px-5 focus:outline-none border border-black rounded-2xl" placeholder="e.g. https://mehedipata.com/wp-json/wp/v2">
            </div>
            <div class="space-y-4">
              <button @click.prevent="changeAPI()" class="p-3 bg-black rounded-full text-white w-full font-semibold">Change API</button>
              <button @click="showModal=false" class="p-3 bg-white border rounded-full w-full font-semibold">Cancel</button>
            </div>
          </div>
        </div>
      </div>
    </div>

</template>