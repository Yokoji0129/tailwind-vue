<script setup lang="ts">
import { ref, onMounted } from "vue";

type Todo = {
  title: string;
  completed: boolean;
};

const todos = ref<Todo[]>([]);
const newTitle = ref<string>("");

// 追加
const isEditOpen = ref(false);
const editingIndex = ref<number | null>(null);
const editingTitle = ref("");

// 編集を開く
const openEdit = (index: number) => {
  editingIndex.value = index;
  editingTitle.value = todos.value[index].title;
  isEditOpen.value = true;
};

// 編集を保存
const saveEdit = () => {
  if (editingIndex.value !== null) {
    todos.value[editingIndex.value].title = editingTitle.value;
    localStorage.setItem("key", JSON.stringify(todos.value));
  }
  isEditOpen.value = false;
};

// タスクを追加する関数
const addTodo = (): void => {
  const title = newTitle.value.trim();
  if (title) {
    todos.value.push({ title, completed: false });
    localStorage.setItem("key", JSON.stringify(todos.value));
    newTitle.value = "";
  } else {
    alert("タスクを入力してください");
  }
};

//削除ボタン
const deleteTodo = (index: number): void => {
  todos.value.splice(index, 1);
  localStorage.setItem("key", JSON.stringify(todos.value));
};

//全削除ボタン
const allDeleteTodo = (): void => {
  todos.value.splice(0);
  localStorage.setItem("key", JSON.stringify(todos.value));
};

//完了切り替えボタン
const toggleBtn = (index: number): void => {
  todos.value[index].completed = !todos.value[index].completed;
  localStorage.setItem("key", JSON.stringify(todos.value));
};

onMounted(() => {
  todos.value = JSON.parse(localStorage.getItem("key") || "[]");
});
</script>

<template>
  <form @submit.prevent="addTodo">
    <div
      class="min-h-screen flex items-center justify-center bg-gray-100 relative"
    >
      <div class="max-w-xl p-8 bg-white shadow-md w-full">
        <!-- ヘッダー部分 -->
        <div class="flex justify-end space-x-2 mb-4">
          <div
            class="bg-blue-500 text-white pl-2 pr-1 py-1 rounded-full font-semibold flex items-center"
          >
            tasks
            <span class="bg-white text-blue-500 px-2 rounded-full ml-2">
              {{ todos.length }}
            </span>
          </div>
          <div
            type="button"
            @click="allDeleteTodo"
            class="bg-red-500 text-white pl-3 pr-3 py-1 rounded-full font-semibold flex items-center cursor-pointer hover:bg-red-700"
          >
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
        <div class="overflow-y-auto md:max-h-[600px] max-md:max-h-[400px]">
          <ul>
            <li
              v-for="(todo, index) in todos"
              :key="index"
              class="my-4 p-3 rounded-sm border border-gray-300 flex justify-between items-center hover:bg-gray-100"
              @click="openEdit(index)"
            >
              <div class="flex items-center">
                <span
                  @click.stop="toggleBtn(index)"
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
              <button
                type="button"
                aria-label="削除"
                class="text-gray-400 hover:text-red-600"
                @click.stop="deleteTodo(index)"
              >
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
        </div>

        <!-- 編集モーダル -->
        <div
          v-if="isEditOpen"
          class="fixed inset-0 bg-black/30 flex items-center justify-center"
        >
          <div class="bg-white p-6 rounded shadow w-80">
            <h2 class="text-lg font-bold mb-2">タスクを編集</h2>
            <input v-model="editingTitle" class="border p-2 w-full mb-4" />
            <div class="flex justify-end space-x-2">
              <button
                type="button"
                @click="isEditOpen = false"
                class="bg-gray-300 px-4 py-2 rounded"
              >
                キャンセル
              </button>
              <button
                type="submit"
                @click="saveEdit"
                class="bg-blue-500 text-white px-4 py-2 rounded"
              >
                保存
              </button>
            </div>
          </div>
        </div>

        <!-- 新規入力 -->
        <div class="flex items-center space-x-2 mt-6">
          <input
            type="text"
            class="flex-1 border-2 border-blue-500 rounded-full p-3 hover:border-blue-700"
            placeholder="New Task"
            v-model="newTitle"
          />
          <button
            class="p-3 rounded-full bg-blue-500 hover:bg-blue-700 text-white cursor-pointer"
            type="submit"
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
  </form>
</template>
