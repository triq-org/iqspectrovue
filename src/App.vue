<template>
  <v-app>
    <div id="dropBg" class="dropBg"></div>

    <v-app-bar app hide-on-scroll>
      <v-toolbar-title class="headline text-uppercase">
        <span>I/Q </span>
        <span class="font-weight-light hidden-xs-only">Spectrogram</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-toolbar-items>
        <v-switch
          class="my-5"
          :label="themeDark ? 'Dark Theme' : 'Light Theme'"
          v-model="themeDark"
          @change="$vuetify.theme.dark=themeDark"
        ></v-switch>
      </v-toolbar-items>
      <v-menu
        class="ma-2 hidden-sm-and-down"
        bottom left
      >
        <template v-slot:activator="{ on }">
          <v-btn v-on="on" text>triq.org</v-btn>
        </template>
        <v-list>
          <v-list-item v-for="item in links" :key="item.text">
            <v-btn text :href="item.link" target="_blank" rel="noopener">{{ item.text }}</v-btn>
          </v-list-item>
        </v-list>
      </v-menu>
    </v-app-bar>

    <v-main>
      <spectrogram-list/>
    </v-main>
  </v-app>
</template>

<script>
import SpectrogramList from './components/SpectrogramList'

export default {
  name: 'App',
  components: {
    SpectrogramList,
  },
  data: () => ({
    themeDark: window.matchMedia
      && window.matchMedia('(prefers-color-scheme: dark)').matches
      || new Date().getHours() % 18 < 8, /* 18:00 to 8:00 is night time */
    links: [
      {text: 'rtl_433', link: "https://github.com/merbanan/rtl_433/"},
      {text: 'rtl_433_tests', link: "https://github.com/merbanan/rtl_433_tests/"},
      {text: 'Documentation', link: "https://triq.org/"},
      {text: 'Pulse viewer', link: "https://triq.org/pdv"},
      {text: 'BitBench', link: "http://triq.net/bitbench"},
    ],
  }),
  created () {
    this.$vuetify.theme.dark = this.themeDark
    window.matchMedia('(prefers-color-scheme: dark)').addEventListener('change', event => {
      this.themeDark = event.matches
      this.$vuetify.theme.dark = this.themeDark
    })
  },
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
