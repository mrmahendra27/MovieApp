<template>
  <Loading v-if="$fetchState.pending" />
  <div v-else class="container single-movie">
    <nuxt-link class="button" :to="{ name: 'index' }">Back</nuxt-link>
    <div class="movie-info">
      <div class="movie-img">
        <img
          :src="`https://image.tmdb.org/t/p/w500${movie.poster_path}`"
          :alt="movie.title"
        />
      </div>
      <div class="movie-content">
        <h1>{{ movie.title }}</h1>
        <div class="movie-fact tagline">
          <span>Tags: </span>"{{ movie.tagline }}"
        </div>
        <div class="movie-fact">
          <span>Released: </span>
          {{
            new Date(movie.release_date).toLocaleString('en-in', {
              month: 'long',
              day: 'numeric',
              year: 'numeric',
            })
          }}
        </div>
        <div class="movie-fact">
          <span>Duration: </span> {{ movie.runtime }} minutes
        </div>
        <div class="movie-fact">
          <span>Rating: </span> {{ movie.vote_average }} stars
        </div>
        <div class="movie-fact">
          <span>Budget: </span> 
          ${{
            movie.budget.toLocaleString('en-us', {
              state: 'currency',
              currency: 'USD',
            })
          }}
        </div>
        <div class="movie-fact">
          <span>Revenue: </span>
          ${{
            movie.revenue.toLocaleString('en-us', {
              state: 'currency',
              currency: 'USD',
            })
          }}
        </div>
        <div class="movie-fact">
          <span>Overview: </span> {{ movie.overview }}
        </div>
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
      movie: "",
    }
  },
  async fetch() {
    await this.getSingleMovie()
  },
   head() {
    return {
      title: this.movie.title,
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: 'Get all the latest streaming movies in theaters & online.',
        },
        {
          hid: 'keywords',
          name: 'keywords',
          content: 'Movies, Streams',
        },
      ],
    }
  },
  methods: {
    async getSingleMovie() {
      const data = await axios.get(
        `https://api.themoviedb.org/3/movie/${this.$route.params.movieid}?api_key=4636cc86f8f58701312b3c0f10d59a8a&language=en-US`
      )
      this.movie = data.data
    },
  },
}
</script>

<style lang="scss">
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