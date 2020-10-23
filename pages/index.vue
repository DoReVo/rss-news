<template>
  <v-list three-line>
    <v-list-item
      v-for="(item, index) in news"
      :key="index"
      :href="item.link"
      target="_blank"
    >
      <v-list-item-content>
        <v-list-item-title>{{ item.title }}</v-list-item-title>
        <v-list-item-subtitle>{{ item.description }}</v-list-item-subtitle>
      </v-list-item-content>
    </v-list-item>
  </v-list>
</template>

<script lang="ts">
import Vue from 'vue'
import axios from 'axios'
export default Vue.extend({
  name: 'IndexPage',
  async asyncData() {
    const { data } = await axios.get(
      'https://api.sensasi.izzatfaris.site/latest'
    )
    const news = data.rss.channel.item
    return { news }
  },
  data() {
    return {
      news: [],
    }
  },
})
</script>
