<template>
  <div role="directory" class="file-explorer">
    <template v-if="data.length">
      <div class="file-explorer__selected-item">
        <img src="/src/assets/img/folder.png" alt="Folder" width="16" height="16"/>
        <span v-if="store.selected">{{ store.selected.path }}</span>
      </div>
      <custom-list class="file-explorer__list" :data="props.data">
        <template #default="{item, index}">
          <label>
          <span v-if="hasChildren(item)">
            <expand-button :expanded="meta[index].expanded"/>
          </span>
            <img src="/src/assets/img/folder.png" alt="Folder" width="16" height="16"/>
            <input
                v-if="hasChildren(item)"
                class="hidden"
                v-model="meta[index].expanded"
                type="checkbox"
            >
          </label>
          <span
              :class="{
              'file-explorer__item': true,
              'file-explorer__item--selected': meta[index].item.id === store.selected?.item.id
            }"
              @click="handleItemClick(meta[index])"
          >
          {{ item.name }}
        </span>
          <folder-explorer
              class="file-explorer--sub"
              v-if="item.children && meta[index].expanded"
              :data="item.children"
              :renderActions="false"
              :parent="meta[index].path"
              :storeID="props.storeID"
              @pick="handleItemClick"
          />
        </template>
      </custom-list>
      <confirm-bar
          class="file-explorer__actions"
          v-if="props.renderActions"
          :disabled="[!store.selected, false]"
          @confirm="handleConfirmClick"
          @cancel="handleCancelClick"
      />
    </template>
    <template v-else>
      <no-data/>
    </template>
  </div>
</template>

<script setup lang="ts">
import {v4 as uuid} from 'uuid'
import CustomList from '@/components/CustomList.vue'
import ConfirmBar from '@/components/ConfirmBar.vue'
import ExpandButton from '@/components/ExpandButton.vue'
import NoData from '@/components/NoData.vue'
import {defineStore} from 'pinia'
import {reactive} from '@vue/runtime-core'

export type TExplorerNode = {
  id: number,
  name: string,
  children: Array<TExplorerNode>,
}

export type TExplorerMeta = {
  item: TExplorerNode;
  expanded: boolean;
  path: string;
}

export type TExplorer = Array<TExplorerNode>

export interface IExplorerProps {
  data: TExplorer,
  renderActions?: boolean,
  parent?: string,
  storeID?: string,
}

const emit = defineEmits<{
  (e: 'select', value: { item: TExplorerNode, path: string }): void,
  (e: 'pick', value: any): void,
  (e: 'cancel'): void,
}>()

const props = withDefaults(defineProps<IExplorerProps>(), {
  renderActions: true,
  parent: '/',
  storeID: uuid(),
})

const meta = reactive(props.data.map((item) => ({
  item,
  expanded: props.renderActions,
  path: `${props.parent}${item.name}/`, //(${item.id})
})))

type TExplorerState = { selected: TExplorerMeta | null }
type TExplorerActions = {
  setItem(item: any): void
}

const useStore = defineStore<`FileExplorer-${string}`, TExplorerState, {}, TExplorerActions>(`FileExplorer-${props.storeID}`, {
  state: () => ({selected: null}),
  actions: {
    setItem(item: any) {
      this.selected = item
    },
  },
})

const store = useStore()

const handleItemClick = (data: TExplorerMeta) => {
  emit('pick', data)
  store.setItem(data)
}

const handleConfirmClick = () => {
  if (store.selected) {
    emit('select', {item: store.selected.item, path: store.selected.path})
  }
}
const handleCancelClick = () => emit('cancel')

const hasChildren = (item: TExplorerNode) => item?.children?.length

</script>

<style scoped>
.file-explorer {
  position: relative;
  min-width: 300px;
}

.file-explorer:not(.file-explorer--sub) {
  height: 420px;
  margin-bottom: 0;
  margin-top: 30px;
}

.file-explorer--sub {
  margin-top: 6px;
}

.file-explorer:not(.file-explorer--sub) > .file-explorer__list {
  min-height: 400px;
  max-height: 420px;
  overflow-y: auto;
}

.file-explorer__selected-item {
  position: absolute;
  top: -30px;
  left: 0;
  right: 0;
  z-index: 1;
  display: flex;
  justify-content: flex-start;
  align-items: center;
  height: 25px;
  padding: .2em 2em;
  border-bottom: 1px solid var(--vt-c-divider-dark-2);
  background-color: var(--vt-c-text-dark-2);
  font-size: 12px;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}

.file-explorer__selected-item > img {
  margin-right: 10px;
}

.file-explorer__selected-item span {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;

}

.file-explorer--sub > .file-explorer__selected-item {
  display: none;
}

.file-explorer:not(.file-explorer--sub) > .file-explorer__list > ::v-deep(li:last-child) {
  margin-bottom: 10px;
}

.file-explorer:not(.file-explorer--sub) > .file-explorer__list > ::v-deep(li:first-child) {
  margin-top: 10px;
}

.file-explorer__list {
  margin-left: 30px;
}

label {
  display: inline-flex;
  justify-content: flex-end;
  align-items: center;
  width: 48px;
  cursor: pointer;
}

label > img {
  margin: 0 .5em;
}

input {
  visibility: hidden;
  position: absolute;
}

label > div {
  display: inline-block;
  padding: 0 .2em;
}

.file-explorer__actions {
  position: absolute;
  bottom: -59px;
  background: var(--vt-c-indigo);
}

.file-explorer__item {
  padding: .2em;
  border-radius: 2px;
  transition: background-color .1s, color .1s;
  /*margin-top: 6px;*/
}

.file-explorer__list > ::v-deep(li) {
  min-height: 24px;
  cursor: pointer;
}

.file-explorer__item--selected {
  background: var(--vt-c-indigo);
  color: var(--vt-c-white);
}

.hidden {
  visibility: hidden;
}

</style>