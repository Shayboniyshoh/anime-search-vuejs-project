<template>
  <div class="app">
    <header>
      <h1 class="heading">Search Your Favorite Anime Here...</h1>
      <form class="search-box" @submit.prevent="HandleSearch">
        <input type="search" class="search-field" placeholder="Search for anime..." required v-model="search_query" />
      </form>
    </header>
    <div class="main">
      <div class="cards" v-if="animeList.length > 0">
        <!-- <CardItem v-for="anime in animeList" :key="anime.mal_id" :anime="anime" /> -->
        <div class="CardItem card" v-for="anime in animeList" :key="anime.mal_id" :anime="anime">
          <a :href="anime.url" class="card_link">
            <img :src="anime.images.jpg.image_url" :alt="anime.title" />
            <div class="card-body">
              <h2>{{ anime.title }}</h2>
            </div>
          </a>
        </div>
      </div>
      <div class="no-relults" v-else>
        <h3>Nothing Found!!!</h3>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue'

export default {
  setup() {
    const search_query = ref('')
    const animeList = ref([])

    const HandleSearch = async () => {
      animeList.value = await fetch(`https://api.jikan.moe/v4/anime?q=${search_query.value}`)
        .then((res) => res.json())
        .then((data) => data.data)
        .catch((err) => console.log(err))
      search_query.value = ''
    }

    return {
      search_query,
      animeList,
      HandleSearch
    }
  }
}
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Fire Sans', sans-serif;
}

.app,
header {
  background: #333;
  margin: 0 !important;
  padding: 1rem;
}

.cards {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
  width: 100%;

  .card {
    display: flex;
    align-items: space-between;
    justify-content: center;
    max-width: 250px;
    width: 100%;
    height: 300px;
    margin: 1rem;
    border-radius: 8px;
    box-shadow: 0 0 10px 1px #000;
  }

  :hover {
    box-shadow: 0 0 15px 3px #000;
    transition: 0.5s ease;
  }

  .card-body {
    display: flex;
    align-items: center;
    justify-content: center;
    background: #000;
    padding: 1rem;
    height: 100px;

    h2 {
      align-self: center;
      font-size: 0.9rem;
      line-height: 1.1rem;
      font-weight: 400;
      text-align: center;
    }
  }

  .card_link {
    width: 100%;
    text-decoration: none;
    color: #fff;
  }

  img {
    width: 100%;
    height: 200px;
    object-fit: cover;
    object-position: bottom;
    border-top-left-radius: 8px;
    border-top-right-radius: 8px;
  }
}

.no-relults h3 {
  color: rgb(255, 143, 143);
  text-align: center;
  text-transform: uppercase;
  letter-spacing: 5px;
  font-weight: 400 !important;
}

form {
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 1rem 0;
}

.heading {
  text-align: center;
  margin: 1rem 0;
  font-size: 1.6rem;
  text-transform: uppercase;
  font-weight: 400;
  color: #fff;
}

.search-field {
  padding: 0.5rem;
  border-radius: 4px;
  margin: 0 auto;
  max-width: 400px;
  width: 100%;
}
</style>
