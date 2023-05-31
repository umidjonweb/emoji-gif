<script setup>
import { ref } from 'vue';
import Emoji from '../components/EmojiCard.vue';
import Gif from '../components/GifCard.vue';

const context = ref()
const gifSearch = ref('Hello')
const isEmoji = ref(true)
function handleBtn(e) {
   if (e === 'emoji') {
      isEmoji.value = true
   }
   else {
      isEmoji.value = false
   }
}

</script>

<template>
   <div class="wrapper">
      <div class="tabs">
         <button :class="{ 'active': isEmoji }" @click="handleBtn('emoji')">Emoji</button>
         <button :class="{ 'active': !isEmoji }" @click="handleBtn('gif')">Gif</button>
      </div>
      <div ref="context" class="filter_bottom">
         <Emoji :item="1" v-if="isEmoji" />
         <Gif :search="gifSearch" v-if="!isEmoji" />
      </div>
      <div v-if="isEmoji" class="emoji_filter">
         <i @click="context.scrollTop = 0" class="ri-time-line"></i>
         <i @click="context.scrollTop = 2080" class="ri-bear-smile-line"></i>
         <i @click="context.scrollTop = 2610" class="ri-apple-line"></i>
         <i @click="context.scrollTop = 3100" class="ri-football-fill"></i>
         <i @click="context.scrollTop = 3350" class="ri-car-line"></i>
         <i @click="context.scrollTop = 4900" class="ri-lightbulb-line"></i>
         <i @click="context.scrollTop = context.scrollHeight" class="ri-heart-line"></i>
      </div>
      <div class="gif_search" v-if="!isEmoji">
         <i class="ri-search-line"></i>
         <input v-model="gifSearch" type="text" placeholder="Search Gif">
      </div>
   </div>
</template>

<style lang="scss" scoped>
.gif_search {
   display: flex;
   padding: 10px;
   padding: 10px 6px;
   background-color: rgb(184, 147, 216);

   i {
      cursor: pointer;
      padding: 4px;
      border-radius: 6px;
      font-size: 18px;
      color: rgb(217, 213, 213);
   }

   input {
      background-color: transparent;
      flex-grow: 1;
      border: 0;
      outline: 0;
      color: white;

      &::placeholder {
         color: rgb(217, 213, 213);
      }
   }
}

.emoji_filter {
   display: flex;
   align-items: center;
   gap: 15px;
   padding: 10px 6px;
   background-color: rgb(184, 147, 216);
   width: 100%;
   color: aliceblue;

   i {
      cursor: pointer;
      padding: 4px;
      border-radius: 6px;
      font-size: 18px;

      &:hover {
         background-color: blueviolet;
      }
   }
}

.filter_bottom {
   position: relative;
   height: 100%;
   width: 100%;
   overflow-y: scroll;
}

.wrapper {
   position: relative;
   width: 300px;
   height: 500px;
   background-color: rgb(199, 199, 226);
   border-radius: 13px;
   margin: auto;
   display: flex;
   justify-content: space-between;
   flex-direction: column;
   overflow: hidden;
}

.tabs {
   display: flex;
   padding: 5px 10px;
   background-color: rgb(184, 147, 216);
   padding-bottom: 0;

   button {
      padding: 5px;
      background-color: transparent;
      border: 0;
      border-bottom: 2px solid transparent;
      outline: 0;
      width: 50%;
      padding-bottom: 8px;
      color: rgb(217, 213, 213);
      font-weight: 700;
   }

   .active {
      border-bottom: 2px solid blueviolet;
      color: blueviolet;
   }
}
</style>