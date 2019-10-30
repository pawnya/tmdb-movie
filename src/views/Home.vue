<template>
  <div class="wrapper">
      <Categories @filterFilm='getMovies' />
      <h2 v-if="loading">
        <p>Choose genre</p>
      </h2>
      <ul v-else-if="movies.length > 0" class="movie-card-grid">
        <movie-card v-for="movie in movies" :key="movie.id" :movie="movie" />
      </ul>
      <h2 v-else>
        <p>Sorry, no movies were found in this genre.</p>
      </h2>
  </div>
</template>

<script>
// @ is an alias to /src
import Categories from '@/components/Categories.vue';
import MovieCard from '@/components/MovieCard.vue';

export default {
  name: 'home',
  components: {
    Categories,
    MovieCard,
  },
  data() {
    return {
      loading: true,
      movies: [],
    };
  },
  methods: {
    getMovies(data) {
      this.movies = data;
      this.loading = false;
    },
  },
};
</script>
<style lang="scss">
body {
  margin: 0;
}
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin: 0;
}
.home {
  display: flex;
  flex-flow: column nowrap;
}
.screen-reader-only {
  height: 1px;
  width: 1px;
  position: absolute;
  left: -100000px;
}
.text-centered {
  text-align: center;
}
.wrapper {
  margin: 0 auto;
  max-width: 1200px;
  @media only screen and (max-width: 799px) {
    max-width: 100%;
    margin: 0;
  }
}
.movie-card-grid {
  list-style: none;
  margin: 0.5rem 0;
  padding: 0;
  display: flex;
  align-items: flex-start;
  flex-wrap: wrap;
}
.navbar {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 1rem;
  background: #F0F0F0;
}
.searchbar {
  width: 300px;
  display: flex;
  align-items: center;
  justify-content: center;
  @media only screen and (max-width: 549px) {
    width: 100%;
    label {
      width: 80%;
    }
  }
}
.searchbar-input {
  padding: 0.5rem 1rem;
  border-radius: 20px;
  font-size: 1rem;
  border: 1px solid gray;
  min-width: 300px;
  @media only screen and (max-width: 549px) {
    min-width: 0;
    width: 100%;
  }
}
.btn {
  padding: 0.5rem 1rem;
  font-size: 1rem;
  border-radius: 20px;
  background: transparent;
  border: none;
}
.btn--green {
  background: #42b983;
  color: white;
  font-weight: bold;
}
.btn--go {
  padding: 0.5rem 2rem;
  margin: 0.3rem 1rem;
}
.image {
  &-card {
    width: calc(33% - 1rem);
    margin: 0.5rem;
    border-radius: 5px;
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.15);
    background: white;
    @media only screen and (max-width: 799px) {
      width: calc(50% - 1rem);
    }
    @media only screen and (max-width: 549px) {
      width: 100%;
      margin: 0.5rem 0;
    }
    &__image {
      border-radius: 5px 5px 0 0;
      width: 100%;
      height: 200px;
      object-fit: cover;
    }
    &__body {
      padding: 0.5rem 1rem 1rem;
    }
  }
  &-title {
    font-weight: bold;
    margin: 0;
  }
  &-owner {
    margin-top: 0;
    font-size: 0.8rem;
  }
  &-title,
  &-owner {
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow: hidden;
  }
  &-date-view-wrapper {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
  &-date,
  &-views {
    margin-bottom: 0;
    font-size: 0.8rem;
  }
}
</style>
