<script setup>
import QrcodeVue from "qrcode.vue"
import {ref} from "vue";

const code = ref("")
const img = ref(null)

const generateImage = () => new Promise((resolve) => {
  const base64 = document.getElementsByTagName('canvas')[0].toDataURL()

  const canvas = document.createElement('canvas')
  canvas.width = 120;
  canvas.height = 120;
  const context = canvas.getContext('2d')
  context.fillStyle = "white"
  context.fillRect(0,0, canvas.width, canvas.height)

  const image = new Image()
  image.src = base64
  image.onload = () => {
    context.drawImage(image, 10,10)
    context.save()
    img.value = context.canvas.toDataURL()
    resolve()
  }
})
const downloadFile = async () => {
  const link = document.createElement('a');
  link.download = `${code.value}.png`;
  link.href = img.value
  link.click();
}
</script>

<template>
  <div class="w-screen xl:h-screen h-[calc(100vh-200px)] grid place-items-center">
      <div class="flex flex-col space-y-4">
        <div class="w-full h-full" v-if="img">
          <img id="image" class="w-full h-full" :src="img">
        </div>
        <div class="flex justify-center hidden">
          <qrcode-vue :value="code" />
        </div>
        <form class="flex flex-col" @submit.prevent="generateImage">
          <input class="p-2 border-4 border-black rounded-md" v-model="code" />
          <div class="flex space-x-3">
<!--            <button type="submit" @click="previewFile">Preview</button>-->
            <button type="button" @click="downloadFile">Download</button>
          </div>
        </form>
      </div>
  </div>
</template>