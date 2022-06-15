<template>
  <div class="flex flex-col gap-4">
    <template v-if="isAdding">
      <TodoForm @submit="handleSubmit" />
    </template>
    <template v-else>
      <button
        class="px-8 py-2 bg-emerald-300 hover:bg-emerald-500 self-center rounded-lg text-slate-50 font-normal"
        @click="isAdding = true"
      >
        New Task
      </button>
    </template>
    <ul class="flex flex-col gap-4">
      <TodoItem
        v-for="todo in todos"
        :key="todo.id"
        :todo="todo"
        @delete="handleDelete"
        @change="handleUpdate"
        @update="handleUpdate"
      />
    </ul>
  </div>
</template>

<script setup>
import ky from "ky";
import { onBeforeMount, ref } from "vue";
import TodoItem from "./TodoItem.vue";
import TodoForm from "./TodoForm.vue";

const todos = ref([]);
const isAdding = ref(false);

onBeforeMount(async () => {
  const resp = await ky.get("http://127.0.0.1:8000/api/todos").json();
  todos.value = resp.data;
});

async function handleSubmit(description) {
  const resp = await ky
    .post("http://127.0.0.1:8000/api/todos", {
      json: {
        description,
      },
    })
    .json();

  todos.value.unshift(resp.data);
  isAdding.value = false;
}

async function handleUpdate(todo, values) {
  const resp = await ky
    .patch(`http://127.0.0.1:8000/api/todos/${todo.id}`, {
      json: {
        completed: values.completed,
        description: values.description,
      },
    })
    .json();
  const index = todos.value.findIndex((t) => t.id === todo.id);
  todos.value.splice(index, 1, resp.data);
}

async function handleDelete(todo) {
  await ky.delete(`http://127.0.0.1:8000/api/todos/${todo.id}`).json();

  const index = todos.value.findIndex((t) => t.id === todo.id);
  todos.value.splice(index, 1);
}
</script>

<style scoped></style>
