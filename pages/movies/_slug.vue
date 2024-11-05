<template>
  <Loading v-if="$fetchState.pending" />
  <div v-else class="container single-movie">
    <NuxtLink class="button" :to="{ name: 'index' }">
      Back
    </NuxtLink>
    <div class="movie-info">
      <div class="movie-img">
        <img :src="'http://image.tmdb.org/t/p/w500/' + movie.poster_path" alt="movie poster">
      </div>
      <div class="movie-content">
        <h1 class="text-white">
          Title: {{ movie.title }}
        </h1>
        <p class="movie-fact tagline">
          <span>Tagline: </span>{{ movie.tagline || 'No Tagline' }}
        </p>
        <p class="movie-fact">
          <span>Released: </span>
          {{
            new Date(movie.release_date).toLocaleString('en-us', {
              month: 'long',
              day: 'numeric',
              year: 'numeric'
            })
          }}
        </p>
        <p class="movie-fact">
          <span>Duration: </span>{{ movie.runtime }} minutes
        </p>
        <p class="movie-fact">
          <span>Revenue: </span>
          {{
            movie.revenue.toLocaleString('en-us', {
              style: 'currency',
              currency: 'USD'
            })
          }}
        </p>
        <p class="movie-fact">
          <span>Overview: </span>{{ movie.overview }}
        </p>
      </div>
    </div>
  </div>
</template>

<script>
// import { ref, onMounted } from 'vue'
import axios from 'axios'
// const movie = ref(null)

export default {
  name: 'SingleMovie',
  asyncData ({ params }) {
    const slug = params.slug // When calling /abc the slug will be "abc"
    return {
      slug,
      movie: ''
    }
  },
  async fetch () {
    await this.getSingleMovie()
  },
  fetchDela: 1000,
  head () {
    return {
      title: this.movie.title,
      meta: [{
        hid: 'description',
        name: 'description',
        content: 'Get all the latest streaming movies in theaters & onine'
      }, {
        hid: 'keywords',
        name: 'keywords',
        content: 'movies, stream, streaming'
      }]
    }
  },
  methods: {
    async getSingleMovie () {
      const req = axios.get(`https://api.themoviedb.org/3/movie/${this.slug}?api_key=74377d285abc67cee17feb954832a6d9&language=en-US`)
      const result = await req
      this.movie = result.data
    }
  }
}
</script>

<style lang="scss" scoped>
.single-movie {
  color: #fff;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 32px 16px;
  .button {
    align-self: flex-start;
    margin-bottom: 32px;
  }
  .movie-info {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 32px;
    color: #fff;
    @media (min-width: 800px) {
      flex-direction: row;
      align-items: flex-start;
    }
    .movie-img {
      img {
        max-height: 500px;
        width: 100%;
        @media (min-width: 800px) {
          max-height: 700px;
          width: initial;
        }
      }
    }
    .movie-content {
      h1 {
        font-size: 56px;
        font-weight: 400;
      }
      .movie-fact {
        margin-top: 12px;
        font-size: 16px;
        line-height: 1.5;
        span {
          font-weight: 600;
          text-decoration: underline;
        }
      }
      .tagline {
        font-style: italic;
        span {
          font-style: normal;
        }
      }
    }
  }
}
</style>
