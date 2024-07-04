<template>
  <div class="country-info" v-if="country">
    <div class="country-flag">
      <img :src="country.flags.png" alt="Country Flag" />
    </div>
    <div class="country-details">
      <p><strong>Native Name:</strong> {{ nativeName }}</p>
      <p>
        <strong>Capital:</strong>
        {{ country.capital ? country.capital[0] : 'N/A' }}
      </p>
      <p>
        <strong>Population:</strong> {{ country.population.toLocaleString() }}
      </p>
      <p><strong>Region:</strong> {{ country.region }}</p>
      <p><strong>Sub-region:</strong> {{ country.subregion }}</p>
      <p><strong>Area:</strong> {{ country.area }} Km²</p>
      <p>
        <strong>Country Code:</strong>
        {{
          `+${country.idd.root}${
            country.idd.suffixes ? country.idd.suffixes[0] : ''
          }`
        }}
      </p>
      <p><strong>Languages:</strong> {{ languages }}</p>
      <p><strong>Currencies:</strong> {{ currencies }}</p>
      <p><strong>Timezones:</strong> {{ timezones }}</p>
    </div>
  </div>
  <div v-else>
    <p>Loading...</p>
  </div>
</template>

<script>
  export default {
    props: {
      country: {
        type: Object,
        required: true,
      },
    },
    computed: {
      nativeName() {
        return this.country.name.nativeName
          ? Object.values(this.country.name.nativeName)[0].common
          : 'N/A'
      },
      languages() {
        return this.country.languages
          ? Object.values(this.country.languages).join(', ')
          : 'N/A'
      },
      currencies() {
        return this.country.currencies
          ? Object.values(this.country.currencies)
              .map((curr) => curr.name)
              .join(', ')
          : 'N/A'
      },
      timezones() {
        return this.country.timezones
          ? this.country.timezones.join(', ')
          : 'N/A'
      },
    },
  }
</script>
