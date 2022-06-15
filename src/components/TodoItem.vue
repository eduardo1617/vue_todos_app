<template>
  <li class="p-4 w-min-full flex gap-4 bg-emerald-200 items-center rounded">
    <template v-if="isEditing">
      <input type="text" v-model="description" />
      <div class="ml-auto">
        <button
          class="px-4 py-2 bg-teal-900 text-white rounded-md"
          @click="handleUpdate"
        >
          Update
        </button>
        <button
          @click="isEditing = false"
          class="px-4 py-2 bg-red-500 text-white ml-1 rounded-md"
        >
          Cancel
        </button>
      </div>
    </template>
    <template v-else>
      <input type="checkbox" :checked="todo.completed" @change="handleChange" />
      <p>{{ todo.id }}.</p>
      <p>{{ todo.description }}</p>
      <div class="ml-auto">
        <button
          class="px-4 py-2 bg-teal-900 text-white rounded-md"
          @click="isEditing = true"
        >
          Edit
        </button>
        <button class="px-4 py-2 bg-red-500 text-white ml-1 rounded-md">
          <DeleteAlert :todo="todo" @delete="(todo) => $emit('delete', todo)" />
        </button>
      </div>
    </template>
  </li>
</template>

<script setup>
import { ref } from "@vue/reactivity";
import DeleteAlert from "./DeleteAlert.vue";

const props = defineProps({
  todo: {
    required: true,
    type: Object,
  },
});

const isEditing = ref(false);
const description = ref(props.todo.description);
const emit = defineEmits(["change", "update"]);

function handleChange() {
  emit("change", props.todo, { completed: !props.todo.completed });
}

function handleUpdate() {
  emit("update", props.todo, { description: description.value });
  isEditing.value = false;
}
</script>

<style scoped></style>
