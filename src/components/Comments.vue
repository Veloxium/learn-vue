<script lang="ts" setup>
import { ref, onMounted, onUnmounted } from 'vue'
import type { comment } from '../../types.ts'
import { defineProps } from 'vue'

const props = defineProps<{
  comments: comment[]
}>()

const now = ref(new Date().getTime())

let timer: number
onMounted(() => {
  timer = setInterval(() => {
    now.value = new Date().getTime()
  }, 60000)
})

onUnmounted(() => {
  clearInterval(timer)
})

const getTimeAgo = (createdAt: string) => {
  const createdAtTime = new Date(createdAt).getTime()
  const diffInSeconds = Math.max(0, Math.floor((now.value - createdAtTime) / 1000))

  if (diffInSeconds < 60) {
    return `${diffInSeconds} second${diffInSeconds === 1 ? '' : 's'} ago`
  }

  const diffInMinutes = Math.floor(diffInSeconds / 60)
  if (diffInMinutes < 60) {
    return `${diffInMinutes} minute${diffInMinutes === 1 ? '' : 's'} ago`
  }

  const diffInHours = Math.floor(diffInMinutes / 60)
  if (diffInHours < 24) {
    return `${diffInHours} hour${diffInHours === 1 ? '' : 's'} ago`
  }

  const diffInDays = Math.floor(diffInHours / 24)
  if (diffInDays < 30) {
    return `${diffInDays} day${diffInDays === 1 ? '' : 's'} ago`
  }

  const diffInMonths = Math.floor(diffInDays / 30)
  if (diffInMonths < 12) {
    return `${diffInMonths} month${diffInMonths === 1 ? '' : 's'} ago`
  }

  const diffInYears = Math.floor(diffInMonths / 12)
  return `${diffInYears} year${diffInYears === 1 ? '' : 's'} ago`
}
</script>

<template>
  <div class="w-full">
    <p v-if="!comments.length" class="mb-6">
      No comments yet. Be the first to add a comment! <br />
      <span class="text-sm text-gray-500">Comments will appear here.</span>
    </p>
    <p v-else class="mb-6">
      {{ comments.length }} comment{{ comments.length > 1 ? 's' : '' }} so far.
    </p>
    <ul class="w-full flex gap-2 flex-wrap">
      <li
        v-for="comment in [...comments].reverse()"
        :key="comment.id"
        class="bg-white py-2 px-4 min-w-[140px] text-lg rounded-lg shadow-md p-2"
      >
        {{ comment.text }}
        <br />
        <div class="text-xs text-gray-500 flex justify-end w-full gap-1 mt-1">
          <span>
            {{ getTimeAgo(comment.createdAt) }}
          </span>
        </div>
      </li>
    </ul>
  </div>
</template>
