<template>
    <div class="flex gap-4 overflow-x-auto items-start">
        <div v-for="column in columns" :key="column.id"
            class="column bg-gray-200 p-5 rounded min-w-[250px]">
            <header class="font-boald mb-4">
                {{ column.title }}
            </header>
            <TrelloBoardTask v-for="task in column.tasks" :task="task" :key="task.id"/>
            <footer>
                <button class="text-gray-500">+ Add a Card</button>
            </footer>
        </div>
    </div>
</template>

<script setup lang="ts">
import type { Column } from '@/types'
import { nanoid } from "nanoid"

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
</script>