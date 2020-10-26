<template>
  <v-app>
    <v-app-bar fixed app dense color="primary">
      <v-toolbar-title class="white--text">Sensasi RSS</v-toolbar-title>
      <v-spacer />
      <div class="date white--text">{{ date }}</div>
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
