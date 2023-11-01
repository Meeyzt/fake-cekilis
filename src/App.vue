<script setup>
import { ref } from 'vue';

const showResult = ref(false);
const names = ref('');
const results = ref([]);

function handleRandom() {
  const array = names.value.split(',').map((item) => item.trim());

  const oznur = array.findIndex((item) => item.toLocaleLowerCase() == 'oznur' || item.toLocaleLowerCase() == 'öznur');
  const aleyna = array.findIndex((item) => item.toLocaleLowerCase() == 'aleyna');

  if(oznur > -1 && aleyna > -1) {
    results.value.push([array[oznur], array[aleyna]]);
    array.splice(oznur, 1);
    array.splice(aleyna, 1);
  }

  for (let i = 0; i < array.length; i += 2) {
    if (i + 1 < array.length) {
      results.value.push([array[i], array[i + 1]]);
    } else {
      results.value.push([array[i]]);
    }
  }

  results.value.sort(() => Math.random() - 0.5);

  showResult.value = true;
}

function closeModal() {
  showResult.value = false;
  results.value = [];
}
</script>

<template>
  <div class="w-full h-full overflow-hidden bg-gray-900 flex flex-col gap-5">
    <div class="w-full text-white text-3xl h-24 flex flex-col items-center justify-center mb-40">
      Kura Çek
      <div class="text-sm text-gray-400 text-center">
        Arkadaşlarınla bir karar vermeye mi ihtiyacın var? Kura çek! <br>
        Aşağıdaki detayları doldur ve butona tıkla.
      </div>
    </div>
    <div class="w-full flex flex-col justify-center items-center gap-6">
      <div class="overflow-hidden rounded-lg border border-gray-300 shadow-sm focus-within:border-indigo-500 focus-within:ring-1 focus-within:ring-indigo-500 w-[70%]">
        <textarea
          v-model="names"
          rows="10"
          class="block w-full resize-none border-0 text-gray-900 placeholder:text-gray-900 focus:ring-0 sm:text-sm sm:leading-6 outline-none p-2 bg-gray-500"
          placeholder="İsimleri virgül ile ayırarak giriniz. Örnek: Ayşe, Ali, Arda, Veli"
        ></textarea>
      </div>

      <button @click="handleRandom" type="button" class="rounded-md bg-indigo-600 px-3.5 py-2.5 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600 focus:ring-indigo-900 focus:ring-offset-2 focus:ring-2" >Çekiliş Yap</button>
    </div>
  </div>

  <Teleport to="body">
    <transition name="drawer">
      <div v-if="showResult" class="w-full h-[60%] rounded-t-xl bg-indigo-800 z-40 fixed bottom-0">
        <div class="tick" @click="closeModal"></div>
        <div class="text-white text-2xl w-full text-center pt-6">
          Kura Sonuçları
        </div>

        <ul class="pl-12 list-decimal pt-2">
          <li v-for="(result, index) in results" :key="index" class="w-full text-xl text-gray-200 py-2">
            {{result[0]}} - {{ result[1] }}
          </li>
        </ul>

        <div class="absolute bottom-4 w-full items-center justify-center inset-x-0 px-4">
          <button @click="closeModal" type="button" class="rounded-md bg-indigo-600 px-3.5 py-2.5 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600 focus:ring-indigo-900 focus:ring-offset-2 focus:ring-2 w-full">Tekrar Çek</button>
        </div>
      </div>
    </transition>
  </Teleport>
</template>

<style>
.tick::before {
  position: absolute;
  content: "";
  display: block;
  width: 36px;
  height: 4px;
  top: 8px;
  left: 50%;
  transform: translateX(-50%);
  border-radius: 2px;
  background-color: rgba(255,255,255,.60);
}

.drawer-enter-active,
.drawer-leave-active {
  transition: transform 0.3s ease-in-out;
}
.drawer-enter-from,
.drawer-leave-to {
  transform: translateY(100%);
}
.drawer-enter-to,
.drawer-leave-from {
  transform: translateY(0);
}
</style>
