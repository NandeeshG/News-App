<template>
  <v-app>
    <v-navigation-drawer v-model="drawer" clipped fixed app>
      <v-list nav>
        <v-list-item>
          <v-list-item-title> Change Country to </v-list-item-title>
        </v-list-item>
        <v-divider />
        <v-list-item
          v-for="(item, i) in countries"
          :key="i"
          @click="fetchNews(i, selectedCategory)"
        >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="item.name" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
      <v-footer fixed>
        <v-chip nuxt to="/about"> Made by Nandeesh </v-chip>
      </v-footer>
    </v-navigation-drawer>

    <v-app-bar clipped-left fixed app>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-spacer />
      <v-toolbar-title class="orange--text" v-text="title" />
      <v-spacer />
      <v-icon>{{ countries[selectedCountry].icon }}</v-icon>
    </v-app-bar>

    <v-main>
      <v-container>
        <v-row justify="space-around">
          <v-col cols="8" sm="8" md="8" lg="8">
            <v-chip-group :show-arrows="true">
              <v-chip
                v-for="(cats, id) in categories"
                :key="id"
                large
                outlined
                ripple
                active-class="primary--text"
                @click="fetchNews(selectedCountry, id)"
              >
                {{ cats }}
              </v-chip>
            </v-chip-group>
          </v-col>
        </v-row>
      </v-container>

      <v-card
        v-for="(art, id) in articles"
        :key="id"
        class="mx-auto ma-4"
        max-width="400"
      >
        <v-img
          class="white--text align-end"
          height="200px"
          :src="
            art.urlToImage ||
            'https://www.hotel.de/xx/assets/hotel-de/img/layout/missing-room.png'
          "
        >
        </v-img>

        <v-card-text class="text--primary">
          {{ art.title }}
        </v-card-text>

        <v-card-actions>
          <v-btn color="orange" text :href="art.url" target="_blank">
            Read More
          </v-btn>
        </v-card-actions>
      </v-card>

      <v-snackbar v-model="showError">
        There was some problem... Try again
        <v-btn dark class="ma-2" @click.native="showError = false">Close</v-btn>
      </v-snackbar>
    </v-main>

    <v-footer fixed app>
      <span class="orange--text"> CONNECT AT -> </span>
      <v-spacer />
      <v-btn
        v-for="(sm, id) in social"
        :key="id"
        icon
        :href="sm.url"
        target="_blank"
      >
        <v-icon>{{ sm.icon }}</v-icon>
      </v-btn>
      <v-spacer />
      <span class="orange--text">{{ new Date().toDateString() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
/* eslint-disable no-console */
export default {
  data() {
    return {
      drawer: false,
      title: 'NEWS VIEWS',
      articles: [],
      social: [
        {
          icon: 'mdi-github',
          url: 'https://www.github.com/NandeeshG/News-App',
        },
        {
          icon: 'mdi-linkedin',
          url: 'https://www.linkedin.com/in/nandeesh-gupta-05b43014a/',
        },
        { icon: 'mdi-twitter', url: 'https://www.twitter.com/NewsAPIorg' },
      ],
      selectedCountry: 0,
      countries: [
        { icon: '🌍', name: 'Global', code: '' },
        { icon: '🇮🇳', name: 'India', code: 'in' },
        { icon: '🇯🇵', name: 'Japan', code: 'jp' },
        { icon: '🇺🇸', name: 'USA', code: 'us' },
        { icon: '🇬🇧', name: 'UK', code: 'gb' },
        { icon: '🇫🇷', name: 'France', code: 'fr' },
      ],
      selectedCategory: 0,
      categories: [
        'general',
        'business',
        'entertainment',
        'health',
        'science',
        'sports',
        'technology',
      ],
      showError: false,
    }
  },
  methods: {
    async fetchNews(cnt, cat) {
      this.selectedCountry = cnt
      this.selectedCategory = cat
      const apikey = process.env.apikey
      const url = `/v2/top-headlines?${
        this.selectedCountry === 0
          ? ''
          : `country=${this.countries[this.selectedCountry].code}&`
      }category=${this.categories[this.selectedCategory]}&apiKey=${apikey}`
      // console.log(this.selectedCountry)
      // console.log(this.selectedCategory)
      // console.log(url)
      try {
        const res = await this.$axios.get(url)
        this.articles = res.data.articles
        console.log(this.articles[0])
      } catch (err) {
        this.showError = true
        this.articles = []
      }
    },
  },
  head() {
    const title = `NV | ${this.countries[this.selectedCountry].name}`
    return {
      title,
    }
  },
}
</script>
