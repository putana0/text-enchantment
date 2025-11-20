<template>
  <span ref="root" class="te-magic" style="position: relative">
    <slot />
  </span>
</template>

<script setup lang="ts">
import { onMounted, ref, onBeforeUnmount } from 'vue'

const root = ref<HTMLElement | null>(null)
let io: IntersectionObserver | null = null
let timers: number[] = []

function rand(min: number, max: number) {
  return Math.floor(Math.random() * (max - min + 1)) + min
}

function animateStar(el: HTMLElement) {
  el.style.setProperty('--star-left', `${rand(0, 100)}%`)
  el.style.setProperty('--star-top', `${rand(-10, 90)}%`)
  el.style.animation = 'none'
  // force reflow
  void el.offsetHeight
  el.style.animation = ''
}

function compile(el: HTMLElement) {
  if (el.dataset.compiled) return
  el.dataset.compiled = '1'

  // prepend 3 stars
  for (let i = 0; i < 3; i++) {
    const span = document.createElement('span')
    span.className = 'magic-star'
    span.innerHTML =
      "<svg viewBox='0 0 512 512'><path d='M512 255.1c0 11.34-7.406 20.86-18.44 23.64l-171.3 42.78l-42.78 171.1C276.7 504.6 267.2 512 255.9 512s-20.84-7.406-23.62-18.44l-42.66-171.2L18.47 279.6C7.406 276.8 0 267.3 0 255.1c0-11.34 7.406-20.83 18.44-23.61l171.2-42.78l42.78-171.1C235.2 7.406 244.7 0 256 0s20.84 7.406 23.62 18.44l42.78 171.2l171.2 42.78C504.6 235.2 512 244.6 512 255.1z'></path></svg>"
    el.prepend(span)
  }

  const stars = Array.from(el.querySelectorAll<HTMLElement>('.magic-star'))
  let index = 0
  for (const star of stars) {
    const t = window.setTimeout(() => {
      animateStar(star)
      const id = window.setInterval(() => animateStar(star), 1500)
      timers.push(id)
    }, 500 * index++)
    timers.push(t)
  }
}

onMounted(() => {
  if (!root.value) return
  io = new IntersectionObserver(
    (entries) => {
      for (const e of entries) {
        if (e.isIntersecting && e.target instanceof HTMLElement) {
          compile(e.target)
        }
      }
    },
    { threshold: 0 }
  )
  io.observe(root.value)
})

onBeforeUnmount(() => {
  if (io && root.value) io.unobserve(root.value)
  timers.forEach((id) => clearInterval(id))
  timers = []
})
</script>

<style>
@keyframes magic-scale {
  from,
  to {
    transform: scale(0);
  }
  50% {
    transform: scale(1);
  }
}
@keyframes magic-rotate {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(180deg);
  }
}

.te-magic > .magic-star {
  fill: rgb(123, 31, 162);
  --size: clamp(0.4em, 0.6em, 0.8em);
  animation: magic-scale 0.7s ease forwards;
  display: inline-block;
  position: absolute;
  left: var(--star-left);
  top: var(--star-top);
  height: var(--size);
  width: var(--size);
}
.te-magic > .magic-star > svg {
  animation: magic-rotate 1s linear infinite;
  display: block;
  opacity: 0.7;
}
</style>
