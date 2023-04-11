<script setup>
import { onMounted, ref } from "vue";
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls.js";
import SplineLoader from "@splinetool/loader";

// import { ScrollTrigger } from "gsap/ScrollTrigger";
import gsap from "gsap";
import Navigation from "./components/Naviagtion.vue";

const isLoading = ref(true);
const navigation = ref(null);
const currentTab = ref(0);
const isModalOpen = ref(false);
const handleModal = (val) => {
  isModalOpen.value = val;
};
const changeTab = (i) => {
  currentTab.value = i;
};
onMounted(() => {
  gsap.set(".slider", { y: "100%" }); 
//   window.addEventListener('message', (e)=>{
//     isLoading.value = false
//  })
});
</script>

<template>
  <div>
    <iframe src="https://room-layer.vercel.app/" class="iframw" frameborder="0"></iframe>
    <loader v-if="isLoading"></loader>
    <navigation
      @change-tab="changeTab"
      :currentTab="currentTab"
      @handle-modal="handleModal"
      ref="navigation"
    ></navigation>
  </div>
</template>

<style scoped>
body {
  margin: 0;
}
html,
body {
  height: 100%;
  overflow: hidden;
}
canvas {
  display: block;
}
iframe{
  height: 100vh;
  width: 100vw;
}
* {
  cursor: pointer;
  -webkit-tap-highlight-color: transparent;
  font-family: "Poppins", sans-serif;
  box-sizing: border-box;
}
.loader {
  height: 100vh;
  width: 100vw;
  background: black;
  position: fixed;
  top: 0;
  left: 0;
  z-index: 10;
}
.loader_text {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  color: white;
  line-height: 126%;
  letter-spacing: 0.02em;
}
.nav {
  min-width: 320px;
  max-width: 340px;
  width: 100%;
  height: 48px;
  background: rgba(12, 12, 13, 0.29);
  backdrop-filter: blur(10px);
  border-radius: 61px;
  position: absolute;
  top: 12px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 6px 12px;
  z-index: 1;
}
.profile {
  display: flex;
  gap: 8px;
  color: white;
  align-items: center;
}
.profile img {
  height: 36px;
  width: 36px;
  cursor: pointer;
}
.profile p {
  font-weight: 500;
  font-size: 16px;
  line-height: 136%;
  /* identical to box height, or 22px */

  letter-spacing: 0.02em;
  font-feature-settings: "salt" on, "ss02" on, "ss01" on;

  /* Tonal Palette/Tonal 100 */

  color: #ffffff;
}
.slider {
  background: rgba(12, 12, 13, 0.29);
  backdrop-filter: blur(10px) contrast(100%);
  border-radius: 36px 36px 0px 0px;
  width: 100%;
  height: 50%;
  display: flex;
  flex-direction: column;
  align-items: center;
  /* overflow: hidden; */
}
.container {
  display: flex;
  align-items: center;
  flex-direction: column;
  margin: 10px 24px;
  gap: 21px;
  min-width: 320px;
  max-width: 360px;
  width: 100%;
  -ms-overflow-style: none; /* Internet Explorer 10+ */
  scrollbar-width: none;
  overflow: hidden;
  overflow-y: scroll;
  margin-top: 36px;
  border-radius: 10px;
}
.slider-pill {
  width: 54px;
  height: 4px;
  background: white;
  border-radius: 28px;
}
.all-link {
  display: flex;
  gap: 16px;
}
.link {
  height: 32px;
  width: 32px;
  border-radius: 50%;
}
.main-links {
  display: flex;
  flex-direction: column;
  gap: 16px;
  width: 100%;
  height: 600px;
  overflow: hidden;
  overflow-y: auto;
  align-items: center;
}
.container::-webkit-scrollbar {
  display: none; /* Safari and Chrome */
}
.main-links .cta-links {
  height: 172px;
  min-width: 280px;
  max-width: 340px;
  width: 100%;
  border-radius: 8px;
  background: black;
}
.show {
  display: block;
}
.slider-trigger {
  margin-top: 12px;
}
.link-nav {
  display: flex;
  max-width: 320px;
  overflow-x: scroll;
  min-height: 24px;
  align-items: center;
  -ms-overflow-style: none; /* IE and Edge */
  scrollbar-width: none;
  margin-top: 30px;
}
.link-nav::-webkit-scrollbar {
  display: none;
}

.nav-pills {
  border-radius: 8px;
  height: 24px;
  padding: 4px 16px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 14px;
  font-weight: 500;
  line-height: 136%;
  letter-spacing: 0.02em;
  color: #ffffff;
}
.current {
  background: white;
  color: black;
}
</style>
