<script lang="ts" setup>
import type { Post } from 'valaxy'
import { defineProps } from 'vue'
import dayjs from 'dayjs'

defineProps<{
  post: Post
}>()
</script>

<template>
  <li class="mb-5 py-2 lt-sm:mb-5 lt-md:mb-6">
    <div class="flex justify-between items-center">
      <a class="text-size-2xl lt-sm:max-w-200px font-bold truncate cursor-default lt-sm:text-size-lg">
        {{ post.title }}
      </a>
      <div class="flex justify-end gap-2 text-size-sm lt-sm:text-size-xs">
        <span>{{ dayjs(post.date).format('YYYY-MM-DD') }}</span>
      </div>
    </div>
    <div class="text-left" v-html="post.excerpt" />
    <meting-js :id="post.music" type="song" theme="var(--hy-c-primary)" server="netease" />
  </li>
</template>
