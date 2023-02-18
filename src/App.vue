<script setup lang="ts">
import {onMounted, ref} from "vue";

const fruits = [
  '1 apple', '2 banana', '3 orange', '4 grape', '5 melon', '6 strawberry',
  '7 peach', '8 lemon', '9 kiwi', '10 mango', '11 pineapple', '12 watermelon',
  '13 cherry', '14 plum', '15 pear', '16 apricot', '17 peach', '18 lemon',
  '19 kiwi', '20 mango', '21 pineapple', '22 watermelon', '23 cherry', '24 plum',
  '25 pear', '26 apricot'
]

const draggingId = ref('')
const draggingIndex = ref(0)

const getItemIndex = (id: string) => {
  return Array.from(document.getElementById("sortable-list")!.children).findIndex((item) => item.id === id)
}

const dragOver = (id: string) => {
  if (draggingId.value === id) {
    return
  }

  const el = document.getElementById(id)

  for (const anim of el?.getAnimations() ?? []) {
    if (anim.playState === 'running') {
      return;
    }
  }

  if (el) {
    const item = document.getElementById(draggingId.value);
    console.log(`draggingIndex: ${draggingIndex.value}, getItemIndex: ${getItemIndex(id)}`);

    if (draggingIndex.value < getItemIndex(id)) {
      el.after(item!)
      el.style.animation = 'transform-up 150ms ease 0s'
      item!.style.animation = 'transform-down 150ms ease 0s'
    } else {
      el.before(item!)
      el.style.animation = 'transform-down 150ms ease 0s'
      item!.style.animation = 'transform-up 150ms ease 0s'
    }
    draggingIndex.value = getItemIndex(draggingId.value);
  }
}

const dragStart = (id: string) => {
  draggingId.value = id
  console.log(draggingId.value)
}

onMounted(() => {
  document.getElementById("sortable-list")?.addEventListener("dragover", (event) => {
    event.preventDefault();
    if (event.dataTransfer) event.dataTransfer.dropEffect = "move"
  });
})
</script>

<template>
  <div id="sortable-list" class="w-88 h-120 bg-white rounded-md shadow-xl px-4 py-2 overflow-scroll">
    <div
        v-for="(fruit, ind) in fruits"
        :id="`fruit-${ind}`"
        :key="fruit"
        class="border-y-1 my-2 bg-gray-100 rounded-md p-2"
        style="transform: translate3d(0px, 0px, 0px);"
        draggable="true"
        @dragstart="dragStart(`fruit-${ind}`)"
        @dragenter="dragOver(`fruit-${ind}`)"
    >
      <p class="cursor-pointer">
        {{ fruit }}
      </p>
    </div>
  </div>
</template>

<style scoped>
</style>
