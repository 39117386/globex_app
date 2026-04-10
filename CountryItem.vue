<template>
  <div class="country-card">
    <img :src="country.flag" alt="Flag of {{ country.name }}" class="country-flag" />
    <div class="country-details">
      <h2 class="country-name">{{ country.name }}</h2>
      <p class="country-population">Population: {{ country.population | number }}</p>
      <p class="country-area">Area: {{ country.area | number }} km²</p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    country: Object,
  },
};
</script>

<style scoped>
.country-card {
  display: flex;
  align-items: center;
  border: 1px solid #ccc;
  border-radius: 8px;
  padding: 16px;
  margin: 10px;
  transition: box-shadow 0.3s;
}

.country-card:hover {
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.country-flag {
  width: 50px;
  height: auto;
  margin-right: 16px;
}

.country-details {
  flex-grow: 1;
}

.country-name {
  font-size: 1.5em;
  margin: 0;
}

.country-population,
.country-area {
  margin: 5px 0;
}
</style>