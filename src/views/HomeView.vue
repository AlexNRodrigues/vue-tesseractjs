<template>
  <section class="text-gray-400 bg-gray-900 body-font">
    <div class="container px-5 py-24 mx-auto flex flex-wrap items-center gap-3 md:gap-0">
      <BaseImageUpload @text="recognizedText" />

      <div
        class="lg:w-3/5 md:w-1/2 p-8 pt-6 border border-slate-600 rounded-lg flex flex-col justify-start items-start h-[420px] w-full overflow-y-auto"
      >
        <ButtonCopy v-if="text" />
        <p id="text-recognized" class="leading-relaxed m-0" v-if="text">{{ text }}</p>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref } from 'vue'
import BaseImageUpload from '../components/BaseImageUpload.vue'
import ButtonCopy from '../components/ButtonCopy.vue'

const text = ref(null)

const recognizedText = (event) => {
  text.value = event
}
</script>

<style scoped></style>
