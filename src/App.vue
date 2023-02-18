<script setup lang="ts">
import {onMounted, ref} from "vue";

const fruits = [
  '1 apple', '2 banana', '3 orange', '4 grape', '5 melon', '6 strawberry',
  '7 peach', '8 lemon', '9 kiwi', '10 mango', '11 pineapple', '12 watermelon',
  '13 cherry', '14 plum', '15 pear', '16 apricot', '17 peach', '18 lemon',
  '19 kiwi', '20 mango', '21 pineapple', '22 watermelon', '23 cherry', '24 plum',
  '25 pear', '26 apricot'
]

const sortableList = ref<HTMLDivElement | null>(null)

const chosenItem = ref<HTMLElement | null>(null)
const chosenItemIndex = ref(0)

const getItemIndex = (target: Element) => {
  return Array.from(sortableList.value?.children ?? []).findIndex((item) => item === target)
}

const dragOver = (event: DragEvent) => {
  const el = event.currentTarget as HTMLElement

  if (chosenItem.value === el) {
    return
  }

  for (const anim of el?.getAnimations() ?? []) {
    if (anim.playState === 'running') {
      return;
    }
  }

  if (el) {
    console.log(`draggingIndex: ${chosenItemIndex.value}, getItemIndex: ${getItemIndex(el)}`);

    if (chosenItemIndex.value < getItemIndex(el)) {
      el.after(chosenItem.value!)
      el.style.animation = 'transform-up 150ms ease 0s'
      chosenItem.value!.style.animation = 'transform-down 150ms ease 0s'
    } else {
      el.before(chosenItem.value!)
      el.style.animation = 'transform-down 150ms ease 0s'
      chosenItem.value!.style.animation = 'transform-up 150ms ease 0s'
    }
    chosenItemIndex.value = getItemIndex(chosenItem.value!);
  }
}

const dragStart = (event: DragEvent) => {
  chosenItem.value = event.target as HTMLElement
  console.log(chosenItem.value.children[0].textContent)
}

onMounted(() => {
  sortableList.value!.addEventListener("dragover", (event) => {
    event.preventDefault();
    if (event.dataTransfer) event.dataTransfer.dropEffect = "move"
  });
})
</script>

<template>
  <div
      ref="sortableList"
      class="w-88 h-120 px-4 py-2 rounded-md bg-white shadow-xl overflow-scroll"
  >
    <div
        v-for="fruit in fruits"
        :key="fruit"
        class="my-2 p-2 rounded-md border-1 bg-gray-100 shadow-sm"
        style="transform: translate3d(0px, 0px, 0px);"
        draggable="true"
        @dragstart="dragStart($event)"
        @dragenter="dragOver($event)"
    >
      <p class="cursor-pointer">
        {{ fruit }}
      </p>
    </div>
  </div>
</template>

<style scoped>
</style>
