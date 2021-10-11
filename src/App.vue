<template>
  <h1>METEO 5 jours / 3 heures</h1>
  <div id="choix">
    <label for="choix">Selectionne la ville de ton choix:</label>
    <input
      id="choix"
      v-model="choixVille"
      type="text"
      @keydown.enter="getForecastByLocationInput"
      placeholder="Choix Ville"
    />
  </div>
  <br />
  <div class="geoloc">
    <button class="localisation" @click="getForecastByLocation">
      Météo par géolocalisation
    </button>
  </div>
  <h2>{{ forecastLocation }}</h2>
  <div class="weatherCard-container">
    <weatherCard
      v-for="(forecast, index) in meteoArray"
      :key="index"
      :icon="forecast.weather[0].icon"
      :dateTime="forecast.dt_txt"
      :temp="forecast.main.temp"
      :description="forecast.weather[0].description"
      :speed="forecast.wind.speed"
    ></weatherCard>
  </div>
</template>

<script>
import weatherCard from "./components/weatherCard.vue";

export default {
  components: {
    weatherCard: weatherCard,
  },

  data() {
    return {
      meteoArray: [],
      forecastLocation: "",
      choixVille: "",
    };
  },

  async mounted() {
    const response = await fetch(
      "https://api.openweathermap.org/data/2.5/forecast?q=Monaco&appid=c23ea174dddff27c2a35fc309bafcc38&units=metric&lang=fr"
    );
    const data = await response.json();
    this.meteoArray = data.list;
    this.forecastLocation = data.city.name;
    console.log(this.meteoArray);
  },
  methods: {
    getForecastByLocation() {
      navigator.geolocation.getCurrentPosition(async (position) => {
        const lat = position.coords.latitude;
        const lng = position.coords.longitude;

        const url = `https://api.openweathermap.org/data/2.5/forecast?lat=${lat}&lon=${lng}&appid=c23ea174dddff27c2a35fc309bafcc38&units=metric&lang=fr`;

        const response = await fetch(url);

        const data = await response.json();

        this.meteoArray = data.list;
        this.forecastLocation = data.city.name;
      });
    },
    async getForecastByLocationInput() {
      let choix = this.choixVille;
      const url = `https://api.openweathermap.org/data/2.5/forecast?q=${choix}&appid=c23ea174dddff27c2a35fc309bafcc38&units=metric&lang=fr`;

      const response = await fetch(url);

      const data = await response.json();

      this.meteoArray = data.list;
      this.forecastLocation = data.city.name;
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Amatic+SC&display=swap");
body {
  background-color: none;
  font-family: "Amatic SC", cursive;
  background-image: url(https://source.unsplash.com/1600x1600/?weather);
  background-size: auto;
}
input {
  font-family: "Amatic SC", cursive;
  border-radius: 3px;
  font-size: 2rem;
  background-color: #c1c4d8;
  color: #fefeff;
}
label {
  font-size: 2rem;
}
button {
  font-family: "Amatic SC", cursive;
  border-radius: 3px;
  font-size: 2rem;
  background-color: #c1c4d8;
  margin-bottom: 1%;
  color: #fefeff;
}
.txt {
  margin: 2px;
}

.weatherCard-container {
  margin: 2vh 0.5vw;
  display: flex;
  flex-direction: row;
  overflow-x: scroll;
  scrollbar-color: blue;
}

.weatherCard {
  margin: 10px;
  background-color: #c1c4d8;
  box-shadow: 4px 5px 8px #444444;
  padding: 10px;
  color: #2b2bc0;
  width: 11vw;
  font-size: 1.5rem;
  display: flex;
  flex-direction: column;
  justify-content: center;
  border-radius: 3px;
}
h1 {
  text-align: center;
  text-decoration: underline;
  font-size: 4rem;
  color: #fefeff;
  margin: 1%;
}

h2 {
  text-align: center;
  text-shadow: 5px 5px 5px #444444;
  color: #fefeff;
  font-size: 2rem;
  border: #fefeff solid 2px;
  margin: 0% 40%;
  border-radius: 5px;
  box-shadow: 5px 5px 5px #fefeff;
}
::-webkit-scrollbar {
  height: 20px;
  border-radius: 5px;
}

/* Track */
::-webkit-scrollbar-track {
  background-color: none;
}

/* Handle */
::-webkit-scrollbar-thumb {
  border: 3px solid #fefeff;
  background: #7f7fd1;
  border-radius: 8px;
}

/* Handle on hover */
::-webkit-scrollbar-thumb:hover {
  background: #4545ca;
}
#choix {
  color: #fefeff;
  margin-bottom: 5px;
}

@media screen and (max-width: 700px) {
  body {
    background-color: none;
    font-family: "Amatic SC", cursive;
    background-image: url(https://source.unsplash.com/3200x1600/?weather);
    background-size: auto;
  }

  #root {
    width: 100%;
  }

  h1 {
    font-size: 2.5rem;
  }

  h2 {
    font-size: 2rem;
    margin: 0% 20%;
  }
  label {
    font-size: 1.9rem;
  }

  button {
    font-size: 2.3rem;
    margin-bottom: 5%;
  }

  input {
    font-size: 1.9rem;
    width: 95%;
  }
  .weatherCard {
    margin: 10px;
    background-color: #c1c4d8;
    box-shadow: 4px 5px 8px #444444;
    padding: 10px;
    color: #2b2bc0;
    width: 35vw;
    font-size: 1rem;
    display: flex;
    flex-direction: column;
    justify-content: center;
    border-radius: 3px;
  }
  .txt {
    margin: 5px;
    font-size: 90%;
  }
}
</style>
