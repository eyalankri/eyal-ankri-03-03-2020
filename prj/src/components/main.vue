<template>
  <div class="city-weather">
    <div class="row">
      <input
        type="text"
        v-model="citiesByLetters"
        v-on:keyup="getCitiesByLetters"
        placeholder="Enter a city name"
      />

      <div v-on:mouseleave="clearCities">
        <div v-for="day in days" :key="day[0]" @click="fetFiveDaysForcast(day.Key, day.LocalizedName)" class="autocompleteCities">{{day.LocalizedName}}</div>
      </div>
    </div>
    
    <div class="row">
      <h1 style="color:#29b6f6" v-if="cityName">{{cityName}} <i class="material-icons">
              <span @click="favoriteCitiesRemove(cityKey)" v-if="storedCities.includes(cityKey)">remove_circle r</span> 
              <span @click="favoriteCitiesAdd(cityKey)" v-else>add_circle</span>
            </i>
      </h1>
      
    </div>   
   
    <div class="row">
      <div v-for="day in days" :key="day[0]">
        <div class="col s6 m3">
          <div class="weather-box" style>
            {{day.Date}}
            <br />
            {{day.MaxTemp}} - {{day.MinTemp}} °F
            <br />

            
          </div>
        </div>
      </div>
    </div>
  </div>
</template> 

<script>
import axios from "axios";
import moment from "moment";
export default {
  name: "weather",
  data() {
    return {
      progressBar: null,
      cityName: null,
      cityKey: null,
      storedCities: [],
      days: [],
      citiesByLetters: null
    };
  },
  mounted() {
    if (JSON.parse(localStorage.getItem("storedCities")) == null) {
      this.storedCities.push(0);
      return false;
    }

    this.storedCities = JSON.parse(localStorage.getItem("storedCities"));
  },
  methods: {
    clearCities() {
      //this.cities = [];
    },

    getCitiesByLetters() {
      if (this.citiesByLetters.length <= 2) return false; // to save api calls...

      axios
        .get(
          process.env.ROOT_API +
            "locations/v1/cities/autocomplete?apikey=E8nDU2Q6AeEMkfj9AdBfaLAQ0Pgo4yNH&q=" +
            this.citiesByLetters
        )
        .then(res => {
          this.days = res.data;
        })
        .catch(error => {
          //  this.$router.push({
          //  name: "Login"
          //  });
        });
    },

    fetFiveDaysForcast(key, localizedName) {
      this.cityName = localizedName;
      this.cityKey = key;
      axios
        .get(
          process.env.ROOT_API +
            "/forecasts/v1/daily/5day/" +
            key +
            "?apikey=E8nDU2Q6AeEMkfj9AdBfaLAQ0Pgo4yNH"
        )
        .then(res => {
          res.data.DailyForecasts.forEach(el => {         
            el.Date = moment(el.Date).format("DD/MM/YYYY");  
            el.MaxTemp = (el.Temperature.Maximum.Value) ;
            el.MinTemp = (el.Temperature.Minimum.Value) ;
          })
          console.log(res.data);
          //this.cityName = res.
          this.days = res.data.DailyForecasts;
          
        })
        .catch(error => {
          //  this.$router.push({
          //  name: "Login"
          //  });
        });
    },

    favoriteCitiesAdd(key) {
      this.storedCities.push(key);
      localStorage.setItem("storedCities", JSON.stringify(this.storedCities));
    },

    favoriteCitiesRemove(key) {
      let index = this.storedCities.indexOf(key);
      if (index > -1) {
        this.storedCities.splice(index, 1);
      }
      localStorage.setItem("storedCities", JSON.stringify(this.storedCities));
    },

    loadDefaultCity() {
      axios
        .get(
          process.env.ROOT_API +
            "/forecasts/v1/daily/1day/215854?apikey=E8nDU2Q6AeEMkfj9AdBfaLAQ0Pgo4yNH"
        )
        .then(res => {
          console.log(res.data);
          /*
          this.city = null;
          this.street = null;
          this.buildingNumber = null;
          this.projectName = null;

          this.feedback = "נשמר בהצלחה!";
          this.progressBar = null;
          var table = $("#buildings").DataTable();
          table.destroy(); // must destroy before calling again
          this.listBuildings();
          */
        })
        .catch(error => {
          //  this.$router.push({
          //  name: "Login"
          //  });
        });
    }
  }
};
</script>
<style>
.city-weather .weather-box {
  text-align: center;
  color: #ff9800;
  border: 1px solid #29b6f6;
  text-overflow: ellipsis;
  white-space: nowrap;
  overflow: hidden;
  padding: 5px;
  margin-bottom: 10px;
}
.city-weather .autocompleteCities {
  background: #e7eef1;
  border-bottom: 1px solid #d2cdcd;
  cursor: pointer;
}
.city-weather .autocompleteCities:hover {
  background: silver;
}
</style>