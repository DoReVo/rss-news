<template>
  <v-container>
    <v-tabs v-model="tab" hide-slider grow @change="fetchNews">
      <v-tab>Latest</v-tab>
      <v-tab>Malaysia</v-tab>
      <v-tab>International</v-tab>
    </v-tabs>
    <v-list three-line>
      <template v-for="(item, index) in news">
        <v-list-item :key="index" :href="item.link" target="_blank">
          <v-list-item-content>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
            <v-list-item-subtitle>{{ item.description }}</v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>
        <v-divider :key="`divider ${index}`"></v-divider>
      </template>
    </v-list>
  </v-container>
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
      tab: 0,
    }
  },
  methods: {
    async fetchNews() {
      let data
      switch (this.tab) {
        case 0:
          // Fetch latest
          data = await axios.get('https://api.sensasi.izzatfaris.site/latest')
          break
        case 1:
          // Fetch Malaysia
          data = await axios.get('https://api.sensasi.izzatfaris.site/malaysia')
          break
        case 2:
          // Fetch International
          data = await axios.get('https://api.sensasi.izzatfaris.site/world')
          break

        default:
          break
      }
      const news = data!.data.rss.channel.item

      this.news = news
    },
  },
})
</script>

<style lang="scss" scoped>
.v-divider {
  // border-color: #201548;
  border: 1px solid #9d65c9;
  border-radius: 10px;
}
</style>
