<template>
  <div class="anime">
    <img :src="anime.image" :alt="anime.title"/>
    <h3>{{ anime.title }}</h3>
    <div class="flex">
      <span class="episodes">{{ anime.watched_episodes }} / {{ anime.total_episodes }}</span>
      <button
          v-if="anime.total_episodes !== anime.watched_episodes"
          @click="changeEpisode(anime.id, 1)" class="button">+
      </button>
      <button
          v-if="anime.watched_episodes > 0"
          @click="changeEpisode(anime.id, -1)" class="button">-
      </button>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "MyAnimePost",
  props: ['anime', 'get_anime', 'user_uuid'],
  methods: {
    changeEpisode(anime_id, count) {
      axios.post(`http://127.0.0.1:5000/episode/${count}` , {user_uuid: this.user_uuid, source: 'jikan', external_anime_id: anime_id})
          .then(()=> this.get_anime())
          .catch(() => console.log(`error episode change count: ${count} for anime_id: ${anime_id}`))
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
.anime {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1.5rem;
  background-color: #FFF;
  padding: 1rem;
  border-radius: 0.5rem;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  width: 750px;

  img {
    width: 100px;
    height: 100px;
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