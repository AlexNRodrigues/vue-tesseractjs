<template>
  <button
    @click="copyToClipboard"
    class="border border-slate-600 hover:border-slate-300 rounded-md p-2 ml-auto -mr-3"
  >
    <IconCopy v-if="!copied" />
    <IconCopied v-else />
  </button>
</template>

<script setup>
import IconCopy from './icons/IconCopy.vue'
import IconCopied from './icons/IconCopied.vue'
import { ref } from 'vue'

const copied = ref(0)

function copyToClipboard() {
  let text = document.querySelector('#text-recognized').innerHTML

  navigator.clipboard
    .writeText(text)
    .then(() => {
      copied.value = 1

      setTimeout(() => {
        copied.value = 0
      }, 3000)
    })
    .catch((err) => {
      console.error('Failed to copy: ', err)
    })
}
</script>
