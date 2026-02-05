<template>
  <nav class="flex items-center gap-2">
    <button
      v-for="tab in tabs"
      :key="tab.id"
      @click="$emit('navigate', tab.id)"
      :class="[
        'px-5 py-2.5 text-sm font-medium rounded-full transition-all duration-200',
        activeView === tab.id 
          ? 'bg-ditto-purple text-white shadow-md shadow-ditto-purple/25' 
          : isDarkMode 
            ? 'bg-[#141414] text-gray-400 border border-gray-700 hover:border-ditto-purple hover:text-ditto-purple'
            : 'bg-white text-gray-600 border border-gray-200 hover:border-ditto-purple hover:text-ditto-purple'
      ]"
    >
      {{ tab.label }}
    </button>
  </nav>
</template>

<script setup lang="ts">
import type { ArtistViewType } from '../../types'

defineProps<{
  activeView: ArtistViewType
  isDarkMode?: boolean
}>()

defineEmits<{
  (e: 'navigate', view: ArtistViewType): void
}>()

const tabs = [
  { id: 'overview' as ArtistViewType, label: 'Overview' },
  { id: 'releases' as ArtistViewType, label: 'Releases' },
  { id: 'contributions' as ArtistViewType, label: 'Contributions' },
  { id: 'smartlinks' as ArtistViewType, label: 'Smartlinks' },
  { id: 'integrations' as ArtistViewType, label: 'Integrations' },
  { id: 'contracts' as ArtistViewType, label: 'Contracts' },
]
</script>
