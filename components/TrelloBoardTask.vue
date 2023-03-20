<script setup lang="ts">
import type { ID, Task } from "@/types";
import { onKeyStroke } from "@vueuse/core";

const focus = ref(false);

const props = defineProps<{
  task: Task;
}>();

const emit = defineEmits<{
  (e: "delete", payload: ID): void;
}>();

onKeyStroke("Delete", (e) => {
  if (focus.value) {
    emit("delete", props.task.id);
  }
});
</script>
<template>
  <div
    class="task mb-2 p-3 bg-white rounded focus:outline-2 focus:outline-slate-400 focus:outline-dashed"
    tabindex="0"
    @focus="focus = true"
    @blur="focus = false"
  >
    <DragHandle class="pr-2" />
    <span>{{ task.title }}</span>
  </div>
</template>
