<!-- Please remove this file from your project -->
<template>
  <div
    class="relative flex items-top justify-center min-h-screen bg-gray-100 sm:items-center sm:pt-0"
  >
    <link
      href="https://cdn.jsdelivr.net/npm/tailwindcss@2.1.2/dist/tailwind.min.css"
      rel="stylesheet"
    >
    <figure
      v-if="playing"
      class="md:flex bg-slate-100 rounded-xl p-8 md:p-0 bg-white rounded-md dark:bg-slate-800"
    >
      <img
        class="w-24 h-24 md:w-48 h-auto rounded-md mx-auto p-5"
        :src="imageUrl"
        alt=""
        width="384"
        height="512"
      >
      <div class="pt-6 md:p-8 text-center md:text-left space-y-4">
        <p class="text-lg font-medium">
          Florian is listening to:
        </p>
        <figcaption class="font-medium">
          <div class="text-sky-500 dark:text-sky-400">
            <a :href="previewUrl" target="blank">{{ track }}</a>
          </div>
          <div class="text-slate-700 dark:text-slate-500">
            {{ artistName }} "<a class="text-green-500 text-sm" :href="albumLink" target="blank">{{ albumName }}</a>"
          </div>
          <br>
          <a :href="albumLink" target="blank">
            <svg class="w-5 h-5" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 496 512"><!--! Font Awesome Pro 6.3.0 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2023 Fonticons, Inc. --><path d="M248 8C111.1 8 0 119.1 0 256s111.1 248 248 248 248-111.1 248-248S384.9 8 248 8zm100.7 364.9c-4.2 0-6.8-1.3-10.7-3.6-62.4-37.6-135-39.2-206.7-24.5-3.9 1-9 2.6-11.9 2.6-9.7 0-15.8-7.7-15.8-15.8 0-10.3 6.1-15.2 13.6-16.8 81.9-18.1 165.6-16.5 237 26.2 6.1 3.9 9.7 7.4 9.7 16.5s-7.1 15.4-15.2 15.4zm26.9-65.6c-5.2 0-8.7-2.3-12.3-4.2-62.5-37-155.7-51.9-238.6-29.4-4.8 1.3-7.4 2.6-11.9 2.6-10.7 0-19.4-8.7-19.4-19.4s5.2-17.8 15.5-20.7c27.8-7.8 56.2-13.6 97.8-13.6 64.9 0 127.6 16.1 177 45.5 8.1 4.8 11.3 11 11.3 19.7-.1 10.8-8.5 19.5-19.4 19.5zm31-76.2c-5.2 0-8.4-1.3-12.9-3.9-71.2-42.5-198.5-52.7-280.9-29.7-3.6 1-8.1 2.6-12.9 2.6-13.2 0-23.3-10.3-23.3-23.6 0-13.6 8.4-21.3 17.4-23.9 35.2-10.3 74.6-15.2 117.5-15.2 73 0 149.5 15.2 205.4 47.8 7.8 4.5 12.9 10.7 12.9 22.6 0 13.6-11 23.3-23.2 23.3z" /></svg>
          </a>
        </figcaption>
      </div>
    </figure>
    <div v-else>
      <p class="text-lg font-medium bg-white rounded-md p-5">
        Florian is not listening to anything right now.
      </p>
    </div>
  </div>
</template>

<script>
const data = {
  query: `
    query {
      nowPlaying {
        track {
          title
          artists {
            name
          }
          previewUrl
          link
        }
        album {
            imageUrl
            link
            title
        }
        isPlaying
      }
    }
  `
}

const headers = {
  headers: {
    'Content-Type': 'application/json'
  }
}

export default {
  name: 'CurrentlyPlaying',
  data () {
    return {
      playing: true,
      track: null,
      artistName: null,
      previewUrl: null,
      imageUrl: null,
      albumName: null,
      albumLink: null
    }
  },

  mounted () {
    this.getData()
    this.getTimer()
  },

  methods: {
    getTimer () {
      window.setInterval(() => {
        this.getData()
      }, 3600)
    },

    getData () {
      this.$axios
        .$post('https://api.wartner.io', data, headers)
        .then((response) => {
          this.playing = response.data.nowPlaying.isPlaying
          this.track = response.data.nowPlaying.track.title
          this.artistName = response.data.nowPlaying.track.artists[0].name
          this.previewUrl = response.data.nowPlaying.track.previewUrl
          this.imageUrl = response.data.nowPlaying.album.imageUrl
          this.albumName = response.data.nowPlaying.album.title
          this.albumLink = response.data.nowPlaying.album.link
        })
    }
  }
}
</script>
