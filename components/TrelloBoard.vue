<script setup lang="ts">
import { Column, Task } from "@/types";
import { nanoid } from "nanoid";
import draggable from "vuedraggable";

const columns = useLocalStorage<Column[]>("trelloBoard", [
  {
    id: nanoid(),
    title: "The first task",
    tasks: [],
  },
  {
    id: nanoid(),
    title: "Science Trace Project",
    tasks: [
      {
        id: nanoid(),
        title: "add header on home page",
        createdAt: new Date(),
      },
      {
        id: nanoid(),
        title: "fix bug of clicking button not working",
        createdAt: new Date(),
      },
    ],
  },
  {
    id: nanoid(),
    title: "Book the new VM",
    tasks: [
      {
        id: nanoid(),
        title: "Book new VM on GCP",
        createdAt: new Date(),
      },
    ],
  },
]);

function createColummn() {
  const column: Column = {
    id: nanoid(),
    title: "new Column",
    tasks: [],
  };

  columns.value.push(column);

  nextTick(() => {
    (
      document.querySelector(
        ".column:last-of-type .input-title"
      ) as HTMLInputElement
    ).focus();
  });
}

/* if you want to communicate with backend server... */
watch(
  columns,
  () => {
    // ajax request
  },
  { deep: true }
);

const alt = useKeyModifier("Alt");
</script>

<template>
  <div class="flex gap-4 overflow-x-auto items-start">
    <draggable
      v-model="columns"
      item-key="id"
      group="columns"
      :animation="200"
      handle=".drag-handle"
      class="flex gap-4 items-start shrink-0"
    >
      <template #item="{ element: column }: { element: Column }">
        <div class="column bg-gray-200 p-5 rounded min-w-[250px]">
          <header class="mb-2 text-lg font-black">
            <DragHandle />
            <input
              type="text"
              v-model="column.title"
              @keydown.enter="($event.target as HTMLInputElement).blur()"
              @keydown.backspace="
                column.title === ''
                  ? (columns = columns.filter((c) => c.id !== column.id))
                  : null
              "
              class="input-title bg-transparent focus:bg-white rounded px-1 focus:outline-slate-600"
            />
          </header>

          <draggable
            v-model="column.tasks"
            item-key="id"
            :group="{ name: 'tasks', pull: alt ? 'clone' : true }"
            :animation="200"
          >
            <template #item="{ element: task, element: Task }">
              <div>
                <TrelloBoardTask
                  :task="task"
                  @delete="
                    column.tasks = column.tasks.filter((t) => t.id !== $event)
                  "
                />
              </div>
            </template>
          </draggable>
          <NewTask @add="column.tasks.push($event)" />
        </div>
      </template>
    </draggable>
    <button
      @click.prevent="createColummn"
      class="rounded bg-slate-300 hover:bg-slate-500 text-white p-2"
    >
      + Add A Column
    </button>
  </div>
</template>

<style>
/* The dragging item when u click and hold on it. */
.sortable-chosen:not(.sortable-drag) {
  @apply shadow-md;
}

/* Once you move the draggable item, it will become .sortable-drag item. */
.sortable-drag .task {
  transform: rotate(-5deg);
}

/* The shadow below the position of dragging item. */
.sortable-ghost {
  position: relative;
}
.sortable-ghost::after {
  content: "";
  @apply absolute top-0 bottom-0 left-0 right-0 rounded bg-slate-400;
}
</style>
