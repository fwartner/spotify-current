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
        <p>{{ artistName }} - {{ track }}</p>
        <img class="rounded-lg h-40 w-40" :src="imageUrl" :alt="track">
      </div>
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
        }
        album {
          imageUrl
        }
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
      track: null,
      artistName: null,
      previewUrl: null,
      imageUrl: null
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
      }, 10000)
    },

    getData () {
      this.$axios
        .$post('https://api.wartner.io', data, headers)
        .then((response) => {
          this.track = response.data.nowPlaying.track.title
          this.artistName = response.data.nowPlaying.track.artists[0].name
          this.previewUrl = response.data.nowPlaying.track.previewUrl
          this.imageUrl = response.data.nowPlaying.album.imageUrl
        })
    }
  }
}
</script>
