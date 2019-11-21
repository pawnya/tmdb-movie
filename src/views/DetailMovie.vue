<template>
  <div class="movie-detail">
    <div class="movie-detail-bg">
      <img :src="'https://image.tmdb.org/t/p/original' + movie.backdrop_path" alt="Movie background" class="movie-detail__bg">
    </div>
    <div class="movie-detail-wrap">
      <div class="movie-detail-content">
        <div class="movie-detail-img">
          <img :src="'https://image.tmdb.org/t/p/w300_and_h450_bestv2' + movie.poster_path" alt="Movie poster" class="movie-detail__img">
        </div>
        <div class="movie-detail-info">
          <a href="/" class="movie-detail__back">Go back</a>
          <h1 class="h1 movie-detail__title">{{ movie.title }}</h1>
          <h3 class="h3 movie-detail__tagline">{{ movie.tagline}}</h3>
          <h2 class="h2 movie-detail__subtitle">Overview</h2>
          <p class="movie-detail__text">{{ movie.overview }}</p>
          <h2 class="h2 movie-detail__subtitle">Release date</h2>
          <p class="movie-detail__text">{{ movie.release_date | moment }}</p>
          <h2 class="h2 movie-detail__subtitle">Rating</h2>
          <div class="movie-detail-rate">
            <span class="movie-detail__rate"></span>
            <span class="movie-detail__rate-text">{{ movie.vote_average }}</span>
          </div>
        </div>
      </div>
      <button class="movie-detail__btn" @click.prevent="showVideo">Show videos</button>
      <div class="movie-detail-add" v-if="media.length > 0">
        <ul class="movie-detail-tab">
          <li v-for="video in media" :key="video.id" class="movie-detail-item">
            <button class="movie-detail__btn movie-detail__show"
              :class="{ active: media[0].key == video.key }"
              @click.prevent="switchVideo(video.key, $event)">{{ video.name }}</button>
          </li>
        </ul>
        <div class="movie-detail-iframe">
          <iframe class="movie-detail__video" width="100%" height="100%" :src="'https://www.youtube-nocookie.com/embed/' + activeMovie" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import moment from 'moment'
import config from '@/config.js'

export default {
  name: 'movieDetail',
  props: {
    id: Number
  },
  data () {
    return {
      movie: [],
      media: [],
      activeMovie: ''
    }
  },
  created () {
    this.getMovie()
  },
  filters: {
    moment (date) {
      return moment(date).format('MMMM Do, YYYY')
    }
  },
  methods: {
    getMovie () {
      axios.get(`https://api.themoviedb.org/3/movie/${this.id}?api_key=${config.api_key}`)
        .then((response) => {
          this.movie = response.data
          this.setRate()
        })
        .catch(error => {
          console.log('Error occured: ', error)
        })
    },
    setRate () {
      const rate = this.movie.vote_average
      const rateStick = document.querySelector('.movie-detail__rate')
      rateStick.style.left = `${rate * 10}%`
    },
    showVideo () {
      axios.get(`https://api.themoviedb.org/3/movie/${this.id}/videos?api_key=${config.api_key}`)
        .then((response) => {
          this.media = response.data.results
          this.activeMovie = response.data.results[0].key
        })
        .catch(error => {
          console.log('Error occured: ', error)
        })
    },
    switchVideo (key, el) {
      this.activeMovie = key
      const btnSwitch = document.querySelectorAll('.movie-detail__show')
      const sectionScroll = document.querySelector('.movie-detail').offsetHeight
      btnSwitch.forEach((btn) => {
        btn.classList.remove('active')
      })
      el.target.classList.add('active')

      window.scrollTo({
        top: sectionScroll,
        behavior: 'smooth'
      })
    }
  }
}
</script>

