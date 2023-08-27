<template>
    <div class="flex items-start overflow-x-auto gap-4">
        <draggable
            v-model="columns"
            group="columns"
            :animation="150"
            handle=".drag-handle"
            item-key="id"
            class="flex gap-4 items-start"
        >
            <template #item="{element: column }">
                <div class="column bg-gray-200 p-5 rounded min-w-[250px]">
                    <header class="font-boald mb-4">
                        <DragHandler/>
                        <input 
                            class="bg-transparent focus:bg-white rounded px-1 w-4/5"
                            @keyup.enter="$event.target.blur()"
                            @keydown.backspace="column.title ==='' ? (columns = columns.filter((c) => c.id !== column.id)): null"
                            type="text"
                            v-model="column.title"
                        />
                    </header>
                        <draggable
                            v-model="column.tasks"
                            :group="{ name: 'tasks', pull: alt ? 'clone' : true }"
                            handle=".drag-handle"
                            :animation="150"
                            item-key="id"
                        > 
                            <template #item="{element: task}">
                                <div>
                                    <TrelloBoardTask :task="task" @delete="column.tasks = column.tasks.filter((t) => t.id !== $event)"/>
                                </div>
                            </template>
                        </draggable>
                    <footer>
                        <NewTask @add="column.tasks.push($event)"/>
                    </footer>
                </div>
            </template>
        </draggable>
        <button
            @click="createColumn"
            class="bg-gray-200 whitespace-nowrap p-2 rounded opacity-50"
        >
            + Add Another column
        </button>
    </div>
</template>

<script setup lang="ts">
import type { Column } from '@/types'
import { nanoid } from "nanoid"
import draggable from "vuedraggable"

const alt = useKeyModifier("Alt")
const columns = ref<Column[]>([
    {
        id: nanoid(),
        title: "Backlog",
        tasks: [
            {   
                id: nanoid(),
                title: "Create marketing landing page",
                createdAt: new Date()

            },
            {   
                id: nanoid(),
                title: "Develop cool new feature",
                createdAt: new Date()

            },
            {   
                id: nanoid(),
                title: "Fix page nav bub",
                createdAt: new Date()

            }
        ],
    },
    {id: nanoid(), title: "selected for Dev",  tasks: []},
    {id: nanoid(), title: "In progress",  tasks: []},
    {id: nanoid(), title: "QA",  tasks: []},
    {id: nanoid(), title: "Complete",  tasks: []},
])

function createColumn() {
    const column: Column = {
        id: nanoid(),
        title: "",
        tasks: []
    }

   columns.value.push(column)

    nextTick(() => {
        (document.querySelector(".column:last-of-type .title-input") as HTMLInputElement).focus()
    })
  
}
</script>