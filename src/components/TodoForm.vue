<template>
  <form @submit.prevent="handleSubmit" class="flex flex-col gap-4">
    <div class="flex flex-row gap-4">
      <input
        type="text"
        v-model="description"
        ref="inputRef"
        class="px-0.5 border-0 border-b-2 border-gray-200 focus:ring-0 focus:border-emerald-500 flex-grow"
      />
      <button
        type="submit"
        class="px-8 py-2 bg-emerald-300 hover:bg-emerald-500 self-center rounded-lg text-slate-50 font-normal"
      >
        Save
      </button>
    </div>

    <p v-if="errors" class="text-center text-red-500 font-bold capitalize">
      {{ errors }}
    </p>
  </form>
</template>

<script setup>
import { ref } from "@vue/reactivity";

const description = ref();
const errors = ref("");
const inputRef = ref(null);

const emit = defineEmits(["submit"]);

function handleSubmit() {
  if (description.value) {
    emit("submit", description.value);
    description.value = "";
    errors.value = "";
  } else {
    errors.value = "the task is required";
    inputRef.value.focus();
  }
}
</script>

<style lang="scss" scoped></style>
