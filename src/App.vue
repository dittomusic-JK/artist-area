<template>
  <div 
    :class="[
      'min-h-screen overflow-hidden w-full max-w-full transition-colors duration-300',
      isDarkMode ? 'bg-[#0F0E0E]' : 'bg-gray-50'
    ]" 
    :data-ditto-colors-light-dark-mode="isDarkMode ? 'dark' : 'light'"
  >
    <!-- Top Navigation -->
    <TopNavbar :is-dark-mode="isDarkMode" @toggle-theme="isDarkMode = !isDarkMode" />
    
    <!-- Main Layout with Left Sidebar -->
    <div class="flex h-[calc(100vh-74px)]">
      <!-- Left Artist List Sidebar -->
      <ArtistListSidebar 
        :artists="artists"
        :selected-artist="selectedArtist"
        :is-dark-mode="isDarkMode"
        @select="selectArtist"
        @add="handleAddArtist"
        class="hidden lg:flex"
      />
      
      <!-- Main Content Area -->
      <div class="flex-1 min-w-0 overflow-y-auto">
        <!-- No Artist Selected State -->
        <div v-if="!selectedArtist" class="p-8">
          <div class="max-w-2xl mx-auto text-center py-20">
            <div :class="['w-24 h-24 mx-auto mb-6 rounded-full flex items-center justify-center', isDarkMode ? 'bg-[#141414]' : 'bg-gray-100']">
              <IconPlus class="w-10 h-10 text-gray-400" />
            </div>
            <h2 :class="['text-2xl font-semibold mb-2', isDarkMode ? 'text-white' : 'text-gray-900']">Select an Artist</h2>
            <p :class="['mb-6', isDarkMode ? 'text-gray-400' : 'text-gray-500']">Choose an artist from the sidebar or add a new one to get started.</p>
            <button 
              @click="handleAddArtist"
              class="px-6 py-3 bg-ditto-purple text-white font-medium rounded-full hover:opacity-90 transition-opacity shadow-lg shadow-ditto-purple/25"
            >
              Add New Artist
            </button>
          </div>
        </div>
        
        <!-- Artist Detail View -->
        <div v-else>
          <!-- Horizontal Tab Navigation -->
          <div :class="['sticky top-0 z-10 px-6 lg:px-8 py-4 border-b', isDarkMode ? 'bg-[#0F0E0E] border-gray-800' : 'bg-white border-gray-200']">
            <HorizontalTabNav 
              :active-view="activeView"
              :is-dark-mode="isDarkMode"
              @navigate="setActiveView"
            />
          </div>
          
          <!-- View Content -->
          <div :class="isDarkMode ? 'bg-[#0F0E0E]' : 'bg-gray-50'">
            <OverviewView 
              v-if="activeView === 'overview'" 
              :artist="selectedArtist"
              :top-songs="topSongs"
              :top-countries="topCountries"
              :integrations="integrations"
              :is-dark-mode="isDarkMode"
              @edit-artist="handleEditArtist"
            />
            <ReleasesView 
              v-else-if="activeView === 'releases'" 
              :releases="releases"
              :is-dark-mode="isDarkMode"
            />
            <ContributionsView 
              v-else-if="activeView === 'contributions'" 
              :contributions="contributions"
              :is-dark-mode="isDarkMode"
            />
            <SmartlinksView 
              v-else-if="activeView === 'smartlinks'" 
              :smartlinks="smartlinks"
              :is-dark-mode="isDarkMode"
            />
            <IntegrationsView 
              v-else-if="activeView === 'integrations'" 
              :integrations="integrations"
              :is-dark-mode="isDarkMode"
            />
            <ContractsView 
              v-else-if="activeView === 'contracts'" 
              :available-contracts="availableContracts"
              :user-contracts="userContracts"
              :is-dark-mode="isDarkMode"
              @select-contract="handleContractSelect"
            />
            <EditArtistView 
              v-else-if="activeView === 'edit'" 
              :artist="selectedArtist"
              @cancel="activeView = 'overview'"
              @save="handleSaveArtist"
            />
          </div>
        </div>
      </div>
    </div>
    
    <!-- Edit Artist Modal -->
    <div 
      v-if="showEditModal" 
      class="fixed inset-0 z-50 flex items-center justify-center bg-black/50"
      @click.self="showEditModal = false"
    >
      <div :class="['rounded-2xl max-w-5xl w-[90vw] max-h-[90vh] overflow-y-auto shadow-2xl', isDarkMode ? 'bg-[#141414]' : 'bg-white']">
        <EditArtistView 
          :artist="isAddingNewArtist ? undefined : selectedArtist!"
          :is-new="isAddingNewArtist"
          :is-dark-mode="isDarkMode"
          @cancel="showEditModal = false"
          @save="handleSaveArtist"
        />
      </div>
    </div>
    
    <!-- Contract Purchase Modal -->
    <div 
      v-if="showContractModal && selectedContract" 
      class="fixed inset-0 z-50 flex items-center justify-center bg-black/50"
      @click.self="showContractModal = false"
    >
      <div :class="['rounded-2xl max-w-4xl w-[90vw] max-h-[90vh] overflow-y-auto shadow-2xl', isDarkMode ? 'bg-[#141414]' : 'bg-white']">
        <ContractPurchaseView 
          :contract="selectedContract"
          :artist="selectedArtist ?? undefined"
          :is-dark-mode="isDarkMode"
          @back="showContractModal = false"
          @submit="handleContractSubmit"
        />
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import type { ArtistViewType, Artist, AvailableContract } from './types'

