<template>
  <footer v-if="$nav.currentPage !== 1" class="research-page-number">
    <SlideCurrentNo /> / <SlidesTotal />
  </footer>
</template>
