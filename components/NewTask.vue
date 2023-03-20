<script setup lang="ts">
import { Task } from "@/types";
import { nanoid } from "nanoid";

const emit = defineEmits<{ (e: "add", payload: Task): void }>();

const focus = ref(false);
const title = ref("");

function addTask(e: Event) {
  if (title.value.trim()) {
    emit("add", {
      id: nanoid(),
      createdAt: new Date(),
      title: title.value.trim(),
    } as Task);

    title.value = "";
  }
}
</script>

<template>
  <div>
    <textarea
      v-model="title"
      :placeholder="focus ? 'Enter title here...' : '+ Add A Task'"
      @focus="focus = true"
      @blur="focus = false"
      @keydown.enter.prevent="addTask"
      @keydown.tab.prevent="addTask"
      class="text-gray-500 w-full outline-none rounded"
      :class="{
        'p-2': focus,
        'cursor-pointer bg-transparent resize-none hover:bg-slate-300 h-10 p-2':
          !focus,
      }"
    ></textarea>
  </div>
</template>
