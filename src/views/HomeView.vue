<script lang="ts" setup>
import { ref } from 'vue'
import Form from '../components/Form.vue'
import Comments from '../components/Comments.vue'
import type { comment } from '../../types.ts'

const title = ref('My First Vue App 🔥')
const comments = ref<comment[]>(JSON.parse(localStorage.getItem('comments') || '[]'))

function addComment(newComment: string) {
  comments.value.push({
    id: crypto.randomUUID(),
    text: newComment,
    createdAt: new Date().toISOString(),
  })
  localStorage.setItem('comments', JSON.stringify(comments.value))
  console.log(comments.value)
}
</script>

<template>
  <div
    class="w-full flex flex-col items-center justify-start gap-8 min-h-screen p-6 md:p-10 bg-gray-100"
  >
    <Form :title="title" @add-comment="addComment" />
    <Comments :comments />
  </div>
</template>
