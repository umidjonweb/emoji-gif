<script setup>
import emojis from 'emojibase-data/en/data.json'
console.log(emojis)
function showEmoji(emoji) {
   alert(emoji)
}
</script>
   
<template>
   <div class="home">
      <div class="emoji_card" v-for="(e, index) of emojis" :key="index">
         <span class="emoji" @click="showEmoji(e.emoji)">{{ e.emoji }}</span>
      </div>
   </div>
</template>

<style lang="scss" scoped>
.emoji_filter {
   display: flex;
   align-items: center;
   gap: 15px;
   padding: 10px 6px;
   background-color: blueviolet;
   width: 100%;
}

.home {
   padding: 10px;
   display: grid;
   grid-template-columns: repeat(8, minmax(0, 1fr));
}

.emoji_card {
   margin: 1px;
   padding: 3px;
   text-align: center;
   width: 20px;
   height: 20px;
   display: flex;
   justify-content: center;
   align-items: center;
   border-radius: 6px;
   &:hover {
      background-color: blueviolet;
   }
}

.emoji {
   user-select: none;
   cursor: pointer;
}
</style>