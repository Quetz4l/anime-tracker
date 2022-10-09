<template>
  <form @submit.prevent="searchAnimeByName">
    <input type="text" placeholder="Найти аниме..." v-model="searchQuery"/>
    <button type="submit" class="button">Search</button>
  </form>

  <div class="result_wrapper" v-if="searchResults.length > 0 && !closeResults">
    <div class="results">
      <div v-for="anime in searchResults" :key="anime.mal_id" class="result">
        <img :src="anime.images.jpg.image_url" :alt="anime.title"/>
        <div class="details">
          <h3>{{ anime.title }}</h3>
          <ul>
            <li><b>Год:</b> {{ anime.aired.prop.from.year }}</li>
            <li><b>Серий:</b> {{ anime.episodes }}</li>
            <li><b>Рейтинг:</b> {{ anime.score }}</li>
          </ul>
          <p :title="anime.synopsis" v-if="anime.synopsis">{{ anime.synopsis.slice(0, 250) }}...</p>
          <span class="flex-1"></span>
          <button @click="addAnime(anime)" class="button">Add to My Anime</button>
        </div>
      </div>
    </div>
    <button class="button closeResults" @click="closeResults = true">Close</button>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "FindAnime",
  data() {
    return {
      searchQuery: '',
      closeResults: false,
      searchResults: [],
      // searchResults: localStorage.getItem('find'),
    }
  },
  props: ['get_anime'],
  methods: {

    addAnime(anime) {
      // let my_anime = JSON.parse(localStorage.getItem('my_anime')) || [];
      //   localStorage.setItem('my_anime', JSON.stringify(my_anime))
      let new_anime = {
        id: anime.mal_id,
        title: anime.title,
      }
      this.post_anime(new_anime)
      this.get_anime()
    },

    searchAnimeByName() {
      axios.get('https://api.jikan.moe/v4/anime', {params: {q: this.searchQuery}})
          .then((response) => {
            this.searchResults = response.data.data
          })
          .catch((e) => console.log(e))
    },

    post_anime(anime) {
      axios.post('http://127.0.0.1:5000/anime', anime)
          .catch(() => console.log(`error post anime ${anime.id}`))
    }
  },
}
</script>

<style scoped lang="scss">
ul {
  margin-bottom: 5px;

  li {
    text-align: left;
    list-style-type: none;

  }
}

form {
  display: flex;
  max-width: 480px;
  margin: 0 auto 1.5rem;

  input {
    appearance: none;
    outline: none;
    border: none;
    background: white;

    display: block;
    color: #888;
    font-size: 1.125rem;
    padding: 0.5rem 1rem;
    width: 100%;
  }
}

.result_wrapper {
  .results {
    background-color: #fff;
    border-radius: 0.5rem;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    max-height: 775px;
    overflow-y: scroll;
  }

  .closeResults {
    background-image: linear-gradient(to right, #ea114d 50%, darkviolet 50%);
    margin-top: 40px;
    margin-bottom: 70px;
    width: 100%;
  }

  .result {
    display: flex;
    margin: 1rem;
    padding: 1rem;
    border: 1px solid #ccc;
    border-radius: 5px;
    transition: 0.4s;

    img {
      width: 130px;
      border-radius: 1rem;
      margin-right: 1rem;
    }


    .details {
      flex: 1 1 0;
      display: flex;
      align-items: flex-start;
      flex-direction: column;
      text-align: left;

      h3 {
        font-size: 1.25rem;
        margin-bottom: 0.5rem;
        text-align: center;
        width: 100%;
      }

      p {
        font-size: 0.875rem;
        margin-bottom: 1rem;
      }

      .button {
        margin-left: auto;
      }
    }
  }
}


</style>