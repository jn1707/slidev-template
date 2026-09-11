<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'

const props = defineProps({
  count: { type: Number, required: true }
})

const currentIndex = ref(0)

const next = () => { if (currentIndex.value < props.count - 1) currentIndex.value++ }
const prev = () => { if (currentIndex.value > 0) currentIndex.value-- }

const onKey = (e) => {
  if (e.key === 'ArrowDown') { next(); e.stopPropagation() }
  else if (e.key === 'ArrowUp') { prev(); e.stopPropagation() }
}

// Each child occupies 1/count of the track height.
// Track height = count * 100% of container.
// translateY as % of track: -index * (1/count) * 100% = -index*(100/count)%
const trackHeight = computed(() => `${props.count * 100}%`)
const childSize = computed(() => `calc(100% / ${props.count})`)
const translateY = computed(() => `translateY(-${currentIndex.value * (100 / props.count)}%)`)

onMounted(() => window.addEventListener('keydown', onKey, true))
onUnmounted(() => window.removeEventListener('keydown', onKey, true))
</script>

<template>
  <div class="vs-outer">
    <div class="vs-track" :style="{ transform: translateY }">
      <slot />
    </div>
  </div>
</template>

<style scoped>
.vs-outer {
  width: 100%;
  height: 100%;
  overflow: hidden;
  position: relative;
}

.vs-track {
  display: flex;
  flex-direction: column;
  height: v-bind(trackHeight);
  /* Match Slidev's default slide transition feel */
  transition: transform 450ms cubic-bezier(0.4, 0, 0.2, 1);
}

/* Every direct child of the track fills exactly one "page" */
.vs-track > :deep(*) {
  flex: 0 0 v-bind(childSize);
  height: v-bind(childSize);
  min-height: 0;
  overflow: hidden;
  box-sizing: border-box;
  position: relative;
}
</style>
