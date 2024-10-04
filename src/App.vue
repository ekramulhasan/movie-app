<script setup>
import { ref, onMounted } from 'vue';

const movieData = ref(null);
const searchQuery = ref('');
const apiKey = 'bd5b5ac1'; // Replace with your actual OMDB API key

const fetchMovie = async () => {
  try {
    const response = await fetch(`http://www.omdbapi.com/?apikey=${apiKey}&t=${searchQuery.value}`);
    const data = await response.json();
    if (data.Response === "True") {
      movieData.value = data;
    } else {
      console.error('Movie not found');
      movieData.value = null;
    }
  } catch (error) {
    console.error('Error fetching movie data:', error);
  }
};

onMounted(() => {
  searchQuery.value = 'Tom and Jerry'; // Default movie
  fetchMovie();
});
</script>

<template>
  <div class="movie-app">
    <h1>Movie Search</h1>
    <div class="search-container">
      <input v-model="searchQuery" placeholder="Discover your next favorite movie..." @keyup.enter="fetchMovie">
      <button @click="fetchMovie">
        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none"
          stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
          <circle cx="11" cy="11" r="8"></circle>
          <line x1="21" y1="21" x2="16.65" y2="16.65"></line>
        </svg>
      </button>
    </div>
    <div v-if="movieData" class="movie-card">
      <div class="movie-poster">
        <img :src="movieData.Poster" :alt="movieData.Title">
      </div>
      <div class="movie-details">
        <h2>{{ movieData.Title }} <span>({{ movieData.Year }})</span></h2>
        <div class="movie-meta">
          <span class="rating">
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="currentColor"
              stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <polygon
                points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2">
              </polygon>
            </svg>
            {{ movieData.imdbRating }}/10
          </span>
          <span class="runtime">
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none"
              stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <circle cx="12" cy="12" r="10"></circle>
              <polyline points="12 6 12 12 16 14"></polyline>
            </svg>
            {{ movieData.Runtime }}
          </span>
          <span class="genre">
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none"
              stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <rect x="2" y="2" width="20" height="20" rx="2.18" ry="2.18"></rect>
              <line x1="7" y1="2" x2="7" y2="22"></line>
              <line x1="17" y1="2" x2="17" y2="22"></line>
              <line x1="2" y1="12" x2="22" y2="12"></line>
              <line x1="2" y1="7" x2="7" y2="7"></line>
              <line x1="2" y1="17" x2="7" y2="17"></line>
              <line x1="17" y1="17" x2="22" y2="17"></line>
              <line x1="17" y1="7" x2="22" y2="7"></line>
            </svg>
            {{ movieData.Genre }}
          </span>
        </div>
        <p class="plot">{{ movieData.Plot }}</p>
        <div class="crew">
          <p><strong>Director:</strong> {{ movieData.Director }}</p>
          <p><strong>Cast:</strong> {{ movieData.Actors }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.movie-app {
  max-width: 900px;
  margin: 0 auto;
  padding: 40px 20px;
  font-family: 'Inter', sans-serif;
  background-color: #f8f9fa;
  border-radius: 20px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.05);
}

h1 {
  text-align: center;
  color: #1a202c;
  font-size: 3em;
  font-weight: 700;
  margin-bottom: 30px;
}

.search-container {
  display: flex;
  margin-bottom: 40px;
}

input {
  flex-grow: 1;
  padding: 15px 20px;
  font-size: 16px;
  border: 2px solid #e2e8f0;
  border-radius: 12px 0 0 12px;
  transition: all 0.3s ease;
}

input:focus {
  outline: none;
  border-color: #4299e1;
  box-shadow: 0 0 0 3px rgba(66, 153, 225, 0.5);
}

button {
  padding: 15px 25px;
  background-color: #4299e1;
  color: white;
  border: none;
  border-radius: 0 12px 12px 0;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #3182ce;
}

.movie-card {
  display: flex;
  background-color: white;
  border-radius: 20px;
  overflow: hidden;
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
}

.movie-poster {
  flex: 0 0 350px;
}

.movie-poster img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.movie-details {
  flex: 1;
  padding: 30px;
}

.movie-details h2 {
  color: #1a202c;
  font-size: 2.5em;
  font-weight: 700;
  margin-bottom: 15px;
}

.movie-details h2 span {
  font-weight: 400;
  color: #718096;
}

.movie-meta {
  display: flex;
  justify-content: flex-start;
  gap: 20px;
  margin-bottom: 20px;
  font-size: 1em;
  color: #4a5568;
}

.movie-meta span {
  display: flex;
  align-items: center;
  gap: 5px;
}

.movie-meta svg {
  width: 20px;
  height: 20px;
}

.plot {
  margin-bottom: 20px;
  line-height: 1.8;
  color: #2d3748;
  font-size: 1.1em;
}

.crew p {
  margin: 10px 0;
  color: #2d3748;
  font-size: 1.1em;
}

.crew strong {
  color: #1a202c;
  font-weight: 600;
}
</style>
