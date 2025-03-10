<script setup lang="ts">
import { useRoute } from 'vitepress'
import { useData } from '../composables/data.js'
import { useSidebar } from '../composables/sidebar.js'
import VPPage from './VPPage.vue'
import VPHome from './VPHome.vue'
import VPDoc from './VPDoc.vue'
import { inject } from 'vue'

const route = useRoute()
const { frontmatter } = useData()
const { hasSidebar } = useSidebar()

const NotFound = inject('NotFound')
</script>

<template>
  <div
    class="VPContent"
    id="VPContent"
    :class="{
      'has-sidebar': hasSidebar,
      'is-home': frontmatter.layout === 'home'
    }"
  >
    <NotFound v-if="route.component === NotFound" />

    <VPPage v-else-if="frontmatter.layout === 'page'" />

    <VPHome v-else-if="frontmatter.layout === 'home'">
      <template #home-hero-before><slot name="home-hero-before" /></template>
      <template #home-hero-info><slot name="home-hero-info" /></template>
      <template #home-hero-image><slot name="home-hero-image" /></template>
      <template #home-hero-after><slot name="home-hero-after" /></template>
      <template #home-features-before><slot name="home-features-before" /></template>
      <template #home-features-after><slot name="home-features-after" /></template>
    </VPHome>

    <VPDoc v-else>
      <template #doc-footer-before><slot name="doc-footer-before" /></template>
      <template #doc-before><slot name="doc-before" /></template>
      <template #doc-after><slot name="doc-after" /></template>

      <template #aside-top><slot name="aside-top" /></template>
      <template #aside-outline-before><slot name="aside-outline-before" /></template>
      <template #aside-outline-after><slot name="aside-outline-after" /></template>
      <template #aside-ads-before><slot name="aside-ads-before" /></template>
      <template #aside-ads-after><slot name="aside-ads-after" /></template>
      <template #aside-bottom><slot name="aside-bottom" /></template>
    </VPDoc>
  </div>
</template>

<style scoped>
.VPContent {
  flex-grow: 1;
  flex-shrink: 0;
  margin: var(--vp-layout-top-height, 0px) auto 0;
  width: 100%;
}

.VPContent.is-home {
  width: 100%;
  max-width: 100%;
}

.VPContent.has-sidebar {
  margin: 0;
}

@media (min-width: 960px) {
  .VPContent {
    padding-top: var(--vp-nav-height);
  }

  .VPContent.has-sidebar {
    margin: var(--vp-layout-top-height, 0px) 0 0;
    padding-left: var(--vp-sidebar-width);
  }
}

@media (min-width: 1440px) {
  .VPContent.has-sidebar {
    padding-right: calc((100vw - var(--vp-layout-max-width)) / 2);
    padding-left: calc((100vw - var(--vp-layout-max-width)) / 2 + var(--vp-sidebar-width));
  }
}
</style>
