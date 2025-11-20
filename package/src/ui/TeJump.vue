<template>
  <span ref="target" class="jump-root">
    <span
      v-for="(ch, i) in chars"
      :key="i"
      :class="{
        jump: isActive && !hover,
        'jump-hover': hover
      }"
      :style="{
        animationDelay: `${(i + 1) * step}s`
      }"
    >
      {{ ch }}
    </span>
  </span>
</template>

<script setup lang="ts">
import { useIntersectionObserver } from '@vueuse/core'
import { computed, onBeforeUnmount, shallowRef, useTemplateRef } from 'vue'

const props = withDefaults(
  defineProps<{
    text: string
    step?: number
    disabled?: boolean
    hover?: boolean
  }>(),
  {
    step: 0.05,
    disabled: false,
    hover: false
  }
)

const target = useTemplateRef('target')
const targetIsVisible = shallowRef(false)

const isActive = computed(() => targetIsVisible.value && !props.disabled)

const chars = computed(() => Array.from(props.text ?? ''))

const { stop } = useIntersectionObserver(target, ([entry]) => {
  targetIsVisible.value = entry?.isIntersecting || false
})

onBeforeUnmount(() => {
  stop()
})
</script>

<style scoped>
.jump-root {
  display: inline-flex;
  white-space: pre-wrap;
}

.jump,
.jump-root:hover .jump-hover {
  animation: jump 0.5s ease-in-out infinite;
}

@keyframes jump {
  0% {
    transform: translateY(4px);
  }
  50% {
    transform: translateY(-8px);
  }
  100% {
    transform: translateY(4px);
  }
}
</style>
