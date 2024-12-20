<script>
import axios from 'axios'

export default {
    data() {
        return {
          city: "",
          region: "",
          city1: "", city2: "", city3: "",
          error: "",

          info: null,
          infoCity1: null, infoCity2: null, infoCity3: null,
          infoRegion: null,

          regions: [
            "Алтайский край", "Амурская область", "Архангельская область", "Астраханская область",
            "Белгородская область", "Брянская область", "Владимирская область", "Волгоградская область",
            "Вологодская область", "Воронежская область", "Донецкая Народная Республика", "Еврейская автономная область",
            "Забайкальский край", "Запорожская область", "Ивановская область", "Иркутская область"
          ]
        }
    },
  computed: {
    showTemp() {
      return this.info ? "Температура: " + this.info.main.temp + "°C" : "";
    },
    showFeelsLike() {
      return this.info ? "Ощущается как: " + this.info.main.feels_like + "°C" : "";
    },
    showMinTemp() {
      return this.info ? "Минимальная температура: " + this.info.main.temp_min + "°C" : "";
    },
    showMaxTemp() {
      return this.info ? "Максимальная температура: " + this.info.main.temp_max + "°C" : "";
    },
    showAverageTemp() {
      if (this.infoCity1 && this.infoCity2 && this.infoCity3) {
        const avgTemp = (this.infoCity1.main.temp + this.infoCity2.main.temp + this.infoCity3.main.temp) / 3;
        return "Средняя температура: " + avgTemp.toFixed(1) + "°C";
      }
      return "";
    }
  },
  methods: {
    clearData() {
      this.info = null;
      this.infoRegion = null;
      this.infoCity1 = null;
      this.infoCity2 = null;
      this.infoCity3 = null;
      this.error = "";
    },
    getWeatherByCity() {
      this.clearData();

      if (this.city.trim().length < 2) {
        this.error = "Нужно название более одного символа :)";
        return false;
      }

      axios
          .get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=0ba39c23e0dd0a5b63ccdd6354473356`)
          .then((res) => (this.info = res.data))
          .catch(() => (this.error = "Не удалось получить данные о погоде"));
    },
    getWeatherByRegion() {
      this.clearData();

      if (!this.region) {
        this.error = "Выберите регион";
        return false;
      }

      axios
          .get(`https://api.openweathermap.org/data/2.5/weather?q=${this.region}&units=metric&appid=0ba39c23e0dd0a5b63ccdd6354473356`)
          .then((res) => (this.infoRegion = res.data))
          .catch(() => (this.error = "Не удалось получить данные о погоде"));
    },
    getWeatherByCities() {
      this.clearData();

      if (this.city1.trim().length < 2 || this.city2.trim().length < 2 || this.city3.trim().length < 2) {
        this.error = "Нужно ввести хотя бы два символа для каждого города.";
        return false;
      }

      const cities = [this.city1, this.city2, this.city3];
      let promises = cities.map(city =>
          axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${city}&units=metric&appid=0ba39c23e0dd0a5b63ccdd6354473356`)
      );

      Promise.all(promises)
          .then(responses => {
            this.infoCity1 = responses[0].data;
            this.infoCity2 = responses[1].data;
            this.infoCity3 = responses[2].data;
          })
          .catch(() => (this.error = "Не удалось получить данные о погоде для одного или нескольких городов"));
    }
  }
};
</script>

