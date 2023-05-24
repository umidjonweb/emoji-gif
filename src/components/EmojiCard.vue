<script setup>
   import emojis from 'emojibase-data/en/data.json'
    function showEmoji(emoji) {
      alert(emoji)
    }
</script>
   
<template>
       <div  class="home">
         <p class="emoji_card" v-for="(e, index) of emojis" :key="index">
            <span class="emoji" @click="showEmoji(e.emoji)">{{ e.emoji }}</span>
         </p>
      </div>
</template>

<style lang="scss" scoped>
  .home {
   padding: 10px;
   display: grid;
   grid-template-columns: repeat(8, minmax(0, 1fr));
}
.emoji_card {
   margin: 4px 0;
}
.emoji {
   user-select: none;
   cursor: pointer;
}
</style>