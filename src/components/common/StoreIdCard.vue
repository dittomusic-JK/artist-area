<template>
  <div :class="[
    'flex items-center gap-3 p-4 rounded-xl border transition-all duration-200 hover:shadow-md',
    isDarkMode 
      ? 'bg-[#141414] border-gray-700/50 hover:border-gray-600' 
      : 'bg-white border-gray-200 hover:border-gray-300 shadow-sm'
  ]">
    <!-- Store Icon -->
    <img :src="storeIconUrl" :alt="storeName" class="w-10 h-10 flex-shrink-0 rounded-lg" />
    
    <!-- Store Info -->
    <div class="flex-1 min-w-0">
      <p :class="['text-xs', isDarkMode ? 'text-gray-500' : 'text-gray-400']">{{ storeName }} ID</p>
      <p :class="['text-sm font-medium truncate', isDarkMode ? 'text-white' : 'text-gray-900', !storeId && 'text-gray-400']">
        {{ storeId || '-' }}
      </p>
    </div>
    
    <!-- Copy Button -->
    <button 
      v-if="storeId"
      @click="copyToClipboard"
      :class="['p-2 transition-colors', isDarkMode ? 'text-gray-500 hover:text-ditto-purple' : 'text-gray-400 hover:text-ditto-purple']"
      :title="copied ? 'Copied!' : 'Copy ID'"
    >
      <IconCopy class="w-5 h-5" />
    </button>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import { IconCopy } from '../icons'

const props = defineProps<{
  store: 'spotify' | 'appleMusic' | 'soundcloud' | 'deezer' | 'audiomack' | 'amazon'
  storeId?: string
  isDarkMode?: boolean
}>()

const copied = ref(false)

const storeConfig = {
  spotify: { name: 'Spotify', icon: '/images/stores/spotify-icon.svg' },
  appleMusic: { name: 'Apple Music', icon: '/images/stores/apple-music-icon.svg' },
  soundcloud: { name: 'Soundcloud', icon: '/images/stores/soundcloud_icon.svg' },
  deezer: { name: 'Deezer', icon: '/images/stores/deezer-icon.svg' },
  audiomack: { name: 'Audiomack', icon: '/images/audiomack-icon.svg' },
  amazon: { name: 'Amazon', icon: '/images/stores/amazonmusic-icon.svg' },
}

const storeName = computed(() => storeConfig[props.store].name)
const storeIconUrl = computed(() => storeConfig[props.store].icon)

const copyToClipboard = async () => {
  if (!props.storeId) return
  
  try {
    await navigator.clipboard.writeText(props.storeId)
    copied.value = true
    setTimeout(() => {
      copied.value = false
    }, 2000)
  } catch (err) {
    console.error('Failed to copy:', err)
  }
}
</script>
