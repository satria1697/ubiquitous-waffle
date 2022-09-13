<script setup>
import QrcodeVue from "qrcode.vue"
import {ref} from "vue";

const code = ref("")

const downloadFile = () => {
  const link = document.createElement('a');
  link.download = `${code.value}.png`;
  link.href = document.getElementsByTagName('canvas')[0].toDataURL()
  link.click();
}
</script>

<template>
  <div class="w-screen xl:h-screen h-[calc(100vh-200px)] grid place-items-center">
      <div class="flex flex-col space-y-4">
        <div class="flex justify-center">
          <qrcode-vue :value="code" />
        </div>
        <input class="p-2 border-4 border-black rounded-md" v-model="code" />
        <button type="submit" @click="downloadFile">Download</button>
      </div>
  </div>
</template>

<style scoped>
canvas {
  height: 80% !important;
  width: 80% !important;
}
</style>
