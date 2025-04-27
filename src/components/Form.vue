<script lang="ts" setup>
import { ref, defineComponent } from 'vue'

const props = defineProps<{
  title: string
}>()

const emit = defineEmits<{
  addComment: [newComment: string]
}>()

const newComment = ref('')
const error = ref<string | null>(null)

function formSubmitted() {
  if (newComment.value.trim() !== '') {
    emit('addComment', newComment.value)
    newComment.value = ''
    error.value = null
  } else {
    error.value = 'Comment cannot be empty.'
  }
}

function clearLocalStorage() {
  localStorage.removeItem('comments')
  location.reload()
}
</script>

<template>
  <div class="w-full max-w-xl flex flex-col bg-white rounded-lg shadow-md p-6">
    <h1 class="text-2xl font-bold text-center">{{ title }}</h1>
    <form @submit.prevent="formSubmitted" class="flex flex-col gap-2 mt-4">
      <p class="text-lg font-bold">Add Comments</p>
      <input
        :aria-invalid="!!error || undefined"
        :aria-required="true"
        class="h-10 w-full border-2 border-gray-500 px-2 py-1 rounded-sm"
        type="text"
        v-model="newComment"
        placeholder="Type something..."
      />
      <small v-if="error" class="text-red-500 text-sm mb-2 -mt-1">
        {{ error }}
      </small>
      <button class="h-10 bg-green-400 text-white rounded-sm hover:bg-green-500" type="submit">
        Add
      </button>
      <p class="text-sm text-gray-500 text-center mt-2">
        Comments will be saved in local storage.
        <button type="button" class="text-red-400 hover:underline" @click="clearLocalStorage">
          Clear local storage.
        </button>
      </p>
    </form>
  </div>
</template>
