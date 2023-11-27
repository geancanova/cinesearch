<template>
    <v-navigation-drawer
      rail
      permanent
    >
      <div class="d-flex flex-column fill-height">

        <v-list-item
          nav
          prepend-avatar="https://randomuser.me/api/portraits/women/75.jpg"
        ></v-list-item>
  
        <v-divider />
  
        <v-list
          density="compact"
          nav
        >
          <v-list-item 
            v-for="item in navItems"
            :key="item.value"
            :prepend-icon="item.icon" 
            :value="item.value" 
            :to="item.type === 'link' ? item.value : null"
            @click="item.type === 'filter' ? $emit('filterResults', item.value) : null"
          >
            <v-tooltip
              activator="parent"
              location="end"
            >{{ item.title }}</v-tooltip>
          </v-list-item>
  
        </v-list>
  
        <v-divider class="mt-auto" />
  
        <v-list-item prepend-icon="mdi-cog" value="settings" to="/settings">
          <v-tooltip
            activator="parent"
            location="end"
          >Settings</v-tooltip>
        </v-list-item>
        
      </div>

    </v-navigation-drawer>
</template>
<script>
export default {
    data() {
      return {
        navItems: [
          { 
            title: 'Home', 
            value: '/',
            icon: 'mdi-home',
            type: 'link' 
          },
          { 
            title: 'Movies & Series',
            value: '',
            icon: 'mdi-video',
            type: 'filter'
          },
          { 
            title: 'Movies',
            value: 'movie',
            icon: 'mdi-movie-open',
            type: 'filter'
          },
          { 
            title: 'TV Series', 
            value: 'series',
            icon: 'mdi-television',
            type: 'filter'
          }
        ]
      }
    },
}
</script>
