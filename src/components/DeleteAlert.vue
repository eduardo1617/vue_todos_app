<template>
  <button @click="open = true">Delete</button>

  <transition name="fade">
    <div
      v-if="open"
      @click="open = false"
      class="fixed w-screen h-screen bg-gray-500/75 top-0 left-0"
    ></div>
  </transition>
  <transition name="pop">
    <div
      class="absolute z-50 w-2/6 h-fit bg-white top-1/4 left-1/3 rounded-md p-4 flex flex-col gap-4 text-left text-gray-700"
      v-if="open"
    >
      <h3 class="font-bold text-gray-800">Delete Task</h3>
      <p>
        Do you really want to delete this task? This process can no be undone
      </p>
      <div class="flex justify-center gap-4 mt-auto">
        <button
          class="border-solid border-2 border-gray-600 p-2 rounded-md"
          @click="open = false"
        >
          Cancel
        </button>
        <button
          class="bg-red-500 text-white p-2 rounded-md"
          @click="handleDelete"
        >
          Delete
        </button>
      </div>
      <!-- </div> -->
    </div>
  </transition>
</template>

<script setup>
import { ref } from "vue";

const open = ref(false);

const props = defineProps({
  todo: {
    required: true,
    type: Object,
  },
});

const emit = defineEmits(["delete"]);

function handleDelete() {
  emit("delete", props.todo);
  open.value = false;
}
</script>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.4s linear;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.pop-enter-active,
.pop-leave-active {
  transition: transform 0.4s cubic-bezier(0.5, 0, 0.5, 1), opacity 0.4s linear;
}

.pop-enter-from,
.pop-leave-to {
  opacity: 0;
  transform: scale(0.3) translateY(-50%);
}
</style>
