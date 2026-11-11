<script setup lang="ts">
import { computed } from 'vue'
import { useNav, useSlideContext } from '@slidev/client'

const { $frontmatter, $page } = useSlideContext()
const { slides } = useNav()
const tag = computed(() => {
  const value = $frontmatter.tag
  return typeof value === 'string' ? value.trim() : ''
})

const group = computed(() => {
  const members = slides.value.filter((slide) => {
    const value = slide.meta.slide.frontmatter.tag
    return typeof value === 'string' && value.trim() === tag.value
  })
  return {
    current: members.findIndex(slide => slide.no === $page.value) + 1,
    total: members.length,
  }
})

const progress = computed(() => group.value.total ? group.value.current / group.value.total : 0)
</script>

<template>
  <div v-if="tag" class="research-slide-tag" :title="tag">
    <span class="research-slide-tag-label">{{ tag }}</span>
    <span
      class="research-slide-tag-progress"
      role="progressbar"
      :aria-label="`${tag} progress`"
      :aria-valuenow="group.current"
      :aria-valuemin="0"
      :aria-valuemax="group.total"
      :aria-valuetext="`${group.current} of ${group.total} slides`"
    >
      <span class="research-slide-tag-progress-fill" :style="{ width: `${progress * 100}%` }" />
    </span>
  </div>
  <footer v-if="$page !== 1" class="research-page-number">
    {{ $page }} / {{ slides.length }}
  </footer>
</template>

<style scoped>
.research-slide-tag {
  position: absolute;
  top: 7px;
  right: 14px;
  display: flex;
  align-items: center;
  gap: 8px;
  max-width: min(360px, calc(100% - 112px));
  padding: 5px 18px 5px 12px;
  isolation: isolate;
  color: #414141;
  filter: drop-shadow(0 2px 3px rgb(0 0 0 / 14%));
  transform: rotate(-1deg);
  transform-origin: top right;
  font-family: 'Nunito Sans Variable', sans-serif;
  font-size: 13px;
  font-weight: 600;
  line-height: 1.25;
  white-space: nowrap;
  print-color-adjust: exact;
}

.research-slide-tag::before {
  content: '';
  position: absolute;
  inset: 0;
  z-index: -1;
  border-left: 3px solid #aeb4ba;
  background: linear-gradient(180deg, #f7f7f7, #eceeef);
  clip-path: polygon(0 0, 100% 0, 100% calc(100% - 9px), calc(100% - 9px) 100%, 0 100%);
}

.research-slide-tag::after {
  content: '';
  position: absolute;
  right: 0;
  bottom: 0;
  width: 9px;
  height: 9px;
  background: #c7ccd1;
  clip-path: polygon(0 0, 100% 0, 0 100%);
}

.research-slide-tag-label {
  min-width: 0;
  overflow: hidden;
  text-overflow: ellipsis;
}

.research-slide-tag-progress {
  flex-shrink: 0;
  width: 48px;
  height: 3px;
  overflow: hidden;
  border-radius: 2px;
  background: #d6d9dc;
}

.research-slide-tag-progress-fill {
  display: block;
  height: 100%;
  border-radius: inherit;
  background: #737b83;
}
</style>
