<template>
  <v-container>
    <v-tabs v-model="tab" :show-arrows="false" fixed-tabs @change="fetchNews">
      <v-tab>Latest</v-tab>
      <v-tab>Malaysia</v-tab>
      <v-tab>World</v-tab>
    </v-tabs>
    <v-list
      three-line
      v-touch="{ left: () => goRight(), right: () => goLeft() }"
    >
      <template v-for="(item, index) in news">
        <v-list-item :key="index" @click.stop="displayInfo(item)">
          <v-list-item-content>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
            <v-list-item-subtitle>{{ item.description }}</v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>
        <v-divider :key="`divider ${index}`"></v-divider>
      </template>
    </v-list>
    <v-dialog v-model="openModal" max-width="500px">
      <v-card>
        <v-card-title>
          <div class="newsTitle">Summary</div>
        </v-card-title>
        <v-card-text>
          <v-row>
            <v-col>
              <div class="subtitle-2">{{ display.title }}</div>
            </v-col>
          </v-row>
          <v-divider></v-divider>
          <v-row>
            <v-col>
              <div class="body-1">
                {{ display.description }}
              </div>
            </v-col>
          </v-row>
          <!-- <v-divider></v-divider> -->
        </v-card-text>
        <v-card-actions>
          <v-btn block color="primary" :href="display.link" target="_blank">
            READ FULL
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script lang="ts">
import Vue from 'vue'
import axios from 'axios'
export default Vue.extend({
  name: 'IndexPage',
  async asyncData() {
    const { data } = await axios.get(
      'https://sensasi-api.izzatfaris.site/latest'
    )
    const news = data.rss.channel.item
    return { news }
  },
  data() {
    return {
      news: [],
      tab: 0,
      openModal: false,
      display: {},
    }
  },
  methods: {
    async fetchNews() {
      let data: any
      switch (this.tab) {
        case 0:
          // Fetch latest
          data = await axios.get('https://sensasi-api.izzatfaris.site/latest')
          break
        case 1:
          // Fetch Malaysia
          data = await axios.get('https://sensasi-api.izzatfaris.site/malaysia')
          break
        case 2:
          // Fetch International
          data = await axios.get('https://sensasi-api.izzatfaris.site/world')
          break

        default:
          break
      }
      const news = data!.data.rss.channel.item

      this.news = news
    },
    displayInfo(item: any) {
      this.openModal = true
      this.display = item
    },
    async goRight() {
      if (this.tab === 2) return
      this.tab += 1
      await this.fetchNews()
    },
    async goLeft() {
      if (this.tab === 0) return
      this.tab -= 1
      await this.fetchNews()
    },
  },
})
</script>

<style lang="scss" scoped>
.v-divider {
  // border: 1px solid #9d65c9;
  border-color: #9d65c9;
}
.newsTitle {
  font-size: 1em;
}
</style>
