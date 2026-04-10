<script setup lang="ts">
import { onMounted, ref, computed, watch } from "vue";

import PageHeader from "./components/PageHeader.vue";
import CountryList from "./components/CountryList.vue";
import axiosClient from "./utils/axios";
import { Country } from "./models/country.model";

const countries = ref<Country[]>([]);
const search = ref("");
const page = ref(1);
const itemsPerPage = ref(12);
const isLoading = ref(false);
const error = ref<string | null>(null);

const fetchCountries = async () => {
  isLoading.value = true;
  error.value = null;
  try {
    const { data } = await axiosClient.get("/all?fields=name,flags,region,capital,population");
    console.log("API Response:", data);
    countries.value = data;
    console.log("Countries loaded:", countries.value.length);
  } catch (err) {
    console.error("Error fetching countries:", err);
    error.value = "Failed to fetch countries. Please try again.";
  } finally {
    isLoading.value = false;
  }
};

const filteredCountries = computed(() => {
  const query = search.value.trim().toLowerCase();
  console.log("Search query:", query);
  if (!query) {
    console.log("Returning all countries:", countries.value.length);
    return countries.value;
  }
  const filtered = countries.value.filter((country) =>
    country.name.common.toLowerCase().includes(query) ||
    country.region.toLowerCase().includes(query)
  );
  console.log("Filtered countries:", filtered.length);
  return filtered;
});

const paginatedCountries = computed(() => {
  const start = (page.value - 1) * itemsPerPage.value;
  const end = start + itemsPerPage.value;
  const paginated = filteredCountries.value.slice(start, end);
  console.log("Paginated countries:", paginated.length, "from page", page.value);
  return paginated;
});

const totalPages = computed(() => Math.ceil(filteredCountries.value.length / itemsPerPage.value));

const changePage = (newPage: number) => {
  if (newPage >= 1 && newPage <= totalPages.value) {
    page.value = newPage;
  }
};

watch(search, () => {
  page.value = 1;
});

onMounted(() => {
  fetchCountries();
});
</script>

<template>
  <div class="min-h-screen bg-gray-900 text-white">
    <PageHeader />
    <div class="container max-w-screen-lg mx-auto px-6">
      <!-- Search Input -->
      <div class="mb-8 relative">
        <input
          type="text"
          v-model="search"
          placeholder="Search by country name or region"
          class="w-full p-4 pl-12 rounded-lg bg-gray-800 border border-gray-700 text-white placeholder-gray-400 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent shadow-lg transition-all duration-300"
        />
        <span class="absolute left-4 top-1/2 transform -translate-y-1/2 text-gray-400">🔍</span>
      </div>

      <!-- Loading State -->
      <div v-if="isLoading" class="flex justify-center items-center py-20">
        <div class="animate-spin rounded-full h-12 w-12 border-b-2 border-blue-500"></div>
      </div>

      <!-- Error State -->
      <div v-else-if="error" class="text-center py-20">
        <p class="text-red-400 text-xl">{{ error }}</p>
        <button @click="fetchCountries" class="mt-4 px-6 py-2 bg-blue-600 hover:bg-blue-700 rounded-lg transition-colors">
          Retry
        </button>
      </div>

      <!-- Countries List -->
      <div v-else>
        <!-- Empty State -->
        <div v-if="filteredCountries.length === 0 && search" class="text-center py-20">
          <p class="text-gray-400 text-xl">No countries found matching "{{ search }}"</p>
        </div>

        <!-- Pagination -->
        <div v-if="totalPages > 1" class="mb-8 flex justify-center space-x-4">
          <button
            @click="changePage(page - 1)"
            :disabled="page <= 1"
            class="px-4 py-2 bg-gray-800 hover:bg-gray-700 disabled:opacity-50 disabled:cursor-not-allowed rounded-lg border border-gray-700 transition-colors"
          >
            Previous
          </button>
          <span class="px-4 py-2 text-gray-400">Page {{ page }} of {{ totalPages }}</span>
          <button
            @click="changePage(page + 1)"
            :disabled="page >= totalPages"
            class="px-4 py-2 bg-gray-800 hover:bg-gray-700 disabled:opacity-50 disabled:cursor-not-allowed rounded-lg border border-gray-700 transition-colors"
          >
            Next
          </button>
        </div>

        <CountryList :countries="paginatedCountries" />
      </div>
    </div>
  </div>
</template>
