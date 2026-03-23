<script setup lang="ts">
/// <reference types="vite/client" />
import type { CSSProperties } from 'vue'
import { computed } from 'vue'

const props = defineProps({
  image: {
    type: String,
  },
  class: {
    type: String,
  },
  backgroundSize: {
    type: String,
    default: 'cover',
  },
})

function resolveAssetUrl(url: string) {
  if (url.startsWith('/'))
    return import.meta.env.BASE_URL + url.slice(1)
  return url
}

const style = computed<CSSProperties>(() => ({
  backgroundImage: props.image ? `url("${resolveAssetUrl(props.image)}")` : undefined,
  backgroundRepeat: 'no-repeat',
  backgroundPosition: 'center',
  backgroundSize: props.backgroundSize,
}))
</script>

<template>
  <div class="grid w-full h-full auto-rows-fr grid-cols-[minmax(0,12fr)_minmax(0,6fr)]">
    <div class="slidev-layout default" :class="props.class">
      <slot />
    </div>
    <div class="w-full h-full" :style="style" />
  </div>
</template>
