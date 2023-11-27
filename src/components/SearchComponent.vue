<template>

  <v-text-field
    single-line
    hide-details
    clearable
    :loading="loading"
    variant="solo"
    label="Search for Movies or Series"
    append-inner-icon="mdi-magnify"
    :model-value="searchTerm"
    @update:model-value="$emit('update:searchTerm', $event)"
    @click:append-inner="$emit('newSearch')"
    @keydown.enter="$emit('newSearch')"
  />

</template>

<script>
export default {
  props: {
    searchTerm: String,
    isLoading: Boolean
  },
  data: () => ({
    loading: false,
  }),
  updated() {
    if (!this.searchTerm) {
      this.$emit('newSearch');
      console.log('vazio');
    }
  }
}
</script>
