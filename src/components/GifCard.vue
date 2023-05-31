<script setup>

import { ref, watch, defineProps } from 'vue';
import axios from 'axios'
import { useClipboard } from '@vueuse/core'
import { useIntersectionObserver } from '@vueuse/core'
const props = defineProps({
   search: {
      type: String
   }
});
const source = ref('')
const gifs = ref([]);
const gifsList = ref([]);
const { copy } = useClipboard({ source })
const offset = ref(10)
let count = 0;
async function searchGifs() {
   const apiKey = 'ZUPBRFW773LORt43oMdNICDyC23pEPZa';
   const apiUrl = `https://api.giphy.com/v1/gifs/search?api_key=${apiKey}&q=${props.search}&limit=16&offset=${offset.value * count}`;
   try {
      const response = await axios.get(apiUrl)
      gifs.value = response.data;
   }
   catch (error) {
      console.log(error);
   }
}
searchGifs()

const target = ref(null)
const targetIsVisible = ref(false)

const { stop } = useIntersectionObserver(
   target,
   async ([{ isIntersecting }], observerElement) => {
      targetIsVisible.value = isIntersecting
      if (targetIsVisible.value) {
         count++
         await searchGifs();
         gifsList.value.push(...gifs.value.data)
      }
      console.log('observerElement', observerElement);
   },
   { rootMargin: '100px' }
)
console.log('stop', stop);
watch(() => props.search, async () => {
   gifsList.value = []
   await searchGifs()
   gifsList.value.push(...gifs.value.data)

})


</script>

<template>
   <div class="gif_content">
      <div class="gifList" v-for="(gif, ind) in gifsList" :key="ind">
         <div @click="copy(gif.images.original.url)" class="gif"
            :style="`background-image:url(${gif.images.original.url})`">
         </div>
      </div>
      <div ref="target" class="loading">
         <div class="lds-ring"><div></div><div></div><div></div><div></div></div>
      </div>
   </div>
</template>


<style lang="scss" scoped>
.gif_content {
   display: flex;
   justify-content: space-between;
   flex-wrap: wrap;
   margin-top: 20px;
}

.gifList {
   display: flex;
   justify-content: space-between;
}

.gif {
   cursor: pointer;
   width: 140px;
   height: 120px;
   background-repeat: no-repeat;
   background-size: cover;
   background-position: center;
   margin-bottom: 10px;
}
.loading {
   width: 100%;
   display: flex;
   align-items: center;
   justify-content: center;
 }
.lds-ring {
  display: inline-block;
  position: relative;
  width: 50px;
  height: 50px;
}

.lds-ring div {
  box-sizing: border-box;
  display: block;
  position: absolute;
  width: 46px;
  height: 46px;
  margin: 6px;
  border: 6px solid #fff;
  border-radius: 50%;
  animation: lds-ring 1.2s cubic-bezier(0.5, 0, 0.5, 1) infinite;
  border-color: #fff transparent transparent transparent;
}
.lds-ring div:nth-child(1) {
  animation-delay: -0.45s;
}
.lds-ring div:nth-child(2) {
  animation-delay: -0.3s;
}
.lds-ring div:nth-child(3) {
  animation-delay: -0.15s;
}
@keyframes lds-ring {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>