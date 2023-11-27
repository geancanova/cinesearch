<template>

  <v-pagination
    :model-value="curPage"
    :total-visible="5"
    :length="pages"
    @update:model-value="$emit('update:page', $event)"
  ></v-pagination>
  
</template>
<script>
export default {
  props: {
    curPage: Number,
    pages: Number,
  }
}
</script>
