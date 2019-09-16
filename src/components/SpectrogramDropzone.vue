<template>
  <v-container style="position: relative">

    <div ref="dropzone" class="dropZone"></div>

    <label class="inputfile-box">
      <svg class="inputfile-icon" xmlns="http://www.w3.org/2000/svg" width="50" height="43" viewBox="0 0 50 43">
        <path d="M48.4 26.5c-.9 0-1.7.7-1.7 1.7v11.6h-43.3v-11.6c0-.9-.7-1.7-1.7-1.7s-1.7.7-1.7 1.7v13.2c0 .9.7 1.7 1.7 1.7h46.7c.9 0 1.7-.7 1.7-1.7v-13.2c0-1-.7-1.7-1.7-1.7zm-24.5 6.1c.3.3.8.5 1.2.5.4 0 .9-.2 1.2-.5l10-11.6c.7-.7.7-1.7 0-2.4s-1.7-.7-2.4 0l-7.1 8.3v-25.3c0-.9-.7-1.7-1.7-1.7s-1.7.7-1.7 1.7v25.3l-7.1-8.3c-.7-.7-1.7-.7-2.4 0s-.7 1.7 0 2.4l10 11.6z"></path>
      </svg>
      <input ref="inputfile" type="file" id="inputfile" class="inputfile-files" name="files[]" multiple />
      <strong>Choose a file</strong>
      <span> or drag it here</span>.
      <br>
      <i>
        .cu4 .cs4 .cu8 (.data .complex16u) .cs8 (.complex16s) .cu12 .cs12 .cu16 .cs16 .cu32 .cs32 .cu64 .cs64 .cf32 (.cfile .complex) .cf64
        (also audio containing I/Q data if supported by the browser:
        .wav .bwf .webm .ogg .opus .flac .mp4 .m4a .aac .mp3)
      </i>
    </label>

    <!--
    <input type="file" id="addfile" class="files" name="files[]" multiple />
    <label for="addfile"><div class="fab"> + </div></label>
    -->

  </v-container>
</template>

<script>
import { DropZone } from 'spectroplot'
import 'spectroplot/lib/styles.css'

export default {
  data: () => ({
    spectrogram: null,
  }),
  mounted () {
    this.dropZone = new DropZone(this.$refs.dropzone, this.drop)
    this.dropZone.addInput(this.$refs.inputfile)
  },
  beforeDestroy () {
    if (this.dropZone)
      this.dropZone.destroy()
  },
  methods: {
    drop: function (file) {
      this.$emit('drop', file)
    },
  },
}
</script>
