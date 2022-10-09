<template>
  <div class="anime">
    <img :src="anime.image" :alt="anime.title"/>
    <h3>{{ anime.title }}</h3>
    <span class="episodes">{{ anime.watched_episodes }} / {{ anime.total_episodes }}</span>
    <button
        v-if="anime.total_episodes !== anime.watched_episodes"
        @click="increaseWatch(anime.id)" class="button">+
    </button>
    <button
        v-if="anime.watched_episodes > 0"
        @click="decreaseWatch(anime.id)" class="button">-
    </button>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "MyAnimePost",
  props: ['anime', 'get_anime'],
  methods: {
    increaseWatch(anime_id) {
      axios.post(`http://127.0.0.1:5000/inc/${anime_id}`)
          .catch(() => console.log(`error inc anime ${anime_id}`))
      this.get_anime()
      // localStorage.setItem('my_anime', JSON.stringify(this.myAnimeList))
    },
    decreaseWatch(anime_id) {
      axios.post(`http://127.0.0.1:5000/dec/${anime_id}`)
          .catch(() => console.log(`error dec anime ${anime_id}`))
      this.get_anime()
      // localStorage.setItem('my_anime', JSON.stringify(this.myAnimeList))
    },
  },
}

</script>


<style lang="scss">
.button {
  appearance: none;
  outline: none;
  border: none;
  cursor: pointer;

  display: block;
  padding: 0.5rem 1rem;
  background-image: linear-gradient(to right, deeppink 50%, darkviolet 50%);
  background-size: 200%;
  color: white;
  font-size: 1.125rem;
  font-weight: bold;
  text-transform: uppercase;
  transition: 0.4s;

  &:hover {
    background-position: right;
  }

  &:first-of-type {
    margin-right: 0.5rem;
  }

  &:last-of-type {
    margin-right: 0;
  }
}

.flex-1 {
  display: block;
  flex: 1 1 0;
}

</style>

<style lang="scss" scoped>
button {
  width: 40px;
  height: 40px;
}

img {
  width: 72px;
  height: 72px;
  object-fit: cover;
  border-radius: 1rem;
  margin-right: 1rem;
}

h3 {
  color: #888;
  font-size: 1.125rem;
  width: 600px;
}

.episodes {
  margin: 0 1rem;
  color: #888;
}

.anime {
  display: flex;
  align-items: center;
  margin-bottom: 1.5rem;
  background-color: #FFF;
  padding: 1rem;
  border-radius: 0.5rem;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);

  img {
    width: 72px;
    height: 72px;
    object-fit: cover;
    border-radius: 1rem;
    margin-right: 1rem;
  }

  h3 {
    color: #888;
    font-size: 1.125rem;
  }

  .episodes {
    margin-right: 1rem;
    color: #888;
  }

  .button {
    &:first-of-type {
      margin-right: 0.5rem;
    }

    &:last-of-type {
      margin-right: 0;
    }
  }

}
</style>