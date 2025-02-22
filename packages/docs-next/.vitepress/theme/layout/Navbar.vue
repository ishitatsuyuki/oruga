<script lang="ts" setup>
import { ref, computed } from 'vue'
import { useSidebar } from 'vitepress/client/theme-default/composables/sidebar'
import VPNavBarTitle from 'vitepress/client/theme-default/components/VPNavBarTitle.vue'
import VPNavBarSearch from 'vitepress/client/theme-default/components/VPNavBarSearch.vue'
import VPNavBarMenu from 'vitepress/client/theme-default/components/VPNavBarMenu.vue'
import VPNavBarTranslations from 'vitepress/client/theme-default/components/VPNavBarTranslations.vue'
import VPNavBarAppearance from 'vitepress/client/theme-default/components/VPNavBarAppearance.vue'
import VPNavBarSocialLinks from 'vitepress/client/theme-default/components/VPNavBarSocialLinks.vue'
import VPNavBarExtra from 'vitepress/client/theme-default/components/VPNavBarExtra.vue'
import VPNavBarHamburger from 'vitepress/client/theme-default/components/VPNavBarHamburger.vue'

defineProps<{
  isScreenOpen: boolean
}>()

defineEmits<{
  (e: 'toggle-screen'): void
}>()

const { hasSidebar } = useSidebar()

const selected = ref('')

const themeOptions = ref([
    { label: 'Base CSS', value: 'basecss' },
    { label: 'Full CSS', value: 'fullcss' },
    { label: 'Bulma CSS', value: 'bulmacss' }
])

const onThemeChange = function () {
    localStorage.setItem('oruga.io_theme', selected.value)
    location.reload()
}

const selectedOption = computed(() => {
    return themeOptions.value.filter(t => t.value === selected.value)[0]
})

if (typeof window !== 'undefined') {
    selected.value = localStorage.getItem('oruga.io_theme') || 'fullcss'
}
</script>

<template>
  <div class="VPNavBar" :class="{ 'has-sidebar' : hasSidebar }">
    <div class="container">
      <VPNavBarTitle />

      <div class="content">

        <div class="theme-selector" v-if="hasSidebar">
            Theme ->
            <select
                v-model="selected"
                @update:modelValue="onThemeChange"
            >
                <option v-for="item in themeOptions" :value="item.value">
                    {{ item.label }}
                </option>
            </select>
        </div>

        <VPNavBarSearch class="search" />
        <VPNavBarMenu class="menu" />
        <VPNavBarTranslations class="translations" />
        <VPNavBarAppearance class="appearance" />
        <VPNavBarSocialLinks class="social-links" />
        <VPNavBarExtra class="extra" />
        <VPNavBarHamburger
          class="hamburger"
          :active="isScreenOpen"
          @click="$emit('toggle-screen')"
        />
      </div>
    </div>
  </div>
</template>

<style scoped>
.theme-selector {
    flex: auto;
    margin-left: 10px;
    align-items: center;
    padding: 0 12px;
    line-height: var(--vp-nav-height-mobile);
    font-size: 14px;
    font-weight: 500;
    color: var(--vp-c-text-1);
    transition: color 0.25s;
}
.theme-selector select {
    cursor: pointer;
    appearance: menulist;
}
.VPNavBar {
  position: relative;
  border-bottom: 1px solid var(--vp-c-divider-light);
  padding: 0 8px 0 24px;
  height: var(--vp-nav-height-mobile);
  transition: border-color 0.5s, background-color 0.5s;
}

@media (min-width: 768px) {
  .VPNavBar {
    padding: 0 32px;
  }
}

@media (min-width: 960px) {
  .VPNavBar {
    height: var(--vp-nav-height-desktop);
    border-bottom: 0;
  }

  .VPNavBar.has-sidebar .content {
    margin-right: -32px;
    padding-right: 32px;
    -webkit-backdrop-filter: saturate(50%) blur(8px);
    backdrop-filter: saturate(50%) blur(8px);
    background: rgba(255, 255, 255, 0.7);
  }

  .dark .VPNavBar.has-sidebar .content {
    background: rgba(36, 36, 36, 0.7);
  }

  @supports not (backdrop-filter: saturate(50%) blur(8px)) {
    .VPNavBar.has-sidebar .content {
      background: rgba(255, 255, 255, 0.95);
    }

    .dark .VPNavBar.has-sidebar .content {
      background: rgba(36, 36, 36, 0.95);
    }
  }
}

.container {
  display: flex;
  justify-content: space-between;
  margin: 0 auto;
  max-width: calc(var(--vp-layout-max-width) - 64px);
}

.content {
  display: flex;
  justify-content: flex-end;
  align-items: center;
  flex-grow: 1;
}

.menu + .translations::before,
.menu + .appearance::before,
.menu + .social-links::before,
.translations + .appearance::before,
.appearance + .social-links::before {
  margin-right: 8px;
  margin-left: 8px;
  width: 1px;
  height: 24px;
  background-color: var(--vp-c-divider-light);
  content: "";
}

.menu + .appearance::before,
.translations + .appearance::before {
  margin-right: 16px;
}

.appearance + .social-links::before {
  margin-left: 16px;
}

.social-links {
  margin-right: -8px;
}
</style>
