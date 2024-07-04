<template>
  <div id="country-list">
    <div v-for="country in countries" :key="country.cca3" class="country-item">
      <div class="image-container">
        <img
          class="country-list-flag"
          :src="country.flags.png"
          :alt="country.name.common + ' Flag'"
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
        <button class="show-map" @click="showMap(country.maps.googleMaps)">
          Show Map
        </button>
        <button class="show-detail" @click="showDetail(country.cca3)">
          Detail
        </button>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    props: {
      countries: {
        type: Array,
        required: true,
      },
    },
    methods: {
      formatDate() {
        const d = new Date()
        let day = d.getDate()
        let month = d.toLocaleString('default', { month: 'long' })
        let year = d.getFullYear()
        let hours = d.getHours()
        let minutes = d.getMinutes().toString().padStart(2, '0')
        return `${day} ${month} ${year}, ${hours}:${minutes}`
      },
      showMap(url) {
        window.open(url, '_blank')
      },
      showDetail(cca3) {
        this.$router.push({ params: { country: cca3 } })
      },
    },
  }
</script>
