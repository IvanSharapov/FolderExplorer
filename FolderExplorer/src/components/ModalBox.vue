<template>
  <dialog class="modal-box" :open="props.open">
    <header class="modal-box__header" v-if="props.header">
      <h3>{{ props.header }}</h3>
    </header>
    <div class="modal-box__content" :id="props.id"/>
    <slot/>
  </dialog>
</template>

<script setup lang="ts">

import {onBeforeUnmount, onMounted} from 'vue'

interface IModalBoxProps {
  open: boolean,
  header?: string,
  id?: string
}

const emit = defineEmits<{
  (e: 'close'): void
}>()

const props = withDefaults(defineProps<IModalBoxProps>(), {open: false})

const handleEscapeDown = ({code}: KeyboardEvent) => {
  code === 'Escape' && props.open && emit('close')
}

onMounted(() => {
  document.addEventListener('keydown', handleEscapeDown)
})

onBeforeUnmount(() => {
  document.removeEventListener('keydown', handleEscapeDown)
})

</script>

<style scoped>
.modal-box {
  position: absolute;
  border: none;
  box-shadow: 0 0 20px 5px #999999;
  border-radius: 4px;
  min-width: 400px;
  max-width: 500px;
  min-height: 560px;
  overflow-y: auto;
  margin: 0 auto;
  padding: 0;
  top: 100px;
  left: 0;
  right: 0;
}

.modal-box__content {

}

.modal-box__header {
  position: sticky;
  top: 0;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1;
  text-transform: uppercase;
  background: var(--vt-c-indigo);
  height: 3.4em;
}

.modal-box__header h3 {
  margin: 0;
  padding: .5em;
  font-weight: 500;
  letter-spacing: .1em;
  color: #ffffff;
}
</style>