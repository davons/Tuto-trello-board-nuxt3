<template>
<div 
  :title="task.createdAt.toLocaleDateString()"
  class="task bg-white p-2 mb-2 rounded shadow-sm max-w-[250px]"
  @focus="focused = true"
  @blur="focused = false"
  tabindex="0"
>
<DragHandler class="text-sm mr-2"/>
    <span>
        {{ task.title }}
    </span>
</div>  
</template>

<script setup lang="ts">
import type { ID, Task } from "@/types"

const props = defineProps<{task: Task}>()
const emits = defineEmits<{(e:"delete", paylod: ID): void}>()
const focused = ref(false)
onKeyStroke('Backspace', (e) => {
    if (focused.value) emits("delete", props.task.id)
})
</script>

<style>
.sortable-drag .task {
   transform: rotate(5deg);
}
.sortoble-ghost .task{
   position: relative;
}
.sortoble-ghost .task::after{
    content: "";
    @apply absolute top-0 bottom-0 left-0 right-0 bg-slate-300 rounded;
}
.task:focus, .task:focus-visible {
    @apply outline-gray-400 !important;
    outline: gray auto 1px;
}
</style>