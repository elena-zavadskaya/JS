<template>
  <div class="top-250-movies">
    <h1>Топ-250 фильмов IMDb</h1>

    <div class="controls">
      <input
        type="text"
        v-model="searchQuery"
        placeholder="Поиск фильмов по названию, году или рейтингу..."
      />
    </div>
    <div class="controls">
      <button @click="sortMovies('title')">Сортировать по названию</button>
      <button @click="sortMovies('rating')">Сортировать по рейтингу</button>
      <button @click="sortMovies('year')">Сортировать по году</button>
    </div>

    <div class="container">
      <div
        v-for="movie in filteredMovies"
        :key="movie.id"
        class="movie-card"
      >
        <img :src="movie.image" :alt="movie.title" />
        <div class="movie-title">{{ movie.title }}</div>
        <div class="movie-details">
          <div class="rating">Рейтинг: {{ movie.imDbRating || 'N/A' }}</div>
          <div class="year">Год: {{ movie.year || 'N/A' }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      movies: [],
      searchQuery: "",
      isAscending: true,
    };
  },
  computed: {
    filteredMovies() {
      return this.movies.filter((movie) => {
        return (
          movie.title.toLowerCase().includes(this.searchQuery.toLowerCase()) ||
          movie.year.toString().includes(this.searchQuery) ||
          (movie.imDbRating && movie.imDbRating.includes(this.searchQuery))
        );
      });
    },
  },
  methods: {
    async fetchMovies() {
      const apiKey = "k_zcuw1ytf";
      const url = `https://tv-api.com/api/Top250Movies/${apiKey}?lang=ru`;

      try {
        const response = await fetch(url);
        const data = await response.json();

        if (!data.items || data.items.length === 0) {
          console.error("Нет данных для отображения.");
          return;
        }

        this.movies = data.items;
      } catch (error) {
        console.error("Ошибка загрузки данных:", error);
      }
    },
    sortMovies(field) {
      this.isAscending = !this.isAscending;

      this.movies.sort((a, b) => {
        let valA, valB;
        if (field === "title") {
          valA = a.title.toLowerCase();
          valB = b.title.toLowerCase();
        } else if (field === "rating") {
          valA = parseFloat(a.imDbRating) || 0;
          valB = parseFloat(b.imDbRating) || 0;
        } else if (field === "year") {
          valA = parseInt(a.year) || 0;
          valB = parseInt(b.year) || 0;
        }

        return this.isAscending ? (valA > valB ? 1 : -1) : (valA < valB ? 1 : -1);
      });
    },
  },
  mounted() {
    this.fetchMovies();
  },
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Montserrat:wght@300;600&display=swap");

* {
  margin: 0;
  padding: 0;
}

.top-250-movies {
  font-family: "Montserrat", sans-serif;
  width: 100%;
  height: 100vh;
  background: linear-gradient(339deg, rgba(51, 21, 176, 1) 0%, rgba(253, 187, 45, 1) 100%);
  text-align: center;
  padding: 20px;
}

.controls {
  margin-bottom: 20px;
}

.controls input {
  padding: 10px;
  width: 80%;
  margin-bottom: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.controls button {
  margin: 5px;
  padding: 10px 20px;
  border: none;
  background-color: #ffca00;
  color: white;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.controls button:hover {
  background-color: #bfa130;
}

.container {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  justify-content: center;
}

.movie-card {
  width: 200px;
  background-color: #fff;
  border: 1px solid #ccc;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  text-align: center;
  padding: 10px;
  transition: transform 0.3s ease-in-out;
}

.movie-card:hover {
  transform: translateY(-10px);
}

.movie-card img {
  width: 100%;
  height: auto;
  border-radius: 4px;
}

.movie-title {
  font-size: 18px;
  font-weight: bold;
  margin: 10px 0;
}

.movie-details {
  font-size: 14px;
  color: #666;
}

.rating {
  background-color: #ffca00;
  color: #fff;
  padding: 5px 10px;
  border-radius: 4px;
  margin-top: 10px;
  font-weight: bold;
}

.year {
  margin-top: 5px;
}
</style>