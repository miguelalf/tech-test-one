<script setup>
import { ref } from 'vue'

const personaje = ref()
const nombre = ref()
const numero = ref()
const altura = ref()
const peso = ref()
const imagen = ref()
const tipos = ref()

const buscarPersonaje = async () => {
  let pid = personaje.value

  if(!Number.isInteger(pid))
  {
    alert('Escribe un numero para continuar')
    return false
  }

  if(pid <= 0 || pid > 1025)
  {
    alert('Escribe un numero entre el 1 y 1025')
    return false
  }

  let options = {
    method: 'get',
    headers: {
        'Content-Type': 'application/json',
    },
  }

  await fetch('https://pokeapi.co/api/v2/pokemon/'+pid, options)
    .then((resp) => resp.json())
    .then((data) => {
      nombre.value = data.name
      numero.value = String(data.id).padStart(4, '0')
      altura.value = data.height
      peso.value = data.weight
      imagen.value = data.sprites.front_default
      tipos.value = data.types.map((data) => data.type.name)
    })
    .catch((err) => {
      alert('Error al consultar datos')
      console.log(err)
    })
}
</script>

<template>
  <div class="bg-black">
    <div class="flex items-center justify-center h-screen">
      <div class="max-w-sm rounded-md shadow-md overflow-hidden bg-white">
        <div class="p-5">
          <h1 class="text-xl font-semibold mb-2">Busca tu pokemon</h1>
          <p class="text-gray-600 text-sm">
            Escribe en la caja de texto un numero entre el 1 y 1025, te mostraremos cual personaje corresponde.
          </p>

          <div class="flex">
            <input type="number" id="personaje" class="w-full mt-4 px-4 py-2 border text-sm rounded-md shadow-xs" placeholder="Ej. 22" v-model="personaje">
          </div>
          <div class="flex">
            <button @click="buscarPersonaje" class="w-full mt-2 px-4 py-2 rounded-md text-white bg-blue-600 hover:bg-blue-800 transition">
              Buscar
            </button>
          </div>
          <div v-if="nombre" class="flex mt-4">
            <div class="w-1/2">
              <div class="flex items-center justify-center">
                <img :src="imagen" alt="Personaje" />
              </div>
            </div>
            <div class="w-1/2">
              <div class="text-sm">
                #{{ numero }}
              </div>
              <div class="text-sm font-bold">
                Nombre: <span class="font-normal capitalize">{{ nombre }}</span>
              </div>
              <div class="text-sm font-bold">
                Altura: <span class="font-normal">{{ altura }} pies</span>
              </div>
              <div class="text-sm font-bold">
                Peso: <span class="font-normal">{{ peso }} libras</span>
              </div>
              <div class="text-sm font-bold">
                Tipo: <span class="px-2 capitalize" v-for="(tipo, index) in tipos" :key="index">{{ tipo }}</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>