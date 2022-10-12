<template>
  <FindAnime v-bind:get_anime="get_anime" v-bind:user_uuid="user_uuid"/>
  <div>
    <h2>My Anime</h2>
    <h3 v-if="myAnimeList.length===0">Empty :c</h3>
    <div class="my_anime" v-else>
      <template v-for="anime in myAnimeList" :key="anime.title">
        <MyAnimePost v-bind:anime="anime" v-bind:get_anime="get_anime" v-bind:user_uuid="user_uuid"/>
      </template>
    </div>
  </div>
</template>


<script>
import axios from "axios";
import MyAnimePost from "@/components/MyAnimePost";
import FindAnime from "@/components/FindAnime";

export default {
  name: "AnimeList",
  components: {
    FindAnime,
    MyAnimePost
  },
  data() {
    return {
      myAnimeList: [],
      user_uuid: localStorage.getItem('user_uuid') || ""
    }
  },
  methods: {
    get_anime() {
      axios.post('http://127.0.0.1:5000/playlist', {'user_uuid': this.user_uuid})
          .then((response) => {
            if (response.data.playlist !== undefined) {
              this.myAnimeList = response.data.playlist
            }

            if (response.data['user_uuid'] !== undefined){
              this.user_uuid = response.data.user_uuid
              localStorage.setItem('user_uuid', this.user_uuid)
              console.log('saved')
            }

          })
          .catch(() => console.log(`error GET all anime `))
    }
  },
  mounted() {
    this.get_anime()
  }
}

</script>

<style>
.flex{
  display: flex;
}
</style>

<style scoped>
.my_anime {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 1.5rem;
  background-color: #FFF;
  padding: 1rem;
  border-radius: 0.5rem;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

h2 {
  color: #888;
  font-weight: 400;
  margin-bottom: 1.5rem;
}
</style>