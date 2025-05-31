<template>
  <div class="mouse-wrapper">
    <img
      :src="mouseSrc"
      :width="mouseWidth"
      :height="mouseHeight"
      :style="mouseStyle"
      @click="handleMouseClick"
      style="pointer-events: auto"
      v-if="isVisible"
    />
    <!-- <Fireworks
      v-if="showCelebration"
      :fireworksStyle="fireworksStyle"
      :isVisible="showCelebration"
    /> -->
    <div v-if="showWinText" class="win-text" :style="winTextStyle">
      You Win!
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from "vue";
//import { Fireworks } from "@/components/firework";
const mousePoses = ref([
  {
    src: "/img/mouse_peek.png",
    width: 50,
    height: 50,
    offsetX: -10,
    offsetY: -10,
  },
  {
    src: "/img/mouse_half.png",
    width: 70,
    height: 70,
    offsetX: -20,
    offsetY: -20,
  },
  {
    src: "/img/mouse_corner.png",
    width: 40,
    height: 40,
    offsetX: -2,
    offsetY: -2,
  },
  // ... другие позы
]);

const isVisible = ref(false);
const mouseStyle = ref({
  position: "fixed", // absolute
  top: "0",
  left: "0",
  zIndex: "1000",
  cursor: "pointer",
});

const currentPoseIndex = ref(0);

const mouseSrc = computed(() => mousePoses.value[currentPoseIndex.value].src);
const mouseWidth = computed(
  () => mousePoses.value[currentPoseIndex.value].width
);
const mouseHeight = computed(
  () => mousePoses.value[currentPoseIndex.value].height
);
const mouseOffsetX = computed(
  () => mousePoses.value[currentPoseIndex.value].offsetX
);
const mouseOffsetY = computed(
  () => mousePoses.value[currentPoseIndex.value].offsetY
);

let timeoutId;
let mainElement;
const showCelebration = ref(false);
const showWinText = ref(false);
const fireworksStyle = ref({ top: "0px", left: "0px" });
const lastMouseX = ref(0);
const lastMouseY = ref(0);

const showMouse = () => {
  const mainElement = document.querySelector(".main");
  if (!mainElement) return;
  const mainRect = mainElement.getBoundingClientRect();
  const maxX = mainRect.width - mouseWidth.value;
  const maxY = mainRect.height - mouseHeight.value;

  currentPoseIndex.value = Math.floor(Math.random() * mousePoses.value.length);
  const randomX = Math.random() * maxX + mouseOffsetX.value;
  const randomY = Math.random() * maxY + mouseOffsetY.value;

  mouseStyle.value.top = `${randomY}px`;
  mouseStyle.value.left = `${randomX}px`;
  lastMouseX.value = randomX;
  lastMouseY.value = randomY;

  isVisible.value = true;

  timeoutId = setTimeout(() => {
    isVisible.value = false;
  }, 2000);
};

const handleMouseClick = () => {
  isVisible.value = false;
  clearTimeout(timeoutId);
  showCelebration.value = true;
  showWinText.value = true;
  fireworksStyle.value = {
    top: lastMouseY.value + "px",
    left: lastMouseX.value + "px",
    transform: "translateX(-50%)",
    width: "200px",
    height: "200px",
    position: "absolute",
    zIndex: "1001",
  };

  setTimeout(() => {
    showCelebration.value = false;
    showWinText.value = false;
  }, 1500);
};

const winTextStyle = computed(() => ({
  position: "absolute",
  top: lastMouseY.value - mouseHeight.value / 2 + "px",
  left: lastMouseX.value + "px",
  transform: "translateX(-50%)",
  fontSize: "2em",
  opacity: 1,
  animation: "fadeOut 1.5s forwards",
  zIndex: "1001",
  pointerEvents: "none",
}));

onMounted(() => {
  mainElement = document.querySelector(".main");
  setInterval(showMouse, 3000); // Появляется каждые 5 секунд
});

onUnmounted(() => {
  clearInterval(timeoutId);
});
</script>
<style scoped>
.mouse-wrapper {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
}
img {
  transition: all 0.3s ease;
}
.celebration {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  display: flex;
  justify-content: center;
  align-items: center;
}

.fireworks {
  /* Fireworks animation (you'll need to find or create this) */
  /* This is just a placeholder */
  width: 100px;
  height: 100px;
  background-color: red;
  animation: fireworks 1s ease-out infinite;
}

.stars {
  /* Stars animation (you'll need to find or create this) */
  /* This is just a placeholder */
  width: 50px;
  height: 50px;
  background-color: yellow;
  animation: stars 1s ease-out infinite;
}

.win-text {
  position: absolute;
  font-size: 2em;
  color: #c37d4a;
  opacity: 1;
  animation: fadeOut 1.5s forwards;
  z-index: 1003; /* Над мышкой и салютом */
  pointer-events: none;
}

@keyframes fireworks {
  0% {
    transform: translateY(0) scale(0);
    opacity: 1;
  }
  100% {
    transform: translateY(-100px) scale(1);
    opacity: 0;
  }
}

@keyframes stars {
  0% {
    transform: translateY(0) scale(0);
    opacity: 1;
  }
  100% {
    transform: translateY(-100px) scale(1);
    opacity: 0;
  }
}

@keyframes fadeOut {
  0% {
    opacity: 1;
  }
  100% {
    opacity: 0;
  }
}
</style>