// Layout Components
import TopNavbar from './components/layout/TopNavbar.vue'
import ArtistListSidebar from './components/layout/ArtistListSidebar.vue'
import HorizontalTabNav from './components/layout/HorizontalTabNav.vue'

// View Components
import OverviewView from './views/OverviewView.vue'
import ReleasesView from './views/ReleasesView.vue'
import ContributionsView from './views/ContributionsView.vue'
import SmartlinksView from './views/SmartlinksView.vue'
import IntegrationsView from './views/IntegrationsView.vue'
import ContractsView from './views/ContractsView.vue'
import EditArtistView from './views/EditArtistView.vue'
import ContractPurchaseView from './views/ContractPurchaseView.vue'

// Icons
import { IconPlus } from './components/icons'

// Mock Data
import { 
  artists, 
  topSongs, 
  topCountries, 
  releases, 
  contributions, 
  smartlinks, 
  integrations,
  availableContracts,
  userContracts
} from './data/mockData'

// State
const selectedArtist = ref<Artist | null>(artists[0]) // Pre-select first artist
const activeView = ref<ArtistViewType>('overview')
const showEditModal = ref(false)
const isAddingNewArtist = ref(false)
const showContractModal = ref(false)
const selectedContract = ref<AvailableContract | null>(null)
const isDarkMode = ref(false) // Start with light mode

// Methods
const selectArtist = (artist: Artist) => {
  selectedArtist.value = artist
  activeView.value = 'overview'
}

const setActiveView = (view: ArtistViewType) => {
  activeView.value = view
}

const handleAddArtist = () => {
  isAddingNewArtist.value = true
  showEditModal.value = true
}

const handleEditArtist = () => {
  isAddingNewArtist.value = false
  showEditModal.value = true
}

const handleSaveArtist = (data: any) => {
  console.log('Saving artist:', data)
  showEditModal.value = false
}

const handleContractSelect = (contract: AvailableContract) => {
  selectedContract.value = contract
  showContractModal.value = true
}

const handleContractSubmit = (data: any) => {
  console.log('Contract submitted:', data)
  showContractModal.value = false
  // In a real app, this would add to basket and navigate
  alert('Contract added to basket! In a real app, you would be redirected to the basket.')
}
</script>
