<template>
  <div>
    <div class="nav">
      <div class="profile">
        <img
          src="https://res.cloudinary.com/qtalk/image/upload/v1679578980/ssup-landing/v2/blurs/JAM/image_103_ek4zex.png"
          alt=""
        />
        <p>Varun Duggirala</p>
      </div>
      <img
        style="height: 24px; width: 24px"
        @click="openModal"
        src="https://res.cloudinary.com/qtalk/image/upload/v1679578987/ssup-landing/v2/blurs/JAM/Frame_427319799_na3u4t.png"
        alt=""
      />
    </div>
    <div v-if="isModalOpen" @click="closeModal" class="layer-tp"></div>
    <div
      class="slider-parent"
      style="
        z-index: -1;
        height: 160%;
        width: 100%;
        position: absolute;
        bottom: 0px;
        transform: translateY(50%);
      "
    >
      <div class="slider">
        <div
          class="slider-trigger"
          style="
            width: 100%;
            display: flex;
            justify-content: center;
            height: 70px;
          "
        >
          <div class="slider-pill"></div>
        </div>

        <div @scroll="changeScroll" ref="navigationLinks" class="link-nav">
          <div
            @click="changeTab(i)"
            v-for="(link, i) in links"
            :key="i"
            class="nav-pills"
            :class="i == currentTab ? 'current' : ''"
          >
            {{ link }}
          </div>
        </div>
        <div class="container">
          <div>
            <div class="socials">
              <a
                href="https://stackoverflow.com/questions/73344760/defineexpose-from-components-script-setup-not-working-in-vue-3"
                target="_blank"
                style="display: flex; gap: 36px; flex: 1 1 0%"
              >
                <img
                  v-for="(link, i) in socialLinks"
                  style="height: 36px; width: 36px"
                  :key="i"
                  :src="link"
                  alt=""
                />
              </a>
            </div>
          </div>
          <div class="main-links">
            <img
              src="https://res.cloudinary.com/qtalk/image/upload/v1679603002/image_243_bbcqu8.png"
              alt=""
              class="cta-links"
            />
            <img
              src="https://res.cloudinary.com/qtalk/image/upload/v1679581042/ssup-landing/v2/blurs/JAM/Card_1_plnbmg.png"
              alt=""
              class="cta-links"
            />
            <img
              src="https://res.cloudinary.com/qtalk/image/upload/v1679602950/Property_1_Default_cgzw3k.png"
              alt=""
              class="cta-links"
            />
            <img
              src="https://res.cloudinary.com/qtalk/image/upload/v1679581042/ssup-landing/v2/blurs/JAM/Card_1_plnbmg.png"
              alt=""
              class="cta-links"
            />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, reactive, ref } from "vue";
import { defineExpose } from "vue";
import gsap from "gsap";
import { Draggable } from "gsap/Draggable";

const props = defineProps({
  currentTab: {
    type: Number,
    default: 0,
  },
});
const navigationLinks = ref(null);
const emit = defineEmits(["change-tab", "handle-modal"]);
const links = reactive(["All", "Youtube", "Amazon", "Spotify", "Newsletter"]);

const socialLinks = reactive([
  "https://res.cloudinary.com/qtalk/image/upload/v1680677989/image_290_puiets.png",
  "https://res.cloudinary.com/qtalk/image/upload/v1680677989/image_290_1_wmprco.png",
  "https://res.cloudinary.com/qtalk/image/upload/v1680677989/image_288_dey0iy.png",
  "https://res.cloudinary.com/qtalk/image/upload/v1680677989/Frame_427319881_jg2opn.png",
]);
const initDraggable = () => {
  gsap.registerPlugin(Draggable);
  Draggable.create(".slider", {
    trigger: ".slider-trigger",
    type: "y",
    bounds: ".slider-parent",
    onDragEnd: (val) => {
      if (val.offsetY > 0) {
        isModalOpen.value = false
        emit("change-tab", 0);
        emit("handle-modal", false);
        gsap.to(".slider-parent", { zIndex: -1, duration: 0.6 });
        gsap.to(".slider", { y: "100%", duration: 0.6 });
        // closeOverlay(false);
      } else {
        gsap.to(".slider", { y: 0, duration: 0.6 });
      }
    },
  });
};
const isModalOpen = ref(false)
function openModal() {
  isModalOpen.value = true
  emit("handle-modal", true);
  gsap.to(".slider-parent", { zIndex: 1, duration: 0.6 });
  gsap.to(".slider", { y: "0%", duration: 0.6 });
}
const changeTab = (i) => {
  emit("change-tab", i);
};
defineExpose({
  openModal,
  navigationLinks,
});
const changeScroll = (e) => {
  console.log(e);
};
const closeModal = () => {
  isModalOpen.value = false
  emit("change-tab", 0);
  emit("handle-modal", false);
  gsap.to(".slider-parent", { zIndex: -1, duration: 0.6 });
  gsap.to(".slider", { y: "100%", duration: 0.6 });
};
onMounted(() => {
  initDraggable();
});
</script>

<style scoped>
.layer-tp {
  position: fixed;
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
  z-index: 1;
}
.socials {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 12px 24px;
  gap: 8px;
  width: 312px;
  background: rgba(12, 12, 12, 0.21);
  border-radius: 16px;
}
</style>
