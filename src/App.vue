<script setup>
import { ref } from 'vue';
import emojis from 'emojibase-data/en/data.json'
import axios from 'axios'
import { useClipboard } from '@vueuse/core'

const searchQuery = ref('bean');
const gifs = ref([]);
const source = ref('')
const isEmoji = ref(true)

function showEmoji(emoji) {
   alert(emoji)
}

async function searchGifs() {
   const apiKey = 'ZUPBRFW773LORt43oMdNICDyC23pEPZa';
   const apiUrl = `https://api.giphy.com/v1/gifs/search?api_key=${apiKey}&q=${searchQuery.value}`;
   try {
      const response = await axios.get(apiUrl)
      gifs.value = response.data;
   }
   catch (error) {
      console.log(error);
   }

}
searchGifs()

function handleBtn(e) {
   if (e === 'emoji') {
      isEmoji.value = true
   }
   else {
      isEmoji.value = false
   }
}

const { copy } = useClipboard({ source })

</script>

<template>
   <div class="wrapper">
      <div class="tabs">
         <button :class="{ 'active': isEmoji }" @click="handleBtn('emoji')">Emoji</button>
         <button :class="{ 'active': !isEmoji }" @click="handleBtn('gif')">Gif</button>
      </div>
      <div v-if="isEmoji" class="home">
         <p class="emoji_card" v-for="(e, index) of emojis" :key="index">
            <span class="emoji" @click="showEmoji(e.emoji)">{{ e.emoji }}</span>
         </p>
      </div>
      <div class="gif_content" v-else>
         <div class="gifList" v-for="(gif, ind) in gifs.data" :key="ind">
            <div @click="copy(gif.images.original.url)" class="gif"
               :style="`background-image:url(${gif.images.original.url})`">
            </div>
         </div>
      </div>
   </div>
</template>

<style lang="scss" scoped>
.wrapper {
   width: 300px;
   height: 500px;
   overflow-y: scroll;
   background-color: rgb(199, 199, 226);
   border-radius: 13px;
}

.gif_content {
   display: flex;
   justify-content: space-between;
   flex-wrap: wrap;
   margin-top: 20px;
}

.tabs {
   display: flex;
   padding: 5px 10px;

   button {
      padding: 5px;
      background-color: transparent;
      border: 0;
      border-bottom: 2px solid transparent;
      outline: 0;
      width: 50%;
   }

   .active {
      border-bottom: 2px solid blueviolet;
      color: blueviolet;
   }

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

.emoji {
   user-select: none;
   cursor: pointer;
}

.home {
   padding: 10px;
   display: grid;
   grid-template-columns: repeat(8, minmax(0, 1fr));
}

.emoji_card {
   margin: 4px 0;
}

::-webkit-scrollbar {
   width: 4px;
}

/* Track */
::-webkit-scrollbar-track {
   background: #f1f1f1;
}

/* Handle */
::-webkit-scrollbar-thumb {
   background: #888;
}

/* Handle on hover */
::-webkit-scrollbar-thumb:hover {
   background: #555;
}
</style>
