<script setup>
import {ref} from "vue";
import TheMessage from "./components/TheMessage.vue"

let generatedPassword = ref("Generated Password")

let passwordLength = ref(10)

let includeLowerCase = ref(false)

let includeUpperCase = ref(false)

let includeNumber = ref(false)

let includeSymbol = ref(false)

let showMessage = ref(false)

const generatePassword = () => {

  let password = ""
  let chars = ""

  let lowercaseChars = "abcdefghijklmnopqrstuvwxyz"

  let uppercaseChars = lowercaseChars.toUpperCase()

  let numbers = "1234567890"

  let symbols = "~`!@#$%^&*()_-=+?"


  if (!passwordLength.value) {
    return
  }

  if (!includeLowerCase.value && !includeUpperCase.value && !includeNumber.value && !includeSymbol.value ){
    includeLowerCase.value = true
  }


  if (includeLowerCase.value){
    chars+=lowercaseChars
  }

  if (includeUpperCase.value) {
    chars += uppercaseChars
  }
  if (includeNumber.value) {
    chars += numbers
  }
  if (includeSymbol.value) {
    chars += symbols
  }

  for (let i = 0; i < passwordLength.value; i++) {
    password += chars.charAt(Math.floor(Math.random() * chars.length))
  }
  generatedPassword.value = password
}

const copyPassword = () => {
  navigator.clipboard.writeText(generatedPassword.value)
  showMessage.value = true
  setTimeout(() => {
    showMessage.value = false
  }, 2500)

}

</script>

<template>
  <main class="h-screen bg-black text-white px-4 overflow-hidden">
    <div class="container mx-auto pt-8">

      <div class="flex justify-between items-center bg-gray-800 p-4 mb-4">
        <Transition name="slide-fade">
        <p :key="generatedPassword" class="text-gray-400 text-xl font-bold">{{ generatedPassword }}</p>
        </Transition>
        <button class="focus:outline-white">
          <svg class="cursor-pointer" @click="copyPassword" xmlns="http://www.w3.org/2000/svg" height="24" viewBox="0 96 960 960" width="24"><path :class="{'fill-white':showMessage}" class="fill-emerald-500" d="M780 896H260q-24 0-42-18t-18-42V196q0-24 18-42t42-18h348l232 232v468q0 24-18 42t-42 18ZM578 394V196H260v640h520V394H578Zm-438 622q-24 0-42-18t-18-42V337h60v619h498v60H140Zm120-820v198-198 640-640Z"/></svg>
        </button>
      </div>
      <form class="flex flex-col bg-gray-800 p-4">
        <div class="flex justify-between items-center text-lg">
          <label>Character length</label>
          <p class="text-emerald-200 font-bold">{{ passwordLength }}</p>
        </div>
        <input
            class="w-full h-3 bg-gray-200 rounded-lg appearance-none cursor-pointer range-lg dark:bg-gray-700 mt-4 accent-emerald-400 focus:outline-white"
            v-model="passwordLength" type="range" min="1" max="20">
        <div class="flex flex-col gap-4 my-4 text-lg">
          <p class="flex items-center space-x-4">
            <input class="accent-emerald-400 focus:outline-white" v-model="includeLowerCase" type="checkbox">
            <label>Include Lowercase Letters</label>
          </p>
          <p class="flex items-center space-x-4">
            <input class="accent-emerald-400 focus:outline-white" v-model="includeUpperCase" type="checkbox">
            <label>Include Uppercase Letters</label>
          </p>
          <p class="flex items-center space-x-4">
            <input class="accent-emerald-400 focus:outline-white" v-model="includeNumber" type="checkbox">
            <label>Include Numbers</label>
          </p>
          <p class="flex items-center space-x-4">
            <input class="accent-emerald-400 focus:outline-white" v-model="includeSymbol" type="checkbox">
            <label>Include Symbols</label>
          </p>
        </div>
        <input class="bg-emerald-400 p-2 text-black font-medium uppercase focus:outline-white"
               @click.prevent="generatePassword" type="submit" value="Generate">
      </form>
    </div>
    <Transition>
      <TheMessage v-if="showMessage"/>
    </Transition>
  </main>
</template>

<style>
.v-enter-active,
.v-leave-active {
  transition: opacity 0.3s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}

.slide-fade-enter-active {
  transition: all 0.3s ease-out;
  opacity: 0;
}

.slide-fade-leave-active {
  transition: all 0.2s ease-in-out;

}

.slide-fade-enter-from,
.slide-fade-leave-to {
  transform: translateX(100%);
}
</style>