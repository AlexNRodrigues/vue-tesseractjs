<template>
  <div
    class="lg:w-2/6 md:w-1/2 bg-gray-800 bg-opacity-50 rounded-lg p-8 flex flex-col justify-between md:mr-auto w-full mt-10 md:mt-0 h-[440px]"
  >
    <div
      class="base-image-input w-full h-auto min-h-[260px] bg-contain bg-center bg-no-repeat bg-gray-200 rounded-lg cursor-pointer"
      :style="{ 'background-image': `url(${imageData})` }"
      @click="chooseImage"
    >
      <span
        v-if="!imageData"
        class="placeholder w-full h-full flex justify-center items-center bg-gray-200 rounded-lg"
      >
        Choose an Image
      </span>
      <input class="hidden" ref="fileInput" type="file" @change="onSelectFile" />
    </div>

    <div v-if="progressInfo" class="flex justify-between items-center">
      {{ progressInfo }}% {{ status }}
      <button class=" p-1 border rounded" v-if="imageData" @click="removeImage">Remove</button>
    </div>
    <button
      class="text-white bg-indigo-500 border-0 py-2 px-8 focus:outline-none hover:bg-indigo-600 rounded text-lg"
      @click="recognizeText"
    >
      Recognize Text
    </button>
  </div>
</template>

<script setup>
import { createWorker } from 'tesseract.js'
import { ref } from 'vue'

const emit = defineEmits(['text'])

const imageData = ref('')
const fileInput = ref(null)

const chooseImage = () => {
  fileInput.value.click()
}

const onSelectFile = async () => {
  const files = fileInput.value.files

  if (files && files.length > 0) {
    try {
      const file = files[0]
      const imageDataUrl = await readFileAsync(file)
      imageData.value = imageDataUrl
    } catch (error) {
      console.error('Error reading the file:', error)
    }
  }
}

const readFileAsync = (file) => {
  return new Promise((resolve, reject) => {
    const reader = new FileReader()
    reader.onload = (e) => resolve(e.target.result)
    reader.onerror = (e) => reject(e)
    reader.readAsDataURL(file)
  })
}

let worker = null

const initializeWorker = async () => {
  worker = await createWorker('eng', 1, {
    logger: (m) => progressCallback(m)
  })

  await worker.load()
  await worker.loadLanguage('eng')
  await worker.reinitialize('eng')
}

const recognizeText = async () => {
  if (!worker) {
    await initializeWorker()
  }

  let txt = ''

  const {
    data: { text }
  } = await worker.recognize(imageData.value)

  emit('text', text)

  terminateWorker()

  return text
}

const terminateWorker = async () => {
  if (worker) {
    await worker.terminate()
    worker = null
  }
}

const progressInfo = ref(null)
const status = ref('')

const progressCallback = (callback) => {
  progressInfo.value = Math.round(callback.progress * 100)
  status.value = callback.status
}

const removeImage = () => {
  imageData.value = ''
  fileInput.value = null
  progressInfo.value = null
  status.value = ''

  emit('text', '')
}
</script>

<style scoped>
.placeholder {
  color: #333;
  font-size: 18px;
  font-family: Helvetica;
}
.placeholder:hover {
  background: #e0e0e0;
}
</style>
