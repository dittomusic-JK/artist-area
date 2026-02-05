<template>
  <!-- Mobile Header -->
  <header :class="[
    'md:hidden h-14 flex items-center justify-between px-4 border-b transition-colors',
    isDarkMode ? 'bg-[#0F0E0E] border-gray-800' : 'bg-white border-gray-200'
  ]">
    <!-- Shop Icon -->
    <button :class="['w-10 h-10 flex items-center justify-center', isDarkMode ? 'text-white' : 'text-gray-900']">
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <path d="M6 2L3 6v14a2 2 0 002 2h14a2 2 0 002-2V6l-3-4z"/>
        <line x1="3" y1="6" x2="21" y2="6"/>
        <path d="M16 10a4 4 0 01-8 0"/>
      </svg>
    </button>
    
    <!-- Centered Logo -->
    <img src="/images/ditto-logo.svg" alt="Ditto" class="h-6" :class="{ 'invert': isDarkMode }" />
    
    <!-- Hamburger Menu -->
    <button :class="['w-10 h-10 flex items-center justify-center', isDarkMode ? 'text-white' : 'text-gray-900']">
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <line x1="3" y1="6" x2="21" y2="6"/>
        <line x1="3" y1="12" x2="21" y2="12"/>
        <line x1="3" y1="18" x2="21" y2="18"/>
      </svg>
    </button>
  </header>
  
  <!-- Desktop Header -->
  <header :class="[
    'hidden md:flex h-[74px] items-center justify-between px-6 border-b transition-colors',
    isDarkMode ? 'bg-[#0F0E0E] border-gray-800' : 'bg-white border-gray-200'
  ]">
    <!-- Logo and Nav -->
    <div class="flex items-center gap-8">
      <img src="/images/ditto-logo.svg" alt="Ditto" class="h-7" :class="{ 'invert': isDarkMode }" />
      
      <!-- Nav Items -->
      <nav class="flex items-center gap-6">
        <a 
          v-for="item in navItems" 
          :key="item.id"
          :href="item.href"
          :class="[
            'text-sm font-medium transition-colors',
            item.active 
              ? 'text-ditto-purple' 
              : isDarkMode ? 'text-gray-400 hover:text-ditto-purple' : 'text-gray-600 hover:text-ditto-purple'
          ]"
        >
          {{ item.label }}
        </a>
      </nav>
    </div>
    
    <!-- Right side actions -->
    <div class="flex items-center gap-3">
      <button class="px-4 py-1.5 bg-success text-gray-900 text-sm font-medium rounded-full hover:opacity-90 transition-opacity">
        Upgrade
      </button>
      
      <button :class="['w-9 h-9 flex items-center justify-center rounded-full transition-colors', isDarkMode ? 'text-gray-400 hover:text-white hover:bg-gray-800' : 'text-gray-500 hover:text-gray-900 hover:bg-gray-100']">
        <IconHelp class="w-5 h-5" />
      </button>
      
      <button :class="['w-9 h-9 flex items-center justify-center rounded-full transition-colors', isDarkMode ? 'text-gray-400 hover:text-white hover:bg-gray-800' : 'text-gray-500 hover:text-gray-900 hover:bg-gray-100']">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M6 2L3 6v14a2 2 0 002 2h14a2 2 0 002-2V6l-3-4z"/>
          <line x1="3" y1="6" x2="21" y2="6"/>
          <path d="M16 10a4 4 0 01-8 0"/>
        </svg>
      </button>
      
      <!-- Theme Toggle -->
      <button 
        @click="$emit('toggle-theme')"
        :class="['w-9 h-9 flex items-center justify-center rounded-full transition-colors', isDarkMode ? 'text-gray-400 hover:text-white hover:bg-gray-800' : 'text-gray-500 hover:text-gray-900 hover:bg-gray-100']"
        :title="isDarkMode ? 'Switch to light mode' : 'Switch to dark mode'"
      >
        <!-- Sun icon for dark mode -->
        <svg v-if="isDarkMode" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <circle cx="12" cy="12" r="5"/>
          <line x1="12" y1="1" x2="12" y2="3"/>
          <line x1="12" y1="21" x2="12" y2="23"/>
          <line x1="4.22" y1="4.22" x2="5.64" y2="5.64"/>
          <line x1="18.36" y1="18.36" x2="19.78" y2="19.78"/>
          <line x1="1" y1="12" x2="3" y2="12"/>
          <line x1="21" y1="12" x2="23" y2="12"/>
          <line x1="4.22" y1="19.78" x2="5.64" y2="18.36"/>
          <line x1="18.36" y1="5.64" x2="19.78" y2="4.22"/>
        </svg>
        <!-- Moon icon for light mode -->
        <svg v-else width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M21 12.79A9 9 0 1 1 11.21 3 7 7 0 0 0 21 12.79z"/>
        </svg>
      </button>
      
      <button class="px-4 py-1.5 bg-ditto-purple text-white text-sm font-medium rounded-full flex items-center gap-2 hover:opacity-90 transition-opacity shadow-md shadow-ditto-purple/25">
        <svg class="w-4 h-4" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" d="M9 9l10.5-3m0 6.553v3.75a2.25 2.25 0 01-1.632 2.163l-1.32.377a1.803 1.803 0 11-.99-3.467l2.31-.66a2.25 2.25 0 001.632-2.163zm0 0V2.25L9 5.25v10.303m0 0v3.75a2.25 2.25 0 01-1.632 2.163l-1.32.377a1.803 1.803 0 01-.99-3.467l2.31-.66A2.25 2.25 0 009 15.553z" />
        </svg>
        Create
      </button>
      
      <!-- User Avatar -->
      <button :class="['w-9 h-9 rounded-full overflow-hidden flex items-center justify-center ring-2 ring-offset-2 transition-all', isDarkMode ? 'ring-transparent ring-offset-[#0F0E0E] hover:ring-ditto-purple' : 'ring-transparent ring-offset-white hover:ring-ditto-purple']">
        <img src="/images/artist_image.png" alt="User" class="w-full h-full object-cover" />
      </button>
    </div>
  </header>
</template>

<script setup lang="ts">
import { IconHelp } from '../icons'

defineProps<{
  isDarkMode: boolean
}>()

defineEmits<{
  (e: 'toggle-theme'): void
}>()

const navItems = [
  { id: 'artists', label: 'Artists', href: '#', active: true },
  { id: 'music', label: 'Music', href: '#', active: false },
  { id: 'videos', label: 'Videos', href: '#', active: false },
  { id: 'royalties', label: 'Royalties', href: '#', active: false },
  { id: 'analytics', label: 'Analytics', href: '#', active: false },
  { id: 'promotion', label: 'Promotion', href: '#', active: false },
  { id: 'publishing', label: 'Publishing', href: '#', active: false },
]
</script>
