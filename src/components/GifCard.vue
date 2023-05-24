<script setup>

import { ref } from 'vue';
import axios from 'axios'
import { useClipboard } from '@vueuse/core'
import { useIntersectionObserver  } from '@vueuse/core'

const searchQuery = ref('Mis');
const source = ref('')
const gifs = ref([]);
const gifsList = ref([]);
const { copy } = useClipboard({ source })
const offset = ref(10)
let count = 0;
async function searchGifs() {
   const apiKey = 'ZUPBRFW773LORt43oMdNICDyC23pEPZa';
   const apiUrl = `https://api.giphy.com/v1/gifs/search?api_key=${apiKey}&q=${searchQuery.value}&limit=16&offset=${offset.value * count}`;
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
        console.log('observerElement',observerElement);
       },
      { rootMargin:'100px'}
)
    console.log('stop',stop);


</script>

<template>
    <div class="gif_content">
         <div class="gifList" v-for="(gif, ind) in gifsList" :key="ind">
            <div @click="copy(gif.images.original.url)" class="gif"
               :style="`background-image:url(${gif.images.original.url})`">
            </div>
         </div>
         <div ref="target" >  
             Loading...
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

</style>