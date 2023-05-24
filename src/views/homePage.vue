 <script setup>
  import { ref } from 'vue';
  import Emoji from '../components/EmojiCard.vue';
  import Gif  from '../components/GifCard.vue';
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
      <Emoji v-if="isEmoji"/>
      <Gif v-if="!isEmoji"/>
   </div>
</template>

<style lang="scss" scoped>
.wrapper {
   width: 300px;
   height: 500px;
   overflow-y: scroll;
   background-color: rgb(199, 199, 226);
   border-radius: 13px;
   margin: auto;
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
</style>