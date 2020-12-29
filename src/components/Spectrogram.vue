<template>
  <v-container fluid class="pa-0 pa-md-3" style="position: relative">

    <div ref="dropzone" class="dropZone"></div>

    <v-toolbar flat>
      <v-app-bar-nav-icon
        @click="tab = 0; infoView = !infoView"
      ></v-app-bar-nav-icon>
      <v-spacer></v-spacer>
      <v-btn
        class="hidden-md-and-up"
        @click="tab = 1; infoView = !infoView"
        text
        icon
      >
        <v-icon>info</v-icon>
      </v-btn>

      <v-select
        v-model="fftN"
        :items="fftNs"
        @change="spectroplot.setOption('fftN', fftN)"
        label="FFT window width"
        class="hidden-sm-and-down"
        hide-details
      ></v-select>
      <v-select
        v-model="windowF"
        :items="windowFs"
        @change="spectroplot.setOption('windowF', windowF)"
        label="FFT windowing function"
        class="hidden-lg-and-down"
        hide-details
      ></v-select>
      <v-select
        v-model="zoom"
        :items="zooms"
        @change="spectroplot.setOption('zoom', zoom)"
        label="Zoom level (magnification factor)"
        class="hidden-sm-and-down"
        hide-details
      ></v-select>
      <v-select
        v-model="gain"
        :items="gains"
        @change="spectroplot.setOption('gain', gain)"
        label="Overall gain (signal amplification)"
        class="hidden-sm-and-down"
        hide-details
      ></v-select>
      <v-select
        v-model="range"
        :items="ranges"
        @change="spectroplot.setOption('range', range)"
        label="Gain range (cut-off to black)"
        class="hidden-sm-and-down"
        hide-details
      ></v-select>
      <v-select
        v-model="cmap"
        :items="cmaps"
        @change="spectroplot.setOption('cmap', cmap)"
        label="Color map"
        class="hidden-sm-and-down"
        hide-details
      ></v-select>
      <v-select
        v-model="ampHeight"
        :items="ampHeights"
        @change="spectroplot.setOption('ampHeight', ampHeight)"
        label="Amplitude bar style"
        class="hidden-lg-and-down"
        hide-details
      ></v-select>
      <v-select
        v-model="minmaxHeight"
        :items="minmaxHeights"
        @change="spectroplot.setOption('minmaxHeight', minmaxHeight)"
        label="Decibel bar style"
        class="hidden-lg-and-down"
        hide-details
      ></v-select>

      <v-spacer></v-spacer>
      <v-btn
        @click="$emit('close')"
        text
        icon
      >
        <v-icon>close</v-icon>
      </v-btn>
    </v-toolbar>

    <v-row justify="center" class="hidden-sm-and-down">
      <v-btn @click.stop="tab = 1; infoView = !infoView" text>Info</v-btn>

      <div class="fileinfo"></div>

      <v-chip
        v-for="item in infos"
        :key="item.text"
        :title="item.text"
        small
        class="ma-1"
      >
        {{ item.value }}
      </v-chip>

    </v-row>

    <div ref="spectro">
      <div class="draw">
        <div class="left">
          <canvas class="freq"></canvas>
        </div>
        <div class="scroll">
          <canvas class="guides"></canvas>
          <canvas class="fft"></canvas>
          <canvas class="time"></canvas>
          <canvas class="amp"></canvas>
          <canvas class="minmax"></canvas>
        </div>
        <div class="right">
          <canvas class="dbfs"></canvas>
        </div>
      </div>
    </div>

    <v-dialog
      v-model="infoView"
      fullscreen
      hide-overlay
      transition="dialog-bottom-transition"
    >
      <v-card>
        <v-tabs
          v-model="tab"
          fixed-tabs
        >
          <v-tab>Settings</v-tab>
          <v-tab>File infos</v-tab>
          <v-tab-item>
            <v-card>
              <v-card-text>

                <v-select
                  v-model="fftN"
                  :items="fftNs"
                  @change="spectroplot.setOption('fftN', fftN)"
                  label="Select the FFT window width (number of different frequencies)"
                ></v-select>
                <v-select
                  v-model="height"
                  :items="heights"
                  @change="spectroplot.setOption('height', height)"
                  label="Select the FFT window height (independent of N)"
                ></v-select>
                <v-select
                  v-model="windowF"
                  :items="windowFs"
                  @change="spectroplot.setOption('windowF', windowF)"
                  label="Select the FFT windowing function"
                  cols="4" class="flex-grow-1 flex-shrink-1"
                ></v-select>
                <v-select
                  v-model="zoom"
                  :items="zooms"
                  @change="spectroplot.setOption('zoom', zoom)"
                  label="Select a zoom level (magnification factor)"
                ></v-select>
                <v-select
                  v-model="gain"
                  :items="gains"
                  @change="spectroplot.setOption('gain', gain)"
                  label="Select an overall gain (signal amplification)"
                ></v-select>
                <v-select
                  v-model="range"
                  :items="ranges"
                  @change="spectroplot.setOption('range', range)"
                  label="Select a gain range (the cut-off point to black)"
                ></v-select>
                <v-select
                  v-model="cmap"
                  :items="cmaps"
                  @change="spectroplot.setOption('cmap', cmap)"
                  label="Select a color map"
                ></v-select>
                <v-select
                  v-model="ampHeight"
                  :items="ampHeights"
                  @change="spectroplot.setOption('ampHeight', ampHeight)"
                  label="Select a style for the amplitude bar"
                ></v-select>
                <v-select
                  v-model="minmaxHeight"
                  :items="minmaxHeights"
                  @change="spectroplot.setOption('minmaxHeight', minmaxHeight)"
                  label="Select a style for the decibel bar"
                ></v-select>
                <v-select
                  v-model="histWidth"
                  :items="histWidths"
                  @change="spectroplot.setOption('histWidth', histWidth)"
                  label="Select a style for the histogram"
                ></v-select>
                <v-select
                  v-model="channelMode"
                  :items="channelModes"
                  @change="spectroplot.setOption('channelMode', channelMode)"
                  label="Select the input channel data type"
                ></v-select>

              </v-card-text>
            </v-card>
          </v-tab-item>
          <v-tab-item>
            <v-card>
              <v-card-text>
                <v-simple-table>
                  <template v-slot:default>
                    <thead>
                      <tr>
                        <th class="text-left">Property</th>
                        <th class="text-left">Value</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr v-for="item in infos" :key="item.text">
                        <td>{{ item.text }}</td>
                        <td>{{ item.value }}</td>
                      </tr>
                    </tbody>
                  </template>
                </v-simple-table>
              </v-card-text>
            </v-card>
          </v-tab-item>
        </v-tabs>

        <v-card-actions>
          <v-btn color="blue darken-1" text @click="infoView = false">Close</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

  </v-container>
