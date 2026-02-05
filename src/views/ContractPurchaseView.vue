<template>
  <div>
    <!-- Step Navigation Bar - Release Builder Style -->
    <div class="border-b border-gray-200 px-6 lg:px-8 py-4">
      <div class="flex items-center">
        <div 
          v-for="(step, index) in steps" 
          :key="step.id"
          class="flex-1 relative"
        >
          <!-- Step indicator -->
          <button
            @click="goToStep(index)"
            :disabled="index > currentStep"
            :class="[
              'flex items-center gap-2 text-sm font-medium transition-colors',
              index <= currentStep ? 'cursor-pointer' : 'cursor-not-allowed'
            ]"
          >
            <span 
              :class="[
                'text-lg font-semibold',
                index < currentStep ? 'text-ditto-purple' : 
                index === currentStep ? 'text-ditto-purple' : 'text-gray-300'
              ]"
            >
              {{ index + 1 }}
            </span>
            <span 
              :class="[
                index < currentStep ? 'text-gray-900' : 
                index === currentStep ? 'text-ditto-purple' : 'text-gray-400'
              ]"
            >
              {{ step.label }}
            </span>
          </button>
          
          <!-- Progress line under step -->
          <div 
            :class="[
              'absolute bottom-[-17px] left-0 right-0 h-[3px] transition-colors',
              index < currentStep ? 'bg-ditto-purple' : 
              index === currentStep ? 'bg-ditto-purple' : 'bg-transparent'
            ]"
          />
        </div>
      </div>
    </div>
    
    <div class="p-6 lg:p-8">
      <!-- Header -->
      <div class="mb-8">
        <h1 class="text-2xl lg:text-3xl font-bold text-gray-900 mb-2">{{ contract.title }}</h1>
        <p class="text-gray-500">{{ contract.description }}</p>
      </div>

    <!-- Step Content -->
    <div class="mb-6">
      <!-- Step 1: Record Label Details -->
      <div v-if="currentStep === 0" class="space-y-6">
        <h2 class="text-xl font-semibold text-gray-900 mb-4">Record Label Information</h2>
        
        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
          <div>
            <label class="block text-xs text-gray-500 mb-1">Record Label Name *</label>
            <input 
              v-model="formData.recordLabel.name"
              type="text"
              class="w-full py-2 text-sm text-gray-900 border-b border-gray-200 focus:border-ditto-purple focus:outline-none transition-colors"
              placeholder="Enter label name"
            />
          </div>
          
          <div>
            <label class="block text-xs text-gray-500 mb-1">Email *</label>
            <input 
              v-model="formData.recordLabel.email"
              type="email"
              class="w-full py-2 text-sm text-gray-900 border-b border-gray-200 focus:border-ditto-purple focus:outline-none transition-colors"
              placeholder="label@email.com"
            />
          </div>
          
          <div class="md:col-span-2">
            <label class="block text-xs text-gray-500 mb-1">Address *</label>
            <input 
              v-model="formData.recordLabel.address"
              type="text"
              class="w-full py-2 text-sm text-gray-900 border-b border-gray-200 focus:border-ditto-purple focus:outline-none transition-colors"
              placeholder="Street address"
            />
          </div>
          
          <div>
            <label class="block text-xs text-gray-500 mb-1">City *</label>
            <input 
              v-model="formData.recordLabel.city"
              type="text"
              class="w-full py-2 text-sm text-gray-900 border-b border-gray-200 focus:border-ditto-purple focus:outline-none transition-colors"
              placeholder="City"
            />
          </div>
          
          <div>
            <label class="block text-xs text-gray-500 mb-1">Country *</label>
            <input 
              v-model="formData.recordLabel.country"
              type="text"
              class="w-full py-2 text-sm text-gray-900 border-b border-gray-200 focus:border-ditto-purple focus:outline-none transition-colors"
              placeholder="Country"
            />
          </div>
          
          <div>
            <label class="block text-xs text-gray-500 mb-1">Postcode *</label>
            <input 
              v-model="formData.recordLabel.postcode"
              type="text"
              class="w-full py-2 text-sm text-gray-900 border-b border-gray-200 focus:border-ditto-purple focus:outline-none transition-colors"
              placeholder="Postcode"
            />
          </div>
          
          <div>
            <label class="block text-xs text-gray-500 mb-1">Phone Number</label>
            <input 
              v-model="formData.recordLabel.phone"
              type="tel"
              class="w-full py-2 text-sm text-gray-900 border-b border-gray-200 focus:border-ditto-purple focus:outline-none transition-colors"
              placeholder="+44 123 456 7890"
            />
          </div>
        </div>
      </div>

      <!-- Step 2: Artist Details -->
      <div v-if="currentStep === 1" class="space-y-6">
        <div class="flex items-center justify-between mb-4">
          <h2 class="text-xl font-semibold text-gray-900">Artist / Band Information</h2>
          <span class="text-sm text-gray-500">{{ formData.artists.length }}/5 artists</span>
        </div>
        
        <!-- Artist Cards -->
        <div 
          v-for="(artist, index) in formData.artists" 
          :key="index"
          class="relative border border-gray-200 rounded-xl p-5 mb-4"
        >
          <!-- Remove button (only if more than 1 artist) -->
          <button
            v-if="formData.artists.length > 1"
            @click="removeArtist(index)"
            class="absolute top-3 right-3 p-1 text-gray-400 hover:text-error transition-colors"
            title="Remove artist"
          >
            <svg class="w-5 h-5" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12" />
            </svg>
          </button>
          
          <h3 class="text-sm font-medium text-gray-700 mb-4">Artist {{ index + 1 }}</h3>
          
          <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
            <div>
              <label class="block text-xs text-gray-500 mb-1">Artist / Band Name *</label>
              <input 
                v-model="artist.name"
                type="text"
                class="w-full py-2 text-sm text-gray-900 border-b border-gray-200 focus:border-ditto-purple focus:outline-none transition-colors"
                placeholder="Artist name"
              />
            </div>
            
            <div>
              <label class="block text-xs text-gray-500 mb-1">Email *</label>
              <input 
                v-model="artist.email"
                type="email"
                class="w-full py-2 text-sm text-gray-900 border-b border-gray-200 focus:border-ditto-purple focus:outline-none transition-colors"
                placeholder="artist@email.com"
              />
            </div>
            
            <div class="md:col-span-2">
              <label class="block text-xs text-gray-500 mb-1">Address *</label>
              <input 
                v-model="artist.address"
                type="text"
                class="w-full py-2 text-sm text-gray-900 border-b border-gray-200 focus:border-ditto-purple focus:outline-none transition-colors"
                placeholder="Street address"
              />
            </div>
            
            <div>
              <label class="block text-xs text-gray-500 mb-1">City *</label>
              <input 
                v-model="artist.city"
                type="text"
                class="w-full py-2 text-sm text-gray-900 border-b border-gray-200 focus:border-ditto-purple focus:outline-none transition-colors"
                placeholder="City"
              />
            </div>
            
            <div>
              <label class="block text-xs text-gray-500 mb-1">Country *</label>
              <input 
                v-model="artist.country"
                type="text"
                class="w-full py-2 text-sm text-gray-900 border-b border-gray-200 focus:border-ditto-purple focus:outline-none transition-colors"
                placeholder="Country"
              />
            </div>
            
            <div>
              <label class="block text-xs text-gray-500 mb-1">Postcode *</label>
              <input 
                v-model="artist.postcode"
                type="text"
                class="w-full py-2 text-sm text-gray-900 border-b border-gray-200 focus:border-ditto-purple focus:outline-none transition-colors"
                placeholder="Postcode"
              />
            </div>
          </div>
        </div>
        
        <!-- Add Artist Button -->
        <button
          v-if="formData.artists.length < 5"
          @click="addArtist"
          class="w-full py-3 border-2 border-dashed border-gray-300 rounded-xl text-sm font-medium text-gray-500 hover:border-ditto-purple hover:text-ditto-purple transition-colors flex items-center justify-center gap-2"
        >
          <svg class="w-5 h-5" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" d="M12 4.5v15m7.5-7.5h-15" />
          </svg>
          Add Another Artist
        </button>
      </div>

      <!-- Step 3: Contract Terms -->
      <div v-if="currentStep === 2" class="space-y-6">
        <h2 class="text-xl font-semibold text-gray-900 mb-4">Contract Terms</h2>
        
        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
          <div>
            <label class="block text-xs text-gray-500 mb-1">Effective Date *</label>
            <input 
              v-model="formData.terms.effectiveDate"
              type="date"
              class="w-full py-2 text-sm text-gray-900 border-b border-gray-200 focus:border-ditto-purple focus:outline-none transition-colors"
            />
          </div>
          
          <div>
            <label class="block text-xs text-gray-500 mb-1">Term (Duration) *</label>
            <input 
              v-model="formData.terms.term"
              type="text"
              class="w-full py-2 text-sm text-gray-900 border-b border-gray-200 focus:border-ditto-purple focus:outline-none transition-colors"
              placeholder="e.g. 2 years"
            />
          </div>
          
          <div>
            <label class="block text-xs text-gray-500 mb-1">Service Start Date *</label>
            <input 
              v-model="formData.terms.serviceStartDate"
              type="date"
              class="w-full py-2 text-sm text-gray-900 border-b border-gray-200 focus:border-ditto-purple focus:outline-none transition-colors"
            />
          </div>
          
          <div>
            <label class="block text-xs text-gray-500 mb-1">Service End Date *</label>
            <input 
              v-model="formData.terms.serviceEndDate"
              type="date"
              class="w-full py-2 text-sm text-gray-900 border-b border-gray-200 focus:border-ditto-purple focus:outline-none transition-colors"
            />
          </div>
          
          <div>
            <label class="block text-xs text-gray-500 mb-1">Number of Full-Length Albums</label>
            <input 
              v-model="formData.terms.albumCount"
              type="number"
              min="1"
              class="w-full py-2 text-sm text-gray-900 border-b border-gray-200 focus:border-ditto-purple focus:outline-none transition-colors"
              placeholder="1"
            />
          </div>
          
          <div>
            <label class="block text-xs text-gray-500 mb-1">Payment Structure *</label>
            <input 
              v-model="formData.terms.paymentStructure"
              type="text"
              class="w-full py-2 text-sm text-gray-900 border-b border-gray-200 focus:border-ditto-purple focus:outline-none transition-colors"
              placeholder="e.g. 50/50 split"
            />
          </div>
          
          <div>
            <label class="block text-xs text-gray-500 mb-1">Cure Period (Days)</label>
            <input 
              v-model="formData.terms.curePeriod"
              type="text"
              class="w-full py-2 text-sm text-gray-900 border-b border-gray-200 focus:border-ditto-purple focus:outline-none transition-colors"
              placeholder="e.g. 30 days"
            />
          </div>
          
          <div>
            <label class="block text-xs text-gray-500 mb-1">Governing Law *</label>
            <select 
              v-model="formData.terms.governingLaw"
              class="w-full py-2 text-sm text-gray-900 border-b border-gray-200 focus:border-ditto-purple focus:outline-none transition-colors bg-transparent"
            >
              <option value="">Select jurisdiction</option>
              <option value="england-wales">England and Wales</option>
              <option value="scotland">Scotland</option>
              <option value="us-california">California, USA</option>
              <option value="us-new-york">New York, USA</option>
            </select>
          </div>
        </div>
      </div>

      <!-- Step 4: Review & Sign -->
      <div v-if="currentStep === 3" class="space-y-6">
        <h2 class="text-xl font-semibold text-gray-900 mb-4">Review & Sign</h2>
        
        <!-- Summary -->
        <div class="bg-gray-50 rounded-lg p-4 space-y-4">
          <h3 class="font-medium text-gray-900">Contract Summary</h3>
          
          <div class="grid grid-cols-2 gap-4 text-sm">
            <div>
              <p class="text-gray-500">Record Label</p>
              <p class="font-medium text-gray-900">{{ formData.recordLabel.name || '—' }}</p>
            </div>
            <div>
              <p class="text-gray-500">Artist(s)</p>
              <p class="font-medium text-gray-900">{{ artistNames || '—' }}</p>
            </div>
            <div>
              <p class="text-gray-500">Effective Date</p>
              <p class="font-medium text-gray-900">{{ formData.terms.effectiveDate || '—' }}</p>
            </div>
            <div>
              <p class="text-gray-500">Term</p>
              <p class="font-medium text-gray-900">{{ formData.terms.term || '—' }}</p>
            </div>
          </div>
        </div>
        
        <!-- Record Label Signature -->
        <div>
          <h3 class="font-medium text-gray-900 mb-3">Record Label Signature</h3>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
            <div>
              <label class="block text-xs text-gray-500 mb-1">Signatory Name *</label>
              <input 
                v-model="formData.signatures.labelSignatory"
                type="text"
                class="w-full py-2 text-sm text-gray-900 border-b border-gray-200 focus:border-ditto-purple focus:outline-none transition-colors"
                placeholder="Full name"
              />
            </div>
            <div>
              <label class="block text-xs text-gray-500 mb-1">Title *</label>
              <input 
                v-model="formData.signatures.labelTitle"
                type="text"
                class="w-full py-2 text-sm text-gray-900 border-b border-gray-200 focus:border-ditto-purple focus:outline-none transition-colors"
                placeholder="e.g. CEO, Manager"
              />
            </div>
            <div>
              <label class="block text-xs text-gray-500 mb-1">Email *</label>
              <input 
                v-model="formData.signatures.labelEmail"
                type="email"
                class="w-full py-2 text-sm text-gray-900 border-b border-gray-200 focus:border-ditto-purple focus:outline-none transition-colors"
                placeholder="signatory@label.com"
              />
            </div>
          </div>
        </div>
        
        <!-- Artist Signature -->
        <div>
          <h3 class="font-medium text-gray-900 mb-3">Artist / Band Signature</h3>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
            <div>
              <label class="block text-xs text-gray-500 mb-1">Signatory Name *</label>
              <input 
                v-model="formData.signatures.artistSignatory"
                type="text"
                class="w-full py-2 text-sm text-gray-900 border-b border-gray-200 focus:border-ditto-purple focus:outline-none transition-colors"
                placeholder="Full name"
              />
            </div>
            <div>
              <label class="block text-xs text-gray-500 mb-1">Title *</label>
              <input 
                v-model="formData.signatures.artistTitle"
                type="text"
                class="w-full py-2 text-sm text-gray-900 border-b border-gray-200 focus:border-ditto-purple focus:outline-none transition-colors"
                placeholder="e.g. Artist"
              />
            </div>
            <div>
              <label class="block text-xs text-gray-500 mb-1">Email *</label>
              <input 
                v-model="formData.signatures.artistEmail"
                type="email"
                class="w-full py-2 text-sm text-gray-900 border-b border-gray-200 focus:border-ditto-purple focus:outline-none transition-colors"
                placeholder="artist@email.com"
              />
            </div>
          </div>
        </div>
        
        <!-- Agreement checkbox -->
        <label class="flex items-start gap-3 cursor-pointer">
          <input 
            v-model="agreedToTerms"
            type="checkbox"
            class="mt-1 w-4 h-4 rounded border-gray-300 text-ditto-purple focus:ring-ditto-purple"
          />
          <span class="text-sm text-gray-600">
            I confirm that all information provided is accurate and I agree to the terms of this contract. 
            The contract will be sent to all signatories for electronic signature.
          </span>
        </label>
      </div>
    </div>

    <!-- Footer Actions -->
    <div class="flex items-center justify-center gap-4 pt-6">
      <button 
        @click="currentStep > 0 ? previousStep() : $emit('back')"
        class="px-8 py-2.5 text-sm font-medium text-gray-600 hover:text-gray-900 transition-colors border border-gray-300 rounded-full"
      >
        Back
      </button>
      
      <button 
        v-if="currentStep < steps.length - 1"
        @click="nextStep"
        class="px-8 py-2.5 bg-ditto-purple text-white text-sm font-medium rounded-full hover:opacity-90 transition-opacity shadow-md shadow-ditto-purple/25"
      >
        Next
      </button>
      
      <button 
        v-else
        @click="submitContract"
        :disabled="!agreedToTerms"
        :class="[
          'px-8 py-2.5 text-sm font-medium rounded-full transition-all shadow-md',
          agreedToTerms 
            ? 'bg-ditto-purple text-white hover:opacity-90 shadow-ditto-purple/25' 
            : 'bg-gray-200 text-gray-400 cursor-not-allowed shadow-none'
        ]"
      >
        Add to Basket
      </button>
    </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, reactive, computed, onMounted } from 'vue'
