<template>
  <section class="page">
    <div class="page__wrapper">
      <div class="page__column page__column--1">

        <div class="page__task">
          <h3>Задание:</h3>
          <custom-list class="page__list" :data="state.tasks1" tag="ol">
            <template #default="{item}">
              <p>{{item}}</p>
            </template>
          </custom-list>

          <h3>Компонент с деревом, указанный в п.3, должен включать в себя:</h3>
          <custom-list class="page__list" :data="state.tasks2" tag="ol">
            <template #default="{item}">
              <p>{{item}}</p>
            </template>
          </custom-list>
        </div>

        <teleport v-if="state.mounted" to="#modal">
          <folder-explorer
              :data="folder"
              @select="handleExplorerSelect"
              @cancel="handleExplorerCancel"
          />
        </teleport>
      </div>

      <div class="page__column page__column--2">
        <button class="page__button" @click="handleOpenClick">Открыть</button>
      </div>
    </div>

    <modal-box
        class="page__modal"
        id="modal"
        :open="state.modal.open"
        :header="state.modal.header"
        @close="handleModalClose"
    />

  </section>
</template>

<script setup lang="ts">

import type {TExplorer} from '@/components/FolderExplorer.vue'
import Explorer from '@/components/FolderExplorer.vue'
import ModalBox from '@/components/ModalBox.vue'
import CustomList from '@/components/CustomList.vue'
import {reactive} from '@vue/runtime-core'
import {onMounted} from 'vue'
import FolderExplorer from '@/components/FolderExplorer.vue';

const state = reactive({
  test: true,
  modal: {
    open: false,
    header: 'Выберите папку',
  },
  tasks1: [
    'Создать новый пустой Vue-проект.',
    'В базовом Компоненте добавить на страницу кнопку «Открыть».',
    'По нажатию на эту кнопку должен запускаться Компонент, формирующий модальное окно с деревом Папок для выбора.',
    'Данные для наполнения дерева Папок можно брать из предопределённого в JavaScript массива объектов.',
  ],
  tasks2: [
    'Текст Заголовка модального окна, который можно передавать из внешнего Vue-кода в Компонент.',
    'Содержать в себе дерево Папок, иерархию которых можно раскрывать и закрывать (уровень вложенности папок не должен быть ограничен).',
    'Кнопку «Ок», по нажатию на которую модальное окно будет закрываться, но перед этим формироваться событие «select», в которое должен передаваться идентификатор выбранной в дереве папки.',
    'Кнопку «Закрыть», по нажатию на которую модальное окно будет закрываться.',
  ],
  mounted: false,
})

onMounted(() => {
  state.mounted = true
})

const handleExplorerSelect = (data: any) => {
  state.modal.open = false
  console.log(data)
}
const handleExplorerCancel = () => {
  state.modal.open = false
}

const createFolder = (num: number) => {
  createFolder.counter++
  const result: TExplorer = new Array(num).fill(null).map(() => ({
    id: createFolder.counter,
    name: `Folder - ${createFolder.counter}`,
    children: createFolder(num - 1),
  }))

  return result
}
createFolder.counter = 0

const folder = reactive<TExplorer>(createFolder(6))
// const folder = reactive<TExplorer>([])

const handleModalClose = () => {
  state.modal.open = false
}

const handleOpenClick = () => {
  state.modal.open = !state.modal.open
}

</script>

<style scoped>
.page {
  min-height: calc(100vh - var(--header-height));
}

.page__wrapper {
  display: grid;
  grid-template-columns: 1fr 1fr;
  justify-items: center;
  align-items: center;
  min-height: calc(100vh - var(--header-height));
}

.page__column {
  display: flex;
  align-items: center;
  justify-content: center;
}

.page__column--1 {
  width: 50%;
}

.page__column--2 {
  width: 50%;
}

.page__button {
  padding: .3em .6em .4em .6em;
  font-size: 20px;
  background-color: var(--vt-c-indigo);
  color: #ffffff;
  text-transform: uppercase;
  border-radius: 4px;
  border: none;
}

.page__task {
  border: 1px solid var(--vt-c-divider-dark-2);
  border-radius: .5em;
  padding: 1em;
  font-size: 10px;
  width: 400px;
}

.page__list {
  list-style: decimal;
  padding-left: 20px;
}
.page__list ::v-deep(li){
  display:list-item;
  padding-left: 1em;
}

.page__task h3 {
  line-height: 1.2em;
  margin: 10px 0;
  font-size: 14px;
  font-weight: 500;
}
</style>