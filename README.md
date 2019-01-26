# Aposynthese

Perfect Pitch for Anyone. 

A tool to convert mp3 files into piano visualizations by decomposing the song's underlying constituent frequencies. 


[![output](assets/sample_output.gif)](https://youtu.be/Gx8MmG-gvlk "output")

---
# Installation
- `pip install -r requirements.txt` (>=Python3.6)
- `npm init`
- Ensure [`ffmpeg`](https://www.ffmpeg.org/) is installed and added to your PATH.

# Usage

## Run with Python locally:
`python audio_to_piano.py [OPTIONS]`

### Options:
```
-h, --help      Print this help text and exit.
-s, --song      Name of the song in assets/ to transpose          default=None, type=str
-y, --youtube   URL of youtube video to transpose                 default=None, type=str
-m, --max_time  Time to process audio file until                  default=None, type=int
-p, --plot      Whether to plot the spectrograms for debugging    default=False, type=bool
```

## Run on a local Node.js server:
`npm start`

Port defaults to 8000. Visit `http://0.0.0.0:8000` and enter a valid YouTube URL.

---

# How it Works 
Sound is a time signal: in the physical world, sound is how we interpret pressurve waves propogating through air over time. Digitially, this is represented as an array, where the values represent the amplitude of the wave and the index is the time point. 

<img src="https://home.cc.umanitoba.ca/~krussll/phonetics/acoustic/img/sine1.png" width="400" />

If we want to extract the musical notes from a song, we need to know which frequencies are resonating at a each time point. [The Fourier Transform](https://en.wikipedia.org/wiki/Fourier_transform) is the mathematical operation used to do this. It translates a time signal into a frequency signal, and allows us to separate those frequncies from one another. [Scipy's spectrogram](https://docs.scipy.org/doc/scipy/reference/generated/scipy.signal.spectrogram.html) method allows us to generate a high resolution fourier plot like the one shown below of a song file. The x-axis is time, the y-axis is frequency, and the color (or z-axis) is the relative intensity of that frequency/time point. Yellow is a higher intensity while blue is lower. The yellow spikes/lines therefore, are notes being played.

<img src="/assets/spectrogram.png" width="1100" />

We then iterate through this plot across time, and apply peak detection at each time vector. Next we map the dominant frequencies at each time point to [notes on a music scale](https://en.wikipedia.org/wiki/Piano_key_frequencies). Below the plot shows a fourier transform (FFT) performed at a given time point, with peak detection applied. The piano shows the detected dominant frequencies mapped onto the keys. 

<img src="/assets/fft.png" width="1100" />

<img src="/assets/piano_sample.png" width="1100" />

If this pipeline is applied across all time points, we can generate a video like this! Click the gif for a sample :)

[![output](assets/sample_output.gif)](https://youtu.be/Gx8MmG-gvlk "output")

---

# To Do Soon
- improve threshold peak detection (...dynamically?)
- modularize nodejs code

# To Do Eventually
- isolate instrument tracks and allow user to select what to decompose
- create guitar-hero like visualizaions for seeing future notes
- write 'how it works' blog post
- write tests