import type { AvailableContract, Artist } from '../types'

const props = defineProps<{
  contract: AvailableContract
  artist?: Artist
}>()

const emit = defineEmits<{
  (e: 'back'): void
  (e: 'submit', data: typeof formData): void
}>()

const currentStep = ref(0)
const agreedToTerms = ref(false)

const steps = [
  { id: 'label', label: 'Record Label' },
  { id: 'artist', label: 'Artist Info' },
  { id: 'terms', label: 'Contract Terms' },
  { id: 'review', label: 'Review & Sign' }
]

const today = new Date().toISOString().split('T')[0]

interface ArtistEntry {
  name: string
  email: string
  address: string
  city: string
  country: string
  postcode: string
}

const createEmptyArtist = (): ArtistEntry => ({
  name: '',
  email: '',
  address: '',
  city: '',
  country: '',
  postcode: ''
})

const formData = reactive({
  recordLabel: {
    name: '',
    email: '',
    address: '',
    city: '',
    country: '',
    postcode: '',
    phone: ''
  },
  artists: [createEmptyArtist()] as ArtistEntry[],
  terms: {
    effectiveDate: today,
    term: '',
    serviceStartDate: today,
    serviceEndDate: '',
    albumCount: '',
    paymentStructure: '',
    curePeriod: '',
    governingLaw: ''
  },
  signatures: {
    labelSignatory: '',
    labelTitle: '',
    labelEmail: '',
    artistSignatory: '',
    artistTitle: '',
    artistEmail: ''
  }
})

const artistNames = computed(() => {
  return formData.artists
    .map(a => a.name)
    .filter(name => name.trim())
    .join(', ')
})

const addArtist = () => {
  if (formData.artists.length < 5) {
    formData.artists.push(createEmptyArtist())
  }
}

const removeArtist = (index: number) => {
  if (formData.artists.length > 1) {
    formData.artists.splice(index, 1)
  }
}

onMounted(() => {
  // Prepopulate artist info if available
  if (props.artist) {
    formData.artists[0].name = props.artist.name
    formData.signatures.artistSignatory = props.artist.name
    formData.signatures.artistTitle = 'Artist'
  }
})

const nextStep = () => {
  if (currentStep.value < steps.length - 1) {
    currentStep.value++
  }
}

const previousStep = () => {
  if (currentStep.value > 0) {
    currentStep.value--
  }
}

const goToStep = (index: number) => {
  if (index <= currentStep.value) {
    currentStep.value = index
  }
}

const submitContract = () => {
  if (!agreedToTerms.value) return
  emit('submit', formData)
}
</script>
