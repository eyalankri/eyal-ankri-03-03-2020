<template>
  <div class="city-weather row">
    <div v-for="city in cities" :key="city[0]">
      <div class="col s6 m3">
        <div class="weather-box" style>
          {{city.LocalizedName}}
          <br />
          {{city.Key}}
          <br />

          <i class="material-icons">
            <span @click="removeCityToFavorit(city.Key)" v-if="storedCities.includes(city.Key)" >remove_circle</span>
            <span @click="saveCityToFavorit(city.Key)" v-else>add_circle</span>
          </i>
        </div>
      </div>
    </div>
  </div>
</template> 

<script>
import axios from "axios";
export default {
  name: "weather",
  data() {
    return {
      progressBar: null,
      storedCities: [],
      cities: [
        {
          Version: 1,
          Key: "215854",
          Type: "City",
          Rank: 31,
          LocalizedName: "Tel Aviv",
          Country: {
            ID: "IL",
            LocalizedName: "Israel"
          },
          AdministrativeArea: {
            ID: "TA",
            LocalizedName: "Tel Aviv"
          }
        },
        {
          Version: 1,
          Key: "3431644",
          Type: "City",
          Rank: 45,
          LocalizedName: "Telanaipura",
          Country: {
            ID: "ID",
            LocalizedName: "Indonesia"
          },
          AdministrativeArea: {
            ID: "JA",
            LocalizedName: "Jambi"
          }
        },
        {
          Version: 1,
          Key: "300558",
          Type: "City",
          Rank: 45,
          LocalizedName: "Telok Blangah New Town",
          Country: {
            ID: "SG",
            LocalizedName: "Singapore"
          },
          AdministrativeArea: {
            ID: "05",
            LocalizedName: "South West"
          }
        },
        {
          Version: 1,
          Key: "325876",
          Type: "City",
          Rank: 51,
          LocalizedName: "Telford",
          Country: {
            ID: "GB",
            LocalizedName: "United Kingdom"
          },
          AdministrativeArea: {
            ID: "TFW",
            LocalizedName: "Telford and Wrekin"
          }
        },
        {
          Version: 1,
          Key: "169072",
          Type: "City",
          Rank: 51,
          LocalizedName: "Telavi",
          Country: {
            ID: "GE",
            LocalizedName: "Georgia"
          },
          AdministrativeArea: {
            ID: "KA",
            LocalizedName: "Kakheti"
          }
        },
        {
          Version: 1,
          Key: "230611",
          Type: "City",
          Rank: 51,
          LocalizedName: "Telsiai",
          Country: {
            ID: "LT",
            LocalizedName: "Lithuania"
          },
          AdministrativeArea: {
            ID: "TE",
            LocalizedName: "Telšiai"
          }
        },
        {
          Version: 1,
          Key: "2723742",
          Type: "City",
          Rank: 55,
          LocalizedName: "Telégrafo",
          Country: {
            ID: "BR",
            LocalizedName: "Brazil"
          },
          AdministrativeArea: {
            ID: "PA",
            LocalizedName: "Pará"
          }
        },
        {
          Version: 1,
          Key: "186933",
          Type: "City",
          Rank: 55,
          LocalizedName: "Tela",
          Country: {
            ID: "HN",
            LocalizedName: "Honduras"
          },
          AdministrativeArea: {
            ID: "AT",
            LocalizedName: "Atlántida"
          }
        },
        {
          Version: 1,
          Key: "3453754",
          Type: "City",
          Rank: 55,
          LocalizedName: "Telaga Asih",
          Country: {
            ID: "ID",
            LocalizedName: "Indonesia"
          },
          AdministrativeArea: {
            ID: "JB",
            LocalizedName: "West Java"
          }
        },
        {
          Version: 1,
          Key: "3453755",
          Type: "City",
          Rank: 55,
          LocalizedName: "Telagamurni",
          Country: {
            ID: "ID",
            LocalizedName: "Indonesia"
          },
          AdministrativeArea: {
            ID: "JB",
            LocalizedName: "West Java"
          }
        }
      ]
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
    saveCityToFavorit(key) {
      this.storedCities.push(key);
      localStorage.setItem("storedCities", JSON.stringify(this.storedCities));
    },
    removeCityToFavorit(key) {
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
</style>