# I/Q Spectrogram viewer

WebApp to visualize the I/Q spectrogram of sample data.
This is a full featured example for [`spectroplot`](https://github.com/triq-org/spectroplot-js).
Just open https://triq.org/iqs and drop your rtl_433 sample data in.

You can choose the FFT bin number and zoom (x1, x2, x4, x8) with horizontal scrolling
There is a frequency scale and a time scale, and dB(fs) range,
a histogram of the plotted dB range and the full dB range. You can choose the gain and dB range.
Also an experimental (spark line type) bar with min/max dB per sample.
A cross hair with info on the frequency and time position is available on hover or touch.
There are some windowing functions (Rectangular, Bartlett, Hamming, Hann, Blackman, Blackman-Harris),
and some Colormap (Cube1, Sox, Naive, Viridis, Plasma, Inferno, Magma, Hot, Afmhot, Gist_heat, Parabola) to choose from.

Supported raw SDR data file types are:
- .cu4
- .cs4
- .cu8 (.data .complex16u)
- .cs8 (.complex16s)
- .cu12
- .cs12
- .cu16
- .cs16
- .cu32
- .cs32
- .cu64
- .cs64
- .cf32 (.cfile .complex)
- .cf64

Also supported are audio files containing I/Q data (if supported by the browser):
- .wav
- .bwf
- .webm
- .ogg
- .opus
- .flac
- .mp4
- .m4a
- .aac
- .mp3

All comments and suggestions very welcome.

If you like to give feedback:
- Is this useful to you? Why not, what is missing?
- Would you like to upload your samples to share by URL?
- Would you like to embed uploaded samples in e.g. a blog?
- Would you like to see more features, like timing guides, bandpass filters, ...?

## Compiling

- Setup dependencies: `yarn install`
- Compile and hot-reload for development: `yarn run serve`
- Compile and minify for production: `yarn run build`
- Lint and fix files: `yarn run lint`

## Copyright and Licence

Copyright (C) 2019-2021 Christian W. Zuckschwerdt <zany@triq.net>

Unless otherwise noted all sources are:

License: GPL-2+
