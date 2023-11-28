<template>
  
  <v-card>

    <v-img cover height="250" :src="media.Poster"></v-img>

    <v-card-item>
      <v-card-title>{{ media.Title }}</v-card-title>

      <v-card-subtitle>
        <span class="me-1">{{ media.Type }}</span>

        <v-icon color="error" icon="mdi-fire-circle" size="small"></v-icon>
      </v-card-subtitle>
    </v-card-item>

    <v-card-text>
      <v-row align="center" justify="space-between" no-gutters>
        <div class="text-grey">
          {{ media.Year }}
        </div>
        <RatingComponent :rating="media.Rating"/>
      </v-row>

    </v-card-text>

  </v-card>

</template>

<script>
import RatingComponent from './RatingComponent.vue';

export default {
  components: {
    RatingComponent
  },
  props:{
    media: Object
  }
}
</script>
