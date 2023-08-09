<script setup lang="ts">
import CardItem from '~/components/CardItem.vue'
import { createVNode } from 'vue'

defineOptions({
  name: 'IndexPage',
})

const componentsArr = ref<any>([])
const thisItem = ref<number>(4)
const maxLength = 10

for (let i = 1; i <= maxLength; i++) {
  const component = markRaw(defineComponent(
    () => {
      return () => {
        return createVNode(CardItem, { context: `<h1 class="card-item">${i}</h1>`, id: i }, null)
      }
    },
  ))
  componentsArr.value.push({ component, id: i })
}

function getItemStyle(index: number): any {
  if (index === thisItem.value)
    return { top: 0, zIndex: 200, background: '#121212' }
  else if (index <= thisItem.value)
    return { top: '-50px', zIndex: 100 + index, transform: 'scale(0.8)' }
  else
    return { top: '50px', zIndex: 100 - index, transform: 'scale(0.8)' }
}

function handleClickItem(e: MouseEvent) {
  const el = e.target as EventTarget
  if (isHTMLElement(el)) {
    const elId = Number(el.dataset.id)
    // elId必须有效
    if (!elId || elId !== thisItem.value) {
      if (elId < thisItem.value)
        thisItem.value--
      else
        thisItem.value++
      return
    }
    if (elId === maxLength || elId === 1)
      return
    const mouseY = e.clientY
    const rect = el.getBoundingClientRect()
    const middle = rect.top + (rect.height / 2)
    if (mouseY < middle)
      thisItem.value--
    else
      thisItem.value++
  }
}

function isHTMLElement(target: EventTarget): target is HTMLElement {
  return (target as HTMLElement).dataset !== undefined
}
</script>

<template>
  <div m-10 flex items-center justify-center>
    <div relative h-80 w-50 flex flex-col justify-center>
      <template v-for="(item) in componentsArr" :key="item.id">
        <component :is="item.component" :data-index="item.id" absolute :style="getItemStyle(item.id)"
          @click="(e: MouseEvent) => handleClickItem(e)" />
      </template>
    </div>
  </div>
</template>

<route lang="yaml">
meta:
  layout: home
</route>
