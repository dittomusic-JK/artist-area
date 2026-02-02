<template>
  <div class="p-6 lg:p-8">
    <!-- Header -->
    <div class="flex flex-col lg:flex-row lg:items-center lg:justify-between gap-4 mb-6">
      <h1 class="text-2xl lg:text-3xl font-bold text-gray-900">Smartlinks</h1>
      
      <div class="flex items-center gap-3">
        <SearchInput 
          v-model="searchQuery"
          placeholder="Search for a release"
          class="lg:w-64"
          :is-dark-mode="isDarkMode"
        />
        <button class="px-4 py-2.5 flex items-center gap-2 border border-gray-300 rounded-lg hover:border-ditto-purple transition-colors text-gray-700">
          <IconFilter class="w-5 h-5 text-gray-500" />
          <span class="text-sm font-medium">Filters</span>
        </button>
      </div>
    </div>
    
    <!-- Table -->
    <div class="bg-white rounded-xl overflow-hidden">
      <!-- Table Header -->
      <div class="hidden lg:grid grid-cols-12 gap-4 px-6 py-3 bg-gray-50 text-xs font-medium text-gray-500 uppercase tracking-wider">
        <div class="col-span-4">Details</div>
        <div class="col-span-2">Status</div>
        <div class="col-span-2">Release Date</div>
        <div class="col-span-2">Clicks</div>
        <div class="col-span-2"></div>
      </div>
      
      <!-- Table Body -->
      <div class="divide-y divide-gray-100">
        <div 
          v-for="smartlink in filteredSmartlinks" 
          :key="smartlink.id"
          class="grid grid-cols-1 lg:grid-cols-12 gap-2 lg:gap-4 px-4 lg:px-6 py-4 hover:bg-gray-50 transition-colors"
        >
          <!-- Details -->
          <div class="lg:col-span-4 flex items-center gap-3">
            <div class="relative">
              <img 
                :src="smartlink.artwork" 
                :alt="smartlink.title" 
                class="w-12 h-12 rounded object-cover flex-shrink-0 shadow-sm"
              />
              <span 
                :class="[
                  'absolute -bottom-1 -right-1 px-1.5 py-0.5 text-[9px] font-semibold rounded whitespace-nowrap',
                  smartlink.status === 'Released' ? 'bg-success text-white' : 'bg-ditto-purple text-white'
                ]"
              >
                {{ smartlink.status === 'Released' ? 'Live' : 'Pre' }}
              </span>
            </div>
            <div class="min-w-0">
              <p class="font-medium text-gray-900 truncate">{{ smartlink.title }}</p>
              <p class="text-xs text-ditto-purple">by {{ smartlink.artist }}</p>
              <p class="text-xs text-gray-400 truncate">{{ smartlink.url }}</p>
            </div>
          </div>
          
          <!-- Status (Mobile shows inline) -->
          <div class="lg:col-span-2 flex items-center">
            <span class="w-3 h-3 rounded-full bg-success mr-2"></span>
            <span class="text-sm text-gray-500 lg:hidden">Active</span>
          </div>
          
          <!-- Release Date -->
          <div class="lg:col-span-2 flex items-center">
            <span class="text-sm text-gray-500">{{ smartlink.releaseDate }}</span>
          </div>
          
          <!-- Clicks -->
          <div class="lg:col-span-2 flex items-center">
            <span class="text-sm font-medium text-gray-900">{{ formatNumber(smartlink.clicks) }}</span>
          </div>
          
          <!-- Actions -->
          <div class="lg:col-span-2 flex items-center justify-end gap-2">
            <button 
              class="p-2 hover:bg-gray-100 rounded-lg transition-colors"
              title="View Analytics"
            >
              <img src="/images/analytics.svg" alt="Analytics" class="w-5 h-5" />
            </button>
            <button 
              class="p-2 hover:bg-gray-100 rounded-lg transition-colors"
              title="Edit"
            >
              <img src="/images/edit.svg" alt="Edit" class="w-5 h-5" />
            </button>
            <button 
              @click="copyLink(smartlink.url)"
              class="p-2 hover:bg-gray-100 rounded-lg transition-colors"
              title="Copy Link"
            >
              <img src="/images/link-alt (1) 1.svg" alt="Copy Link" class="w-5 h-5" />
            </button>
          </div>
        </div>
      </div>
      
      <!-- Empty State -->
      <div v-if="filteredSmartlinks.length === 0" class="px-6 py-12 text-center">
        <p class="text-gray-500">No smartlinks found</p>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import type { Smartlink } from '../types'
import SearchInput from '../components/common/SearchInput.vue'
import { IconFilter } from '../components/icons'

const props = defineProps<{
  smartlinks: Smartlink[]
  isDarkMode?: boolean
}>()

const searchQuery = ref('')

const filteredSmartlinks = computed(() => {
  if (!searchQuery.value) return props.smartlinks
  
  const query = searchQuery.value.toLowerCase()
  return props.smartlinks.filter(smartlink => 
    smartlink.title.toLowerCase().includes(query) ||
    smartlink.artist.toLowerCase().includes(query)
  )
})

const formatNumber = (num: number): string => {
  return num.toLocaleString()
}

const copyLink = async (url: string) => {
  try {
    await navigator.clipboard.writeText(url)
    // Could add a toast notification here
  } catch (err) {
    console.error('Failed to copy:', err)
  }
}
</script>
