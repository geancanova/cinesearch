<template>
  <v-rating 
    :model-value="computedRating" 
    color="amber" 
    density="compact" 
    half-increments 
    readonly 
    size="small"
  />
</template>
<script>
export default {
  props: {
    rating: String
  },
  computed: {
    // a computed getter
    computedRating() {
      // `this` points to the component instance
      return this.rating / 2
    }
  }
}
</script>
