<script setup lang="ts">
import { Country } from "../models/country.model";

interface Props {
  country: Country;
}

const props = defineProps<Props>();
</script>

<template>
  <article
    class="group overflow-hidden rounded-xl border border-white/10 bg-slate-800/80 shadow-lg transition-all duration-300 hover:scale-105 hover:shadow-2xl hover:shadow-black/30"
  >
    <div class="aspect-[16/9] max-h-44 overflow-hidden rounded-t-xl">
      <img
        class="h-full w-full object-cover transition-transform duration-500 group-hover:scale-[1.03]"
        :src="props.country.flags.png"
        :alt="props.country.flags.alt || `Flag of ${props.country.name.common}`"
      />
    </div>
    <div class="space-y-4 p-5">
      <h4 class="font-sans text-xl font-bold tracking-tight text-white">
        {{ props.country.name.common }}
      </h4>

      <div class="space-y-2 text-sm text-gray-400">
        <p class="flex items-center justify-between gap-3">
          <span class="text-gray-500">Population</span>
          <span class="font-medium text-gray-300">
            {{ props.country.population?.toLocaleString() || "N/A" }}
          </span>
        </p>
        <p class="flex items-center justify-between gap-3">
          <span class="text-gray-500">Continent</span>
          <span class="font-medium text-gray-300">
            {{ props.country.region || "N/A" }}
          </span>
        </p>
        <p class="flex items-center justify-between gap-3">
          <span class="text-gray-500">Capital</span>
          <span class="font-medium text-gray-300">
            {{ props.country.capital?.[0] || "N/A" }}
          </span>
        </p>
      </div>
    </div>
  </article>
</template>
