<template>
  <div class="main-links">
    <img v-for="(link,i) in links" :src="link" :key="link+i" alt="cta-tab" class="cta-links">
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