</template>

<script>
import SpectroplotWorker from 'worker-loader?filename=js/spectroplot.[hash].worker.js!spectroplot/lib/worker.js'
import { Spectroplot } from 'spectroplot'
import 'spectroplot/lib/styles.css'
// we could start workers eagerly with:
// import { startWorkers } from 'spectroplot'
// startWorker(SpectroplotWorker)

export default {
  props: {
    file: [Object, File, String],
  },
  data: () => ({
    spectroplot: null,
    infos: [],
    infoView: false,
    tab: null,

    fftN: 512,
    height: 512,
    windowF: 'blackmanHarris',
    zoom: 1,
    gain: 6,
    range: 30,
    cmap: 'cube1',
    ampHeight: 0,
    minmaxHeight: 16,
    histWidth: 100,
    channelMode: 'I/Q',

    fftNs: [
      {text: 'N=1024', value: 1024},
      {text: 'N=512', value: 512},
      {text: 'N=256', value: 256},
      {text: 'N=128', value: 128},
    ],
    heights: [
      {text: 'auto', value: 0},
      {text: '1024px', value: 1024},
      {text: '768px', value: 768},
      {text: '512px', value: 512},
      {text: '256px', value: 256},
      {text: '128px', value: 128},
    ],
    windowFs: [
      {text: 'Rectangular', value: 'rectangular'},
      {text: 'Bartlett', value: 'bartlett'},
      {text: 'Hamming', value: 'hamming'},
      {text: 'Hann', value: 'hann'},
      {text: 'Blackman', value: 'blackman'},
      {text: 'Blackman-Harris', value: 'blackmanHarris'},
    ],
    zooms: [
      {text: 'x1', value: 1},
      {text: 'x2', value: 2},
      {text: 'x4', value: 4},
      {text: 'x8', value: 8},
    ],
    gains: [
      {text: '+0 dB', value: 0},
      {text: '+3 dB', value: 3},
      {text: '+6 dB', value: 6},
      {text: '+9 dB', value: 9},
      {text: '+12 dB', value: 12},
      {text: '+15 dB', value: 15},
      {text: '+18 dB', value: 18},
      {text: '+21 dB', value: 21},
      {text: '+24 dB', value: 24},
      {text: '+27 dB', value: 27},
      {text: '+30 dB', value: 30},
    ],
    ranges: [
      {text: '+6 dB', value: 6},
      {text: '+12 dB', value: 12},
      {text: '+18 dB', value: 18},
      {text: '+24 dB', value: 24},
      {text: '+30 dB', value: 30},
      {text: '+36 dB', value: 36},
      {text: '+42 dB', value: 42},
      {text: '+48 dB', value: 48},
      {text: '+54 dB', value: 54},
      {text: '+60 dB', value: 60},
      {text: '+90 dB', value: 90},
    ],
    cmaps: [
      {text: 'Cube1', value: 'cube1'},
      {text: 'Viridis', value: 'viridis'},
      {text: 'Plasma', value: 'plasma'},
      {text: 'Inferno', value: 'inferno'},
      {text: 'Magma', value: 'magma'},
      {text: 'Hot', value: 'hot'},
      {text: 'Afmhot', value: 'afmhot'},
      {text: 'Gist heat', value: 'gist_heat'},
      {text: 'Naive', value: 'naive'},
      {text: 'Parabola', value: 'parabola'},
      {text: 'Sox', value: 'sox'},
      {text: 'Gray', value: 'grayscale'},
      {text: 'Röntgen', value: 'roentgen'},
      {text: 'Phosphor', value: 'phosphor'},
    ],
    ampHeights: [
      {text: 'Amp bar off', value: 0},
      {text: 'Amp tiny', value: 16},
      {text: 'Amp full', value: 256},
    ],
    minmaxHeights: [
      {text: 'dB bar off', value: 0},
      {text: 'dB tiny', value: 16},
      {text: 'dB full', value: 256},
    ],
    histWidths: [
      {text: 'Hist off', value: 0},
      {text: 'Hist tiny', value: 30},
      {text: 'Hist full', value: 100},
    ],
    channelModes: [
      {text: 'I/Q SDR data', value: 'I/Q'},
      {text: 'L/R stereo audio', value: 'L/R'},
    ],
  }),
  mounted () {
    const w = Math.max(document.documentElement.clientWidth, window.innerWidth || 0)
    const h = Math.max(document.documentElement.clientHeight, window.innerHeight || 0)
    const options = {
      workerOrUrl: SpectroplotWorker,
      parent: this.$refs.spectro,
      filedata: this.file,
      renderInfo: this.renderInfo,
      theme: this.$vuetify.theme.dark ? 'dark' : null,
    }
    if (w < 768) {
      options.histWidth = 0
    }
    if (h < 768) {
      options.fftN = 256
      options.height = 0
    }
    this.spectroplot = new Spectroplot(options)
    this.spectroplot.enableGuides()
    this.spectroplot.createDropZone(this.$refs.dropzone)

    this.$watch('$vuetify.theme.dark', function (isDark) {
      this.spectroplot.setTheme(isDark ? 'dark' : null)
    })
  },
  beforeDestroy () {
    if (this.spectroplot)
      this.spectroplot.destroy()
  },
  methods: {
    renderInfo: function (infos) {
      this.infos = infos
    },
  },
}
</script>
