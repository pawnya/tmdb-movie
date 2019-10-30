<template>
  <li class="movie-card">
    <img class="movie-card__image" :src="imageUrl" :alt="movie.title">
    <div class="movie-card__body">
      <p class="movie-title">{{ movie.title }}</p>
      <section class="movie-date-view-wrapper">
        <p class="movie-date">{{ movie.release_date | moment }}</p>
        <p class="movie-views">{{ movie.vote_count }}</p>
      </section>
      <p class="movie-overview">{{ movie.overview }}</p>
      <router-link :to="{name: 'detailMovie', params: { id: movie.id }}">Detail</router-link>
    </div>
  </li>
</template>

<script>
import moment from 'moment';

const TRANSPARENT_GIF = 'data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7';

export default {
  name: 'MovieCard',
  props: ['movie'],
  computed: {
    imageUrl() {
      if (this.movie.backdrop_path == null) return TRANSPARENT_GIF;
      return `https://image.tmdb.org/t/p/w500_and_h282_bestv2${this.movie.backdrop_path}`;
    },
  },
  filters: {
    moment(date) {
      return moment(date).format('MMMM Do, YYYY');
    },
  },
};
</script>
<style lang="scss">
.movie-card {
  width: calc(33% - 1rem);
  margin: .5rem;
  border-radius: 5px;
  box-shadow: 0 5px 15px rgba(0, 0, 0, .15);
  background: white;
  @media only screen and (max-width: 1024px) {
    width: calc(50% - 1rem);
  }
  @media only screen and (max-width: 549px) {
    width: 100%;
    margin: .5rem 0;
  }
}
.movie-card__image {
  border-radius: 5px 5px 0 0;
  width: 100%;
  height: 200px;
  object-fit: cover;
}
.movie-card__body {
  margin: .5rem 1rem 1rem;
  text-align: left;
  overflow: hidden;
}
.movie-title {
  font-weight: bold;
  margin: 0;
}
.movie-owner {
  margin-top: 0;
  font-size: .8rem;
}
.movie-date-view-wrapper {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.movie-date,
.movie-views {
  margin-bottom: 0;
  font-size: .8rem;
}
</style>
