<template>
  <div class="container">
    <h1 id="country-name">{{ country?.name.common }} Detail</h1>
    <CountryDetail :country="country" v-if="country" />
    <h2>Neighbour Countries</h2>
    <div id="neighbour-countries" class="neighbour-countries">
      <img
        v-for="neighbour in neighbours"
        :key="neighbour?.cca3"
        :src="neighbour?.flags?.png"
        :alt="neighbour?.name?.common"
        :title="neighbour?.name?.common"
        @click="goToNeighbour(neighbour)"
      />
    </div>
  </div>
</template>

<script setup>
  import { ref, onMounted } from 'vue'
  import { useRoute, useRouter } from 'vue-router'
  import CountryDetail from '~/components/CountryDetail.vue'

  const route = useRoute()
  const router = useRouter()

  const country = ref(null)
  const neighbours = ref([])

  const goToNeighbour = (neighbour) => {
    router.push({ path: `/country/${neighbour.cca3}` })
  }

  onMounted(async () => {
    const cca3 = route.params.country
    const data = await $fetch(`https://restcountries.com/v3.1/alpha/${cca3}`)
    country.value = data[0]

    const neighbourCodes = country.value.borders || []
    const neighbourPromises = neighbourCodes.map((code) =>
      $fetch(`https://restcountries.com/v3.1/alpha/${code}`),
    )

    const response = await Promise.all(neighbourPromises)
    neighbours.value = response.map((res) => res[0])
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
