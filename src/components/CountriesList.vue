
  
  <script setup>
  import { ref, onMounted, onUnmounted } from "vue";
  import Spinner from "./Spinner.vue";
  
  let countries = ref(null);

  const fetchCountries = async () => {
    
    let response = await fetch(
      "https://ih-countries-api.herokuapp.com/countries"
    );
    
    let apiCleanup = await response.json();
    
    let sortedCountries = apiCleanup.sort((paisA, paisB) =>
      paisA.name.common.localeCompare(paisB.name.common)
    );
    console.log(sortedCountries);
  
    
    countries.value = sortedCountries;
  };
  fetchCountries();
  
  
  </script>
  
  <template>
    <h1 class="text-center">Countries Liste</h1>
    <div class="container" v-if="countries">
      <div class="row">
       
        <div class="col-sm-4">
          <ul class="list-group py-4">

            
            <RouterLink
              :to="`/list/${country.alpha3Code}`"
              v-for="(country, index) in countries"
              :key="index"
            >
              <div class="">
                <li
                  class="list-group-item d-flex flex-column justify-content-center align-items-center"
                >
                  <img
                    :src="`https://flagpedia.net/data/flags/icon/72x54/${country.alpha2Code.toLowerCase()}.png`"
                    alt=""
                    class="mb-2 country-flag"
                  />
                  <p>{{ country.name.common }}</p>
                </li>
              </div>
            </RouterLink>
          </ul>
        </div>
        
        <div class="col-sm-8"><RouterView /></div>
      </div>
    </div>
    <div v-else class="col-12"><Spinner text="Loading Countries..." /></div>
  </template>
  
  <style scoped>
  .country-flag {
    width: 125px;
  }
  </style>