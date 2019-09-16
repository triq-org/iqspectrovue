<template>
  <v-container fluid class="pa-0">
    <spectrogram
      v-for="item in items"
      :key="item.key"
      :file="item.file"
      @close="removeFile"
    ></spectrogram>
    <spectrogram-dropzone
      @drop="addFile"
    ></spectrogram-dropzone>
  </v-container>
</template>

<script>
import Spectrogram from './Spectrogram'
import SpectrogramDropzone from './SpectrogramDropzone'

export default {
  components: {
    Spectrogram,
    SpectrogramDropzone,
  },
  data: () => ({
    items: [],
  }),
  mounted () {
    // load data if requested
    if (document.location.hash) {
      const file = document.location.hash.substring(1)
      const key = this.items.length
      this.items.push({key, file})
    }
  },
  methods: {
    addFile: function (file) {
      const key = this.items.length
      this.items.push({key, file})
    },
    removeFile: function (item) {
      const index = this.items.indexOf(item)
      this.items.splice(index, 1)
    },
  },
}
</script>