<style lang="scss">
.movie-detail {
  position: relative;
  margin-bottom: 50px;
  display: -webkit-flex;
  display: -ms-flex;
  display: flex;
  flex-flow: column nowrap;
  width: 100%;
  height: 100%;

  &-bg {
    position: absolute;
    z-index: -1;
    width: 100%;
    height: 500px;

    @media screen and (max-width: 991px) {
      height: 350px;
    }
  }

  &__bg {
    width: 100%;
    height: 100%;
    object-fit: cover;
    object-position: center;
  }

  &-wrap {
    margin-top: 400px;
    margin-right: auto;
    margin-left: auto;
    display: -webkit-flex;
    display: -ms-flex;
    display: flex;
    flex-flow: column nowrap;
    max-width: 1024px;
    width: 100%;

    @media screen and (max-width: 991px) {
      margin-top: 250px;
    }

    @media screen and (max-width: 767px) {
      margin-top: 40px;
    }
  }
  &-content {
    display: -webkit-flex;
    display: -ms-flex;
    display: flex;
    flex-flow: row wrap;
    align-items: flex-start;

    @media screen and (max-width: 767px) {
      flex-flow: column nowrap;
      align-items: center;
    }
  }

  &-img {
    max-width: 300px;
    width: 100%;
    height: 450px;
  }

  &__img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  &-info {
    position: relative;
    margin-left: 50px;
    padding: 40px;
    display: -webkit-flex;
    display: -ms-flex;
    display: flex;
    flex: 1;
    flex-flow: column nowrap;
    align-items: flex-start;
    text-align: left;
    background: linear-gradient(to right, rgb(241, 241, 241), rgba(0, 0, 0, 0));

    @media screen and (max-width: 991px) {
      margin-left: 25px;
    }

    @media screen and (max-width: 767px) {
      margin-left: 0;
    }
  }

  &__title {
    margin: 0;
    margin-bottom: 15px;
    font-size: 45px;
  }

  &__tagline {
    margin: 0;
    font-style: italic;
    font-size: 12px;
  }

  &__subtitle {
    margin: 20px 0px 15px;
  }

  &__text {
    margin: 0;
  }

  &-rate {
    position: relative;
    display: -webkit-flex;
    display: -ms-flex;
    display: flex;
    flex-flow: row nowrap;
    align-items: center;
    width: 50%;
    height: 30px;
    background: linear-gradient(to right, rgb(255, 0, 0), rgb(0, 255, 0));
  }

  &__rate {
    position: absolute;
    left: 0;
    top: 0;
    width: 3px;
    height: 100%;
    background: #000;
    transition: all 1s ease-in-out;

    &-text {
      position: absolute;
      left: calc(100% + 20px);
      font-weight: bold;
      font-style: 20px;
    }
  }

  &__video {
    width: 100%;
    height: 350px;
  }

  &-tab {
    margin: 15px 0;
    padding: 0;
    display: -webkit-flex;
    display: -ms-flex;
    display: flex;
    flex-flow: row wrap;
  }

  &-item {
    list-style: none;
  }

  &__btn {
    margin: 15px 0;
    color: #fff;
    text-transform: uppercase;
    word-wrap: break-word;
    white-space: normal;
    cursor: pointer;
    border: 0;
    border-radius: .125rem;
    box-shadow: 0 2px 5px 0 rgba(0,0,0,.16),0 2px 10px 0 rgba(0,0,0,.12);
    transition: color .15s ease-in-out,
      background-color .15s ease-in-out,
      border-color .15s ease-in-out,
      box-shadow .15s ease-in-out;
    padding: .84rem 2.14rem;
    font-size: .81rem;
    background-color: #323a4e;
    box-shadow: 0 5px 11px 0 rgba(0,0,0,.18), 0 4px 15px 0 rgba(0,0,0,.15);

    &:active,
    &:focus,
    &:hover {
      outline: 0;
      box-shadow: 0 5px 11px 0 rgba(0,0,0,.18),0 4px 15px 0 rgba(0,0,0,.15)
    }
  }

  &__show {
    margin: .375rem;
    background-color: #4285f4;

    &.active {
      background-color: #3f51b5;
    }
  }

  &-iframe {
    position: relative;
    padding-top: 25px;
    padding-bottom: 56.25%;
    height: 0;

    iframe {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
    }
  }

  &__back {
    position: absolute;
    top: 20px;
    right: 30px;
    font-size: 1rem;
    font-weight: 700;
    line-height: 1;
    color: #000;
    text-decoration: none;
  }
}
</style>
