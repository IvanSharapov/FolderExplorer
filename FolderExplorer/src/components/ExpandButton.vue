<template>
  <div role="button" :class="classes" @click.shift />
</template>

<script setup lang="ts">
import {computed} from '@vue/runtime-core'

const props = defineProps<{expanded: boolean}>()

const classes = computed(() => ({
  'expand-button': true,
  'expand-button__inverted': props.expanded
}))

</script>

<style scoped>

.expand-button {
  box-sizing: border-box;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 16px;
  height: 8px;
  background: #ffffff;
  border: 2px solid #333333;
  border-radius: .4em;
  transition: transform .3s;
  background: var(--vt-c-indigo);
  opacity: .9;
  clip-path: polygon(100% 0, 50% 100%, 0 0)
}

.expand-button__inverted {
  transform: rotate(180deg);
}

</style>