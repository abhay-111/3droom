<template>
  <div class="main-links">
   <a target="_blank" href="https://www.linkedin.com/feed/">
     <img v-for="(link,i) in links" :src="link" :key="link+i" alt="cta-tab" class="cta-links">
   </a>
  </div>

</template>

<script setup>
import {defineProps} from "vue"
 defineProps({
    links:{
        type:Array,
        default:[]
    }
 })
</script>