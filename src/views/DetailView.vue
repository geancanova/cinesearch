<template>

  <v-container fluid>

    <v-row no-gutters>

      <v-btn icon="mdi-arrow-left" @click="$router.go(-1)"></v-btn>

    </v-row>

    <v-row class="mb-4">

        <v-col cols="12" md="3">

          <v-card>

            <v-img
              :src="media.Poster"
              max-height="650"
              cover
            ></v-img>

          </v-card>

        </v-col>

        <v-col cols="12" md="6">

          <h2 class="text-h2 font-weight-medium">{{ media.Title }}</h2>

          <div class="movie-info d-flex align-center py-2">
            <div class="text-grey">
              {{ media.Year }}
            </div>
            <div class="text-grey">
              {{ media.Runtime }}
            </div>
            <RatingComponent :rating="media.imdbRating"/>
          </div>

          <p class="mb-4">
            {{ media.Plot }}
          </p>

          <div>
            <div>Country: <span class="text-grey">{{ media.Country }}</span></div>
            <div>Genres: <span class="text-grey">{{ media.Genre }}</span></div>
            <div>Cast: <span class="text-grey">{{ media.Actors }}</span></div>
            <div>Director: <span class="text-grey">{{ media.Director }}</span></div>
          </div>

        </v-col>

      </v-row>

      <v-divider class="mb-7"></v-divider>
      
      <div 
        v-if="mediaType === 'series'"
        class="d-flex"
      >

        <v-tabs
          v-model="tab"
          direction="vertical"
          class="flex-grow-0 flex-shrink-0"
        >
          <v-tab 
            v-for="s in seasons"
            :key="s"
            :value="s" 
            class="rounded-s bg-black"
            selected-class="bg-grey-darken-4"
            @click="fetchEpisodes(s)"
          >
            Season {{ s }}
          </v-tab>
        </v-tabs>
        
        <v-window 
          v-model="tab"
          class="flex-grow-1 flex-shrink-0 bg-grey-darken-4 rounded rounded-s-0"
        >
          <v-window-item
            v-for="s in seasons"
            :key="s"
            :value="s"
            fullscreen
          >
            <v-container fluid>

              <div
                v-for="e in episodes"
                :key="e.Episode"
                class="d-flex justify-start align-center mb-5"
              >
                <v-img
                  :src="media.Poster"
                  :width="300"
                  max-height="150"
                  cover
                  class="flex-grow-0 rounded mr-5"
                />

                <div class="flex-grow-1">
                  <h3 class="text-h5">
                    {{ e.Episode }}. {{ e.Title }}
                  </h3>
                </div>

              </div>

            </v-container>
          </v-window-item>
        </v-window>
        
      </div>

  </v-container>

</template>

<script>
import RatingComponent from '../components/RatingComponent.vue';

export default {
  components: {
    RatingComponent
  },
  data: () => ({
    dataURL: import.meta.env.VITE_API_URL,
    media: {},
    tab: null,
    mediaType: '',
    seasons: null,
    episodes: []
  }),
  methods: {
    async fetchMedia(query = `i=${this.$route.params.id}&plot=full`) {
      try {
        const response = await fetch(`${this.dataURL}&${query}`);

        if (!response.ok) {
          throw new Error('Network response was not OK');
        }

        const data = await response.json();

        this.media = data;
        this.mediaType = data.Type;

        if (this.mediaType === 'series') {
          this.seasons = +data.totalSeasons;
        }
      } catch (error) {
        console.error('There has been a problem with your fetch operation:', error);
      }
    },
    async fetchEpisodes(season) {
      this.episodes = [];
      try {
        const response = await fetch(`${this.dataURL}&i=${this.$route.params.id}&season=${season}`);

        if (!response.ok) {
          throw new Error('Network response was not OK');
        }

        const data = await response.json();
        this.episodes = data.Episodes
        console.log(data);
        console.log(this.episodes);

      } catch (error) {
        console.error('There has been a problem with your fetch operation:', error);
      }
    }

  },
  mounted() {
    this.fetchMedia().then(() => {
      if (this.mediaType === 'series') {
        this.fetchEpisodes(this.tab);
      }
    })
  }
}
</script>
<style scoped lang="scss">
  .movie-info {
    *:not(:last-child) {
      &:after {
        content: '•';
        margin: 0 10px;
      }
    }
  }
</style>
