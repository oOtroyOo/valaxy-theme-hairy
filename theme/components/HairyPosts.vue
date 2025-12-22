<script lang="ts" setup>
import { computed, defineProps, ref, withDefaults } from 'vue'
import type { Post } from 'valaxy'
import { usePostList } from 'valaxy'
import { useLayoutPost } from '../composables'

const props = withDefaults(defineProps<{
  type?: string
  posts?: Post[]
  curPage?: number
  pagination?: boolean
  updated?: boolean
}>(), {
  curPage: 1,
  pagination: false,
})
const layout = useLayoutPost()

const pageSize = ref(7)

const routes = usePostList({ type: props.type || '' })

const posts = computed<any[]>(() => props.posts || routes.value)
const pagePosts = computed(() => posts.value.slice((props.curPage - 1) * pageSize.value, props.curPage * pageSize.value))

const displayedPosts = computed(() => props.pagination ? pagePosts.value : posts.value)
const reverse = computed(() => layout.value.includes('reverse'))
</script>

<template>
  <div class="mt-8">
    <HairyPostToggleLayout />
    <HairyUpdatedPost v-if="updated" :posts="posts" />
    <ul class="divide-y divide-gray-200 dark:divide-gray-700">
      <template v-for="post, i in displayedPosts" :key="i">
        <template v-if="post.music">
          <li class="mb-5 py-2 lt-sm:mb-5 lt-md:mb-6">
            <a
              class="text-size-2xl lt-sm:max-w-200px font-bold truncate cursor-pointer lt-sm:text-size-lg"
              :class="[reverse ? 'order-last' : 'order-first']"
            >
              {{ post.title }}
            </a>
            {{ post.excerpt }}
            <meting-js :id="post.music" type="song" theme="var(--hy-c-primary)" server="netease" />
          </li>
        </template>
        <HairyArticleImage v-else-if="layout.includes('image')" :post="post" :reverse="reverse && !((i % 2) === 0)" />
        <HairyArticleText v-else :post="post" />
      </template>
    </ul>
    <ValaxyPagination v-if="pagination" class="mb-6" :cur-page="curPage" :page-size="pageSize" :total="posts.length" />
  </div>
</template>

<style lang="scss">
.pagination {
  font-size: 16px;
}

.pagination .prev.active,
.pagination .next.active,
.pagination .page-number.active {
  font-weight: normal;
  background: transparent;
  color: var(--hy-c-primary);
  cursor: default;
}

.pagination .prev:hover,
.pagination .next:hover,
.pagination .page-number:hover {
  color: var(--va-c-bg);
  background: rgba(143, 230, 213, 0.8);
}
</style>
