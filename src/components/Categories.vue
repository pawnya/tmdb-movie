<template>
  <div class="genres">
    <ul class="genres-list">
      <li v-for="genre in genres" :key="genre.id">
        <button
          type="button"
          class="btn btn--green btn--go"
          @click.prevent="searchForGenre(genre.id)"
        >{{ genre.name }}</button>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios';
import config from '../../config';

export default {
  name: 'categories',
  data() {
    return {
      genres: [],
      films: [],
      page: 1,
      totalPages: null,
      scrollAllow: true,
      activeGenre: '',
    };
  },
  created() {
    window.addEventListener('scroll', this.scrollFilm);
  },
  mounted() {
    this.fetchGenres()
      .then((response) => {
        this.genres = response.data.genres;
      });
  },
  methods: {
    fetchGenres() {
      return axios.get(`https://api.themoviedb.org/3/genre/movie/list?api_key=${config.api_key}`);
    },
    fetchPopular() {
      return axios.get(`https://api.themoviedb.org/3/movie/popular?page=${this.page}&api_key=${config.api_key}`);
    },
    searchForGenre(id) {
      if (this.activeGenre !== id) {
        this.films = [];
        this.page = 1;
      }
      this.activeGenre = id;
      this.getTotalPages();
      this.fetchPopular()
        .then((response) => {
          response.data.results.forEach((film) => {
            film.genre_ids.filter((filmId) => {
              if (filmId === id) {
                this.films.push(film);
              }
              return true;
            });
            return this.$emit('filterFilm', this.films);
          });
          this.scrollAllow = true;
        });
    },
    getTotalPages() {
      if (this.totalPages == null) {
        this.fetchPopular()
          .then((response) => {
            this.totalPages = response.data.total_pages;
          });
      }
      return false;
    },
    scrollFilm() {
      if (this.scrollAllow) {
        const windowHeight = window.innerHeight + window.pageYOffset;
        const viewPortHeight = Math.round(document.body.getBoundingClientRect().height) - 100;
        if (windowHeight > viewPortHeight) {
          this.pageInc();
          this.scrollAllow = false;
          this.searchForGenre(this.activeGenre);
        }
      }
    },
    pageInc() {
      if (this.page <= this.totalPages) {
        this.page += 1;
      }
      return true;
    },
  },
};
</script>
<style lang="scss">
.genres {
  ul {
    padding: 0;
    display: flex;
    flex-flow: row wrap;
    justify-content: center;
    width: 100%;

    li {
      list-style: none;
    }
  }
}
</style>
