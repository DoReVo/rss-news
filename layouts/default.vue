<template>
  <v-app>
    <v-app-bar fixed app dense color="primary">
      <v-toolbar-title class="white--text">
        RSS News <v-divider vertical></v-divider>
        <div class="date">{{ date }}</div>
      </v-toolbar-title>
      <v-spacer />
    </v-app-bar>
    <v-content>
      <v-container>
        <nuxt />
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
export default {
  computed: {
    date() {
      const date = new Date()

      return date.toDateString()
    },
  },
}
</script>

<style lang="scss" scoped>
.date {
  display: inline;
  font-size: 1rem;
}
</style>
