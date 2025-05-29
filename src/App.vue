<script setup lang="ts">
import { ref } from "vue";

type Todo = {
  id: number;
  title: string;
  completed: boolean;
};

const newTask: Todo[] = [
  { id: 1, title: "読書する", completed: false },
  { id: 2, title: "勉強する", completed: true },
  { id: 3, title: "散歩に行く", completed: false },
];
</script>

<template>
  <div class="min-h-screen flex items-center justify-center bg-gray-100">
    <div class="max-w-xl p-8 bg-white shadow-md w-full">
      <!-- ヘッダー部分 -->
      <div class="flex justify-end space-x-2 mb-4">
        <div class="bg-blue-500 text-white pl-2 pr-1 py-1 rounded-full font-semibold flex items-center">
          tasks
          <span class="bg-white text-blue-500 px-2 rounded-full ml-2">
            {{ newTask.length }}
          </span>
        </div>
        <div class="bg-red-500 text-white pl-3 pr-3 py-1 rounded-full font-semibold flex items-center cursor-pointer hover:bg-red-700">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-5 w-5 mr-1"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
            stroke-width="2"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5-4h4m-4 0a1 1 0 00-1 1v1h6V4a1 1 0 00-1-1m-4 0h4"
            />
          </svg>
          <span>Clear All</span>
        </div>
      </div>

      <!-- タスクリスト -->
      <ul>
        <li
          v-for="(todo, index) in newTask"
          :key="index"
          class="my-4 p-3 rounded-sm border border-gray-300 flex justify-between items-center hover:bg-gray-100"
        >
          <div class="flex items-center">
            <span
              class="text-white mr-3 rounded-full px-2 py-1 cursor-pointer"
              :class="todo.completed ? 'bg-green-500' : 'bg-gray-300'"
            >
              ✔
            </span>
            <span
              class="text-lg"
              :class="todo.completed ? 'line-through text-gray-400' : ''"
            >
              {{ todo.title }}
            </span>
          </div>
          <button aria-label="削除" class="text-gray-400 hover:text-red-600">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-8 w-8 cursor-pointer"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
              stroke-width="2"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5-4h4m-4 0a1 1 0 00-1 1v1h6V4a1 1 0 00-1-1m-4 0h4"
              />
            </svg>
          </button>
        </li>
      </ul>

      <!-- 新規入力 -->
      <div class="flex items-center space-x-2 mt-6">
        <input
          type="text"
          class="flex-1 border-2 border-blue-500 rounded-full p-3 hover:border-blue-700"
          placeholder="New Task"
        />
        <button
          class="p-3 rounded-full bg-blue-500 hover:bg-blue-700 text-white cursor-pointer"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-6 w-6"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
            stroke-width="2"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M12 4v16m8-8H4"
            />
          </svg>
        </button>
      </div>
    </div>
  </div>
</template>
