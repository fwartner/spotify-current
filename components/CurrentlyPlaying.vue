<!-- Please remove this file from your project -->
<template>
  <div
    class="
      relative
      flex
      items-top
      justify-center
      min-h-screen
      bg-gray-100
      sm:items-center sm:pt-0
    "
  >
    <link
      href="https://cdn.jsdelivr.net/npm/tailwindcss@2.1.2/dist/tailwind.min.css"
      rel="stylesheet"
    >
    <div class="max-w-4xl mx-auto sm:px-6 lg:px-8">
      <div class="mt-8 bg-white overflow-hidden shadow sm:rounded-lg p-6">
        <h2 class="text-2xl leading-7 font-semibold">
          Currently playing on Spotify:
        </h2>
        <p>{{ artist }} - {{ currentlyPlaying.name }}</p>
        <img
          class="rounded-lg h-40 w-40"
          :src="imagePath"
          :alt="currentlyPlaying.name"
        >
      </div>
    </div>
  </div>
</template>

<script>
import collect from 'collect.js'

const config = {
  headers: {
    Authorization:
      'Bearer BQCKJwGDen1YuMxCCJZCBgtFTjKxrXUWrx-AcqFxyeV4E_7nghBeZBsxpKlwNjb3naN_vqzilO7EAX7MWmFbB7B4yn2W1dnYiCb4cmw7TYnjXKIh_t9kzvyjKtG4S0KuVhc1vwea7nzoZiNwYhTDp7vcmGvK3vT9p_kKpNc_MZeJ37O_mfYCu2SiUvxfa57JerZg4k6kLMPZZA'
  }
}

export default {
  name: 'CurrentlyPlaying',
  data () {
    return {
      currentlyPlaying: {},
      album: {},
      artist: null,
      imagePath: null
    }
  },

  mounted () {
    this.getData()
  },

  methods: {
    getData () {
      window.setInterval(() => {
        this.getCurrentlyPlaying()
        this.getCurrentImage()
        this.getCurrentArtist()
      }, 100)
    },

    getCurrentlyPlaying () {
      this.$axios
        .$get(
          'https://api.spotify.com/v1/me/player/currently-playing?market=DE',
          config
        )
        .then((response) => {
          this.currentlyPlaying = response.item
        })
        .catch((error) => {
          console.log(error)
        })
    },

    getCurrentImage () {
      this.$axios
        .$get(
          'https://api.spotify.com/v1/me/player/currently-playing?market=DE',
          config
        )
        .then((response) => {
          this.imagePath = collect(response.item.album.images).first().url
        })
        .catch((error) => {
          console.log(error)
        })
    },

    getCurrentArtist () {
      this.$axios
        .$get(
          'https://api.spotify.com/v1/me/player/currently-playing?market=DE',
          config
        )
        .then((response) => {
          this.artist = collect(response.item.artists).first().name
        })
        .catch((error) => {
          console.log(error)
        })
    }
  }
}
</script>
