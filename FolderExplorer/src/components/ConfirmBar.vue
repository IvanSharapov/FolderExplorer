<template>
  <div class="confirm-bar">
    <button
      class="confirm-bar__button confirm-bar__button--confirm"
      :disabled="props.disabled[0]"
      @click="handleConfirmClick"
    >
      {{ props.labels[0] }}
    </button>
    <button
      class="confirm-bar__button confirm-bar__button--cancel"
      :disabled="props.disabled[1]"
      @click="handleCancelClick"
    >
      {{ props.labels[1] }}
    </button>
  </div>
</template>

<script setup lang="ts">

const emit = defineEmits<{
  (e: 'confirm'): void,
  (e: 'cancel'): void,
}>()

const props = withDefaults(defineProps<{
  labels?: [string, string]
  disabled?: [boolean, boolean]
}>(), {
  labels: () => ['OK', 'Закрыть'],
  disabled: () => [false, false]
})

const handleConfirmClick = () => emit('confirm')
const handleCancelClick = () => emit('cancel')

</script>

<style scoped>
.confirm-bar {
  box-sizing: border-box;
  display: flex;
  justify-content: flex-end;
  align-items: center;
  width: 100%;
  padding: 1em;
  border-bottom-left-radius: 4px;
  border-bottom-right-radius: 4px;
}

.confirm-bar__button {
  display: flex;
  align-items: center;
  border: none;
  background: var(--color-background);
  padding: .3em .6em .4em .6em;
  border-radius: 2px;
  color: var(--vt-c-text-light-1);
  text-transform: uppercase;
  font-weight: 600;
  transition: color .3s, background-color .3s;
}

.confirm-bar__button:disabled {
  opacity: .5;
}

.confirm-bar__button:not(:disabled):hover {
  background: var(--color-background-mute);
  color: var(--vt-c-black-soft);
}

.confirm-bar__button:not(:disabled):active {
  background: var(--vt-c-divider-dark-1);
  color: var(--color-background-mute);
}

.confirm-bar__button--confirm {

}

.confirm-bar__button--cancel {
  margin-left: 1em;
}
</style>