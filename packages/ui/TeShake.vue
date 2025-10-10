<template>
  <span ref="target" class="shake-root">
    <span
      v-for="(ch, i) in chars"
      :key="i"
      :class="{
        shake: isActive && !hover,
        'shake-hover': hover
      }"
      :style="{ animationDelay: `${-Math.random()}s` }"
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
    disabled?: boolean
    hover?: boolean
  }>(),
  {
    disabled: false,
    hover: false,
    lengthLimit: 144
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
.shake-root {
  display: inline-flex;
  white-space: pre-wrap;
}

.shake,
.shake-root:hover .shake-hover {
  animation: shake 0.5s linear infinite;
}

@keyframes shake {
  0% {
    transform: translate(1px, 1px) rotate(0deg);
  }
  10% {
    transform: translate(-1px, -2px) rotate(-1deg);
  }
  20% {
    transform: translate(-3px, 0px) rotate(1deg);
  }
  30% {
    transform: translate(3px, 2px) rotate(0deg);
  }
  40% {
    transform: translate(1px, -1px) rotate(1deg);
  }
  50% {
    transform: translate(-1px, 2px) rotate(-1deg);
  }
  60% {
    transform: translate(-3px, 1px) rotate(0deg);
  }
  70% {
    transform: translate(3px, 1px) rotate(-1deg);
  }
  80% {
    transform: translate(-1px, -1px) rotate(1deg);
  }
  90% {
    transform: translate(1px, 2px) rotate(0deg);
  }
  100% {
    transform: translate(1px, -2px) rotate(-1deg);
  }
}
</style>
