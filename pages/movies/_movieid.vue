<template>
  <Loading v-if="$fetchState.pending" />
  <div v-else class="container single-movie">
    <NuxtLink class="button" :to="{ name: 'index' }">Regresar</NuxtLink>
    <div class="movie-info">
      <div class="movie-img">
        <img
          :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`"
          alt=""
        />
      </div>
      <div class="movie-content">
        <h1>Título: {{ movie.title }}</h1>
        <p class="movie-fact tagline"><span>Tema: </span>{{ movie.tagline }}</p>
        <p class="movie-fact">
          <span>Publicada: </span>
          {{
            new Date(movie.release_date).toLocaleString('es-es', {
              month: 'long',
              day: 'numeric',
              year: 'numeric',
            })
          }}
        </p>
        <p class="movie-fact">
          <span>Duración: </span>{{ movie.runtime }} minutes
        </p>
        <p class="movie-fact">
          <span>Renovar:</span>
          {{
            movie.revenue.toLocaleString('es-es', {
              style: 'currency',
              currency: 'PEN',
            })
          }}
        </p>
        <p class="movie-fact"><span>Overview:</span>{{ movie.overview }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'SingleMovie',

  data() {
    return {
      movie: '',
    }
  },

  async fetch() {
    await this.getSingleMovie()
  },
  fetchDelay: 1000,
  head() {
    return {
      title: this.movie.title,
    }
  },
  methods: {
    async getSingleMovie() {
      const data = axios.get(
        `https://api.themoviedb.org/3/movie/${this.$route.params.movieid}?api_key=37ed43a4f8eaa2abd75f9283692947bc&language=en-US`
      )
      const result = await data
      this.movie = result.data
    },
  },
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
        font-size: 20px;
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
