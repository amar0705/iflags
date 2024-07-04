<template>
  <div class="container">
    <h1>Country List</h1>
    <div class="form">
      <input
        type="search"
        v-model="searchQuery"
        placeholder="Search country by name..."
      />
      <button @click="filterCountries">Search</button>
    </div>
    <div id="country-list">
      <div
        v-for="country in filteredCountries"
        :key="country.cca3"
        class="country-item"
      >
        <div class="image-container">
          <img
            class="country-list-flag"
            :src="country.flags.png"
            :alt="`${country.name.common} Flag`"
          />
        </div>
        <div>
          <h2 class="custom-line-height">{{ country.name.common }}</h2>
          <p>
            Currency:
            {{
              country.currencies
                ? Object.values(country.currencies)[0].name
                : 'N/A'
            }}
          </p>
          <p>Current Time & Date: {{ formatDate() }}</p>
          <button @click="showMap(country.maps.googleMaps)">Show Map</button>
          <button @click="showDetail(country.cca3)">Detail</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
  import { ref, onMounted } from 'vue'
  import { useRouter } from 'vue-router'

  const router = useRouter()

  const countries = ref([])
  const searchQuery = ref('')
  const filteredCountries = ref([])

  const filterCountries = () => {
    const query = searchQuery.value.toLowerCase()
    filteredCountries.value = countries.value.filter((country) =>
      country.name.common.toLowerCase().includes(query),
    )
  }

  const showMap = (url) => {
    window.open(url, '_blank')
  }

  const showDetail = (cca3) => {
    router.push({ path: `/country/${cca3}` })
  }

  const formatDate = () => {
    const d = new Date()
    const day = d.getDate()
    const month = d.toLocaleString('default', { month: 'long' })
    const year = d.getFullYear()
    const hours = d.getHours()
    const minutes = d.getMinutes().toString().padStart(2, '0')
    return `${day} ${month} ${year}, ${hours}:${minutes}`
  }

  onMounted(async () => {
    const { data } = await useFetch('https://restcountries.com/v3.1/all')
    countries.value = data.value
    filteredCountries.value = data.value
  })
</script>

<style scoped>
  .country-item {
    border: 1px solid #ccc;
    border-radius: 4px;
    padding: 10px;
    margin: 10px;
    text-align: left;
    display: flex;
    flex-direction: row;
    gap: 20px;
  }

  body {
    font-family: Arial, sans-serif;
  }

  .container {
    width: 50%;
    margin: 0 auto;
    text-align: center;
  }

  .country-info {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    margin-top: 20px;
  }

  .country-flag {
    flex: 1;
    min-width: 300px;
    text-align: center;
  }

  .country-flag img {
    width: 100%;
    max-width: 300px;
  }

  .country-details {
    flex: 2;
    min-width: 300px;
    text-align: left;
    padding-left: 20px;
  }

  .country-details p {
    font-size: 16px;
    margin: 5px 0;
  }

  .neighbour-countries {
    display: flex;
    border: 1px solid black;
    flex-wrap: wrap;
    justify-content: center;
    margin-top: 20px;
  }

  .neighbour-countries img {
    width: 30%;
    margin: 10px;
    cursor: pointer;
    height: 250px;
  }

  .country-list-flag {
    width: 100%;
  }

  .image-container {
    width: 280px;
  }

  .custom-line-height {
    line-height: 10px;
  }

  button {
    border: 3px solid blue;
    border-radius: 4px;
    padding: 10px 30px;
    color: blue;
    background-color: white;
    cursor: pointer;
    font-size: 18px;
  }

  button:hover {
    background-color: aliceblue;
  }

  .form {
    color: #555;
    display: flex;
    padding: 2px;
    border: 1px solid currentColor;
    border-radius: 5px;
    margin: 0 0 30px;
  }

  input[type='search'] {
    border: none;
    background: transparent;
    margin: 0;
    padding: 7px 8px;
    font-size: 14px;
    color: inherit;
    border: 1px solid transparent;
    border-radius: inherit;
    display: flex;
    flex: 1 0 auto;
  }

  input[type='search']::placeholder {
    color: #bbb;
  }

  button[type='submit'] {
    text-indent: -999px;
    overflow: hidden;
    width: 40px;
    padding: 0;
    margin: 0;
    border: 1px solid transparent;
    border-radius: inherit;
    background: transparent
      url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='16' height='16' class='bi bi-search' viewBox='0 0 16 16'%3E%3Cpath d='M11.742 10.344a6.5 6.5 0 1 0-1.397 1.398h-.001c.03.04.062.078.098.115l3.85 3.85a1 1 0 0 0 1.415-1.414l-3.85-3.85a1.007 1.007 0 0 0-.115-.1zM12 6.5a5.5 5.5 0 1 1-11 0 5.5 5.5 0 0 1 11 0z'%3E%3C/path%3E%3C/svg%3E")
      no-repeat center;
    cursor: pointer;
    opacity: 0.7;
  }

  button[type='submit']:hover {
    opacity: 1;
  }

  button[type='submit']:focus,
  input[type='search']:focus {
    box-shadow: 0 0 3px 0 #1183d6;
    border-color: #1183d6;
    outline: none;
  }
</style>
