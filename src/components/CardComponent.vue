<template>
  
  <v-card>

    <v-img cover height="250" :src="poster"></v-img>

    <v-card-item>
      <v-card-title>{{ title }}</v-card-title>

      <v-card-subtitle>
        <span class="me-1">{{ type }}</span>

        <v-icon color="error" icon="mdi-fire-circle" size="small"></v-icon>
      </v-card-subtitle>
    </v-card-item>

    <v-card-text>
      <v-row align="center" justify="space-between" no-gutters>
        <div class="text-grey">
          {{ year }}
        </div>
        <RatingComponent :rating="rating"/>
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
    title: String,
    year: String,
    type: String,
    poster: String,
    rating: String
  }
}
</script>
