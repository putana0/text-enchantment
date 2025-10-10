<script setup lang="ts">
import { TeJump, TeShake, TeMagic } from '../packages'
</script>

<template>
  <div class="container mx-auto px-2">
    <h1 class="uppercase">text effects</h1>
  </div>

  <div class="container mx-auto px-2 mb-3">
    <h2>Class</h2>
    <div class="inline-flex flex-col mb-3">
      <span class="te-gold">te-gold</span>
      <span class="te-gold-bg">te-gold-bg</span>
      <span class="te-silver">te-silver</span>
      <span class="te-silver-bg">te-silver-bg</span>
      <span class="te-bronze">te-bronze</span>
      <span class="te-bronze-bg">te-bronze-bg</span>
      <span class="te-rainbow">te-rainbow</span>
      <span class="te-rainbow-bg">te-rainbow-bg</span>
      <span class="te-glitch">te-glitch</span>
      <span class="te-blink">te-blink</span>
    </div>
  </div>

  <div class="container mx-auto px-2 mb-3">
    <h2>Components</h2>
    <div class="inline-flex flex-col mb-3">
      <TeJump text="te-jump" />
      <TeShake text="te-shake" />
      <TeMagic>te-magic</TeMagic>
    </div>
  </div>
</template>
