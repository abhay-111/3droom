<template>
  <div>
    <div class="nav">
      <div class="profile">
        <img
          src="https://res.cloudinary.com/qtalk/image/upload/v1679578980/ssup-landing/v2/blurs/JAM/image_103_ek4zex.png"
          alt="" />
        <p>Varun Duggirala</p>
      </div>
      <img style="height: 24px; width: 24px" @click="openModal"
        src="https://res.cloudinary.com/qtalk/image/upload/v1679578987/ssup-landing/v2/blurs/JAM/Frame_427319799_na3u4t.png"
        alt="" />
    </div>
    <div v-if="isModalOpen" @click.stop="closeModal" class="layer-tp"></div>
    <div class="slider-parent" style="
              z-index: -1;
              height: 160%;
              width: 100%;
              position: absolute;
              bottom: 0px;
              transform: translateY(50%);
            ">
      <div class="slider">
        <div class="slider-trigger" style="
                  width: 100%;
                  display: flex;
                  justify-content: center;
                  height: 70px;
                ">
          <div class="slider-pill"></div>
        </div>

        <div @scroll="changeScroll" ref="navigationLinks" class="link-nav">
          <div @click="changeTab(i)" v-for="(link, i) in links" :key="i" class="nav-pills" :style="i===currentTab ? {color:'black'} :{color:'#999597'}">
            {{ link }}
          </div>
          <div class="label" ref="label"></div>
        </div>
        <div class="container">
          <div>
            <div class="socials">
              <a href="https://stackoverflow.com/questions/73344760/defineexpose-from-components-script-setup-not-working-in-vue-3"
                target="_blank" style="display: flex; gap: 36px; flex: 1 1 0%">
                <img v-for="(link, i) in socialLinks" style="height: 36px; width: 36px" :key="i" :src="link" alt="" />
              </a>
            </div>
          </div>
          <Transition name="fade">
            <LinksTabs :links="getTabLinks" />
          </Transition>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed, nextTick, onMounted, reactive, ref,Transition, watch } from "vue";
import { defineExpose } from "vue";
import LinksTabs from "./LinksTabs.vue";
import gsap from "gsap";
import { Draggable } from "gsap/Draggable";

const props = defineProps({
  currentTab: {
    type: Number,
    default: 0,
  },
});
const navigationLinks = ref(null);
const label = ref()
const emit = defineEmits(["change-tab", "handle-modal"]);
const links = reactive(["All", "Youtube", "Amazon", "Spotify", "Newsletter"]);
const tabLinks = ref({
  all: [
    'https://res.cloudinary.com/qtalk/image/upload/v1679603002/image_243_bbcqu8.png',
    'https://res.cloudinary.com/qtalk/image/upload/v1679581042/ssup-landing/v2/blurs/JAM/Card_1_plnbmg.png',
    'https://res.cloudinary.com/qtalk/image/upload/v1679602950/Property_1_Default_cgzw3k.png',
    'https://res.cloudinary.com/qtalk/image/upload/v1679581042/ssup-landing/v2/blurs/JAM/Card_1_plnbmg.png'
  ],
  youtube: [
    'https://res.cloudinary.com/qtalk/image/upload/v1681107080/superbio-3d/youtubenew_2_ow9xpd.png',
    'https://res.cloudinary.com/qtalk/image/upload/v1681107080/superbio-3d/youtubenew_3_ldcb3p.png'
  ],
  spotify: [
    'https://res.cloudinary.com/qtalk/image/upload/v1681107121/superbio-3d/spotifynew_1_1_nne4te.png',
    'https://res.cloudinary.com/qtalk/image/upload/v1681107121/superbio-3d/spotifynew_1_ww9j9p.png'
  ],
  amazon: [
    'https://res.cloudinary.com/qtalk/image/upload/v1681107081/superbio-3d/%D0%BC%D0%BE%D0%BA%D0%B0%D0%BF1_1_pfabcr.png'
  ],
  newsletter: [
    'https://res.cloudinary.com/qtalk/image/upload/v1681107121/superbio-3d/spotifynew_1_1_nne4te.png',
    'https://res.cloudinary.com/qtalk/image/upload/v1681107121/superbio-3d/spotifynew_1_ww9j9p.png'
  ],

})
const socialLinks = reactive([
  "https://res.cloudinary.com/qtalk/image/upload/v1681109526/superbio-3d/icons/Group_427319365_bz1mc7.svg",
  "https://res.cloudinary.com/qtalk/image/upload/v1681109526/superbio-3d/icons/Group_427319366_r53hcn.svg",
  "https://res.cloudinary.com/qtalk/image/upload/v1681109526/superbio-3d/icons/Group_427319368_h1phe8.svg",
  "https://res.cloudinary.com/qtalk/image/upload/v1681109527/superbio-3d/icons/Group_427319369_ntpd1f.svg",
]);

let tl
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
  nextTick(()=>{
    gsap.to(".slider-parent", { zIndex: 1, duration: 0.6 });
  gsap.to(".slider", { y: "0%", duration: 0.6 });
  })
}
const changeTab = (i) => {
  // change tab 
  if (!tl.isActive()) {
    const tabsBlock = document.querySelectorAll('.nav-pills');
    const parent = document.querySelector('.link-nav')
    const currentTab = tabsBlock[i]
    currentTab.style.color='black'
    const elWidth = currentTab.offsetWidth;
    const containerLeft = parent.getBoundingClientRect().left;
    const offsetLeft = currentTab.getBoundingClientRect().left - containerLeft + 100;
    gsap.to(label.value, { duration: 0.1, left: offsetLeft, width: elWidth });
  }
  emit("change-tab", i);
};
watch(props,()=>{
   const tabsBlock = document.querySelectorAll('.nav-pills');
    const parent = document.querySelector('.link-nav')
    const currentTab = tabsBlock[props.currentTab]
    console.log(currentTab)
    currentTab.style.color='black'
    const elWidth = currentTab.offsetWidth;
    console.log(parent.scrollLeft)
    const containerLeft = parent.getBoundingClientRect().left;
    console.log(currentTab.getBoundingClientRect().left,currentTab.getBoundingClientRect())
    const offsetLeft = currentTab.getBoundingClientRect().left - containerLeft + parent.scrollLeft;
    gsap.to(label.value, { duration: 0.1, left: offsetLeft, width: elWidth });
})
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

//
const sliderInit = () => {
  const navPills = document.querySelectorAll('.nav-pills');
  const startElement = navPills[0]
  const innerWidth = startElement.offsetWidth
  gsap.set(label.value, { width: innerWidth });
}
onMounted(() => {
  tl = gsap.timeline()
  sliderInit()
  initDraggable();
});

const getTabLinks = computed(() => {
  switch (props.currentTab) {
    case 0:  //all
      return [...tabLinks.value['youtube'], ...tabLinks.value['amazon'], ...tabLinks.value['spotify']]
    case 1:
      return tabLinks.value['youtube']
    case 2:
      return tabLinks.value['amazon']
    case 3:
      return tabLinks.value['spotify']
      case 3:
      return tabLinks.value['newsletter']
    default:
      return [...tabLinks.value['youtube'], ...tabLinks.value['amazon'], ...tabLinks.value['spotify']]
  }
})
// #999597
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

.label {
  position: absolute;
  left: 0;
  top: 6px;
  background: #ffffff;
  height: 24px;
  transition: 200ms;
  border-radius: 8px;
  z-index: -1;
}

.link-nav {
  position: relative;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 2s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
