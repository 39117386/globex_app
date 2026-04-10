<script setup lang="ts">
import { Country } from "../models/country.model";

interface Props {
  country: Country;
}

defineProps<Props>();
</script>

<template>
  <div class="bg-gray-800 rounded-lg overflow-hidden shadow-lg border border-gray-700 hover:border-blue-500 hover:shadow-blue-500/25 transition-all duration-300">
    <div class="h-48 overflow-hidden">
      <img
        class="w-full h-full object-cover"
        :src="country.flags.png"
        :alt="country.flags.alt"
      />
    </div>
    <div class="p-4">
      <h4 class="text-xl font-bold text-blue-400 mb-2">{{ country.name.common }}</h4>
      <p class="text-sm text-gray-300 mb-1">
        <span class="text-purple-400 font-semibold">Capital:</span>
        {{ country.capital?.[0] || 'N/A' }}
      </p>
      <p class="text-sm text-gray-300 mb-1">
        <span class="text-green-400 font-semibold">Population:</span>
        {{ country.population?.toLocaleString() || 'N/A' }}
      </p>
      <p class="text-sm text-gray-300">
        <span class="text-pink-400 font-semibold">Region:</span>
        {{ country.region }}
      </p>
    </div>
  </div>
</template>
