<template>
  <component :is="props.tag" class="custom-list">
    <li
        class="asd"
        v-for="(item, index) in props.data"
        :key="index"
    >
      <slot :item="item" :index="index" />
    </li>
  </component>
</template>

<script setup lang="ts">

const props = withDefaults(defineProps<{
  data: Array<unknown>,
  tag?: 'ul' | 'ol',
}>(), {
  tag: 'ul',
})

</script>

<style scoped>
.custom-list {
  list-style: none;
  padding: 0;
  margin: 0 0 0 .4em;
}
</style>