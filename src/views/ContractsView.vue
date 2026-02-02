<template>
  <div class="p-6 lg:p-8 space-y-8">
    <!-- Hero Banner -->
    <div class="relative bg-gradient-to-r from-[#141414] to-ditto-purple rounded-xl p-6 lg:p-8 overflow-hidden">
      <div class="relative z-10 max-w-lg">
        <h1 class="text-2xl lg:text-3xl font-bold text-white mb-2">Music Agreements Made Easy</h1>
        <p class="text-white/80 text-sm lg:text-base">
          Purchase contracts and licenses to protect your music, simplify collaborations, and secure your rights.
        </p>
      </div>
      <!-- Hero Image -->
      <div class="absolute -right-4 top-1/2 -translate-y-1/2 hidden lg:block">
        <img 
          src="/images/contracts-heroimage.webp" 
          alt="Contracts" 
          class="h-48 w-auto"
        />
      </div>
    </div>
    
    <!-- Available Contracts -->
    <section>
      <h2 class="text-xl font-semibold text-gray-900 mb-4">Available Contracts</h2>
      <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4">
        <ContractTypeCard 
          v-for="contract in availableContracts" 
          :key="contract.id"
          :contract="contract"
          :is-dark-mode="isDarkMode"
          @select="handleContractSelect"
        />
      </div>
    </section>
    
    <!-- Your Contracts -->
    <section>
      <h2 class="text-xl font-semibold text-gray-900 mb-4">Your Contracts</h2>
      
      <div class="bg-white rounded-xl overflow-hidden">
        <!-- Table Header -->
        <div class="hidden lg:grid grid-cols-12 gap-4 px-6 py-3 bg-gray-50 text-xs font-medium text-gray-500 uppercase tracking-wider">
          <div class="col-span-5">Name</div>
          <div class="col-span-2">Status</div>
          <div class="col-span-3">Date Created</div>
          <div class="col-span-2">Actions</div>
        </div>
        
        <!-- Table Body -->
        <div class="divide-y divide-gray-100">
          <div 
            v-for="contract in userContracts" 
            :key="contract.id"
            class="grid grid-cols-1 lg:grid-cols-12 gap-2 lg:gap-4 px-4 lg:px-6 py-4 hover:bg-gray-50 transition-colors"
          >
            <!-- Name -->
            <div class="lg:col-span-5 flex items-center">
              <span class="font-medium text-gray-900">{{ contract.name }}</span>
            </div>
            
            <!-- Status -->
            <div class="lg:col-span-2 flex items-center">
              <span 
                :class="[
                  'px-2 py-1 text-xs font-medium rounded-full',
                  getStatusClass(contract.status)
                ]"
              >
                {{ contract.status }}
              </span>
            </div>
            
            <!-- Date Created -->
            <div class="lg:col-span-3 flex items-center">
              <span class="text-sm text-gray-500">{{ contract.dateCreated }}</span>
            </div>
            
            <!-- Actions -->
            <div class="lg:col-span-2 flex items-center">
              <button 
                v-if="contract.canResend"
                class="flex items-center gap-2 px-3 py-1.5 border border-gray-300 rounded-full text-sm text-gray-600 hover:border-ditto-purple hover:text-ditto-purple transition-colors"
              >
                <IconMail class="w-4 h-4" />
                Resend to Email
              </button>
            </div>
          </div>
        </div>
        
        <!-- Empty State -->
        <div v-if="userContracts.length === 0" class="px-6 py-12 text-center">
          <p class="text-gray-500">You don't have any contracts yet</p>
        </div>
      </div>
    </section>
  </div>
</template>

<script setup lang="ts">
import type { AvailableContract, UserContract, ContractStatus } from '../types'
import ContractTypeCard from '../components/common/ContractTypeCard.vue'
import { IconMail } from '../components/icons'

defineProps<{
  availableContracts: AvailableContract[]
  userContracts: UserContract[]
  isDarkMode?: boolean
}>()

const emit = defineEmits<{
  (e: 'select-contract', contract: AvailableContract): void
}>()

const handleContractSelect = (contract: AvailableContract) => {
  emit('select-contract', contract)
}

const getStatusClass = (status: ContractStatus): string => {
  switch (status) {
    case 'Ready':
      return 'bg-success/20 text-success'
    case 'Processing':
      return 'bg-warning/20 text-warning'
    case 'Draft':
      return 'bg-ditto-light-grey text-ditto-subtext'
    default:
      return 'bg-ditto-light-grey text-ditto-subtext'
  }
}
</script>
