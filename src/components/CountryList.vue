<script setup lang="ts">
import { Country } from "../models/country.model";
import CountryItem from "./CountryItem.vue";

interface Props {
  countries: Country[];
}

defineProps<Props>();
</script>

<template>
  <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6">
    <CountryItem
      v-for="country in countries"
      :key="country.cca3"
      :country="country"
      class="transition-transform duration-300 hover:scale-105"
    />
  </div>
</template>
