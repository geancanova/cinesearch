<template>

  <v-container fluid>

    <v-row no-gutters>

      <SearchComponent 
        :search-term="searchTerm"
        :loading="loadingResults"
        @update:search-term="searchTerm = $event"
        @new-search="newSearch"
      />

    </v-row>

    <v-row v-if="mediaList.length > 0 && errorMessage === ''">

      <v-col
        v-for="media in mediaList"
        :key="media.imdbID"
        cols="12"
        sm="6"
        md="4"
      >

        <CardComponent 
          :media="media"
          :to="`${media.Type}/${media.imdbID}`"
        />

      </v-col>

      <v-col cols="12" class="text-center">
        <PaginationComponent 
          :cur-page="page"
          :pages="computedPagination"
          @update:page="newSearch(filterType, $event)"
        />
      </v-col>

    </v-row>

    <v-row v-else-if="loadingResults === false && errorMessage === ''">
        <v-col cols="6" class="text-center offset-3">
          <h2 class="text-h4 font-weight-black text-orange mb-4">CineSearch: Explore Movies & Series Database</h2>
          <p class="text-body-1 mb-4">Welcome to <span class="text-orange font-weight-bold">CineSearch</span>, your ultimate destination for discovering comprehensive information about movies and series. Our user-friendly interface and powerful search bar allow you to delve into a vast collection of films and TV shows, unlocking details on plots, casts, ratings, reviews, and much more. Whether you're seeking timeless classics or the latest releases, <span class="text-orange font-weight-bold">CineSearch</span> simplifies your quest for entertainment.</p>
          <p class="text-h6 font-weight-bold">Start exploring and uncover the cinematic world at your fingertips.</p>
        </v-col>
    </v-row>

    <v-row v-else>
      <v-col cols="6" class="text-center offset-3">
        <p class="text-h6">{{ errorMessage }}</p>
      </v-col>
    </v-row>

  </v-container>

</template>

<script>
import SearchComponent from "../components/SearchComponent.vue";
import CardComponent from "../components/CardComponent.vue";
import PaginationComponent from "../components/PaginationComponent.vue";

export default {
  name: 'HomeView',
  components: {
    SearchComponent,
    CardComponent,
    PaginationComponent,
  },
  props: {
    filterType: String
  },
  data() {
    return {
      dataURL: import.meta.env.VITE_API_URL,
      mediaList: [],
      searchTerm: '',
      loadingResults: false,
      errorMessage: '',
      totalResults: 0,
      page: 1
    }
  },
  computed: {
    computedPagination() {
      return Math.ceil(this.totalResults / 10);
    },
    computedFilterType() {
      // if (!this.filterType) {
      //   return;
      // }
      return this.newSearch(this.filterType, 1);
    }
  },
  methods: {
    async fetchMedia(endpoint) {
      this.loadingResults = true;

      try {
        const response = await fetch(endpoint);

        if (!response.ok) throw new Error('Network response was not OK');

        const data = await response.json();

        if (data.Response === 'False') {
          console.log('No results');
          this.errorMessage = data.Error;
          return;
        }

        const items = data.Search;

        this.mediaList = items;
        this.totalResults = +data.totalResults;

        items.forEach(async (media, i) => {
          const response = await fetch(`${this.dataURL}&i=${media.imdbID}`);

          if (!response.ok) throw new Error('Network response was not OK');

          const data = await response.json();
          this.mediaList[i].Rating = data.imdbRating;
        });

        console.log(this.searchTerm);
        console.log(this.totalResults);
      } catch (error) {
        console.error('There has been a problem with your fetch operation:', error);
      } finally {
        this.loadingResults = false;
      }
    },
    resetData() {
      this.mediaList = [];
      this.totalResults = 0;
      this.loadingResults = false;
      this.errorMessage = '';
      this.page = 1;
    },
    newSearch(media = this.filterType, page = this.page) {
      this.resetData();

      if (!this.searchTerm) {
        return;
      }

      this.page = page;

      let endpoint = `${this.dataURL}&s=${this.searchTerm}&page=${page}`;

      if (media) {
        endpoint = `${endpoint}&type=${media}`;
      }

      console.log(media, endpoint)

      this.fetchMedia(endpoint);
      window.scrollTo(0, 0);
    },
  },
  created() {
    console.log('created');
    this.searchTerm = '';
    this.resetData();
  },
  mounted() {
    console.log('mounted');
  },
  updated() {
    console.log('updated');
    this.computedFilterType;
    console.log(this.filterType);
    
  }
}
</script>
