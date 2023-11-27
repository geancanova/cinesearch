<template>
  
  <v-layout>
    
    <NavigationComponent @filter-results="filterResults" />

    <v-main>

      <RouterView :filter-type="filterType" />

    </v-main>

  </v-layout>

</template>

<script>
import NavigationComponent from './components/NavigationComponent.vue';

export default {
  components: {
    NavigationComponent,
  },
  data() {
    return {
      filterType: '',
    }
  },
  methods: {
    filterResults(filterType) {
      this.filterType = filterType;
    }
  }
}
</script>
