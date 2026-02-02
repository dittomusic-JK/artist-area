<template>
  <div class="p-6 lg:p-8">
    <!-- Header -->
    <div class="flex flex-col lg:flex-row lg:items-center lg:justify-between gap-4 mb-6">
      <h1 class="text-2xl lg:text-3xl font-bold text-gray-900">Contributions</h1>
      
      <div class="flex items-center gap-3">
        <SearchInput 
          v-model="searchQuery"
          placeholder="Search for a contribution"
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
        <div class="col-span-5">Details</div>
        <div class="col-span-3">Release Date</div>
        <div class="col-span-2">Artist role</div>
        <div class="col-span-2">Status</div>
      </div>
      
      <!-- Table Body -->
      <div class="divide-y divide-gray-100">
        <div 
          v-for="contribution in filteredContributions" 
          :key="contribution.id"
          class="grid grid-cols-1 lg:grid-cols-12 gap-2 lg:gap-4 px-4 lg:px-6 py-4 hover:bg-gray-50 transition-colors"
        >
          <!-- Details -->
          <div class="lg:col-span-5 flex items-center gap-3">
            <img 
              :src="contribution.artwork" 
              :alt="contribution.title" 
              class="w-12 h-12 rounded object-cover flex-shrink-0 shadow-sm"
            />
            <span class="font-medium text-gray-900 truncate">{{ contribution.title }}</span>
          </div>
          
          <!-- Release Date -->
          <div class="lg:col-span-3 flex items-center">
            <span class="text-sm text-gray-500">{{ contribution.releaseDate }}</span>
          </div>
          
          <!-- Artist Role -->
          <div class="lg:col-span-2 flex items-center">
            <span class="text-sm text-gray-500">{{ contribution.artistRole }}</span>
          </div>
          
          <!-- Status -->
          <div class="lg:col-span-2 flex items-center">
            <span 
              :class="[
                'px-2 py-1 text-xs font-medium rounded-full',
                getStatusClass(contribution.status)
              ]"
            >
              {{ contribution.status }}
            </span>
          </div>
        </div>
      </div>
      
      <!-- Empty State -->
      <div v-if="filteredContributions.length === 0" class="px-6 py-12 text-center">
        <p class="text-gray-500">No contributions found</p>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import type { ArtistRelease, ReleaseStatus } from '../types'
import SearchInput from '../components/common/SearchInput.vue'
import { IconFilter } from '../components/icons'

const props = defineProps<{
  contributions: ArtistRelease[]
  isDarkMode?: boolean
}>()

const searchQuery = ref('')

const filteredContributions = computed(() => {
  if (!searchQuery.value) return props.contributions
  
  const query = searchQuery.value.toLowerCase()
  return props.contributions.filter(contribution => 
    contribution.title.toLowerCase().includes(query)
  )
})

const getStatusClass = (status: ReleaseStatus): string => {
  switch (status) {
    case 'Active':
      return 'bg-success/20 text-success'
    case 'Sent to stores':
      return 'bg-ditto-purple/20 text-ditto-purple'
    case 'Pending':
      return 'bg-warning/20 text-warning'
    case 'Processing':
      return 'bg-info/20 text-info'
    default:
      return 'bg-ditto-light-grey text-ditto-subtext'
  }
}
</script>
