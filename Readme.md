# Deep Bio
Andy's sand box
[Andy@SantaCruzIntegration.com](mailto:Andy@SantaCruzIntegration.com?subject=DeepBio%20question)

I want to see if I can apply some recent Deep NLP and speech recognition techniques to create base caller for nano pore. Many deep speech recognition systems start by converting raw signal data from the time domain to the frequency domain.


## Table of Contents (Juypter notebooks)
You can view the notesbooks on github without having to download the github repo and run the juypter server locally.

* specgramExperiments.ipynb
    - demonstrates how to create a spectrogram in python. 
    - the pxx matrix returned by specgram() is used as the input to our model.

* nanoPoreSpectrogram.ipynb 
    -  opens a fast5 file and creates a spectrogram.
    - also demonstrates how to review white noise from a signal


## instalation notes:
```
 python --version
Python 3.6.1
```

requirements.txt is a list of python packages.

### To view, run or edit the notebooks
1. clone the git hub repo
2. start a web browser 
3. open a terminal
4. cd to the root of the repo
5 start jupyter notebook server
    ```
    $ juypter notebook
    ```
6. A new page should have opened in your browser

To terminate the juypter server on unix enter "cntrl c"



## nanoPoreSpectrogram.ipynb TODO:
* explore how to use nfft and pad_to more effectively. 
    * nfft should probably be the expected length of a segments and maybe use a 50% window overlap?
    * We can use pad_to to define range of frequence we want calculated. ie [0 ... number of kmers]. 
    * [matplotlib.pyplot.specgram](https://matplotlib.org/api/_as_gen/matplotlib.pyplot.specgram.html)
* do we get a more sparse like representation if we filter before running FFT? I.E. space vector of frequencies.


### Model  TODO:
* should we normalize the raw data? 
    - Chiron does normalization as a preprocessing step. I assume they do this to speed up learning?
    - probalby does not make sense if we are working in frequency domain

### Preprocess Pipeline TODO:
does using some combination of filtering, normalization, and spectrogram improve results? E.G. better performance on error metric? train faster? Simpler NN architeture?

- FFT and Filtering should be fast enough for our purposes
- I would expect FFT would reduce the number of CNN layers required.
- normalization typically speeds up gradient decent by reducing search range. I do not think this applies to frequency domain. frequency domain should be a sparce represetnation.
- I would expect filtering would give a better, more sparse representation. Hopefully this will be make it easier to learn

### Segmentation problem:
* Chiron: "Segmentation is error-prone approach. Segmentation algorithms determine a boundary between two
segments based on a sharp change of signal values within a window. The window size is determined by the expected speed of the translocation of the DNA fragment in the pore. We noticed that the speed of DNA translocation is variable during a sequencing run, which coupled with the high level of signal-to-noise in the raw data, can result in low segmentation accuracy."

Here is my spin. Segmentation algorithms break raw data into chucks. At least some segmentation algorithms calculate the mean and std of each chunk. The (mean,std) is the model input. This is likely to be error prone. Mean and Std are not robust measures. I.E. if the segment is off a little the (mean,std) will be off. They probably try to correct for this by trimming the ends off of each segment

When we transform our data from time to frequency domain we choose windows and overlaps such that we capture the frequency range we are interseted. This allows a given window to have samples from multiple kmers present in the sample. Transforming the data from time to frequency domain does not loose information. How ever we do loose information about location in the window. How ever our windows are ordered and realitvely small. The CNN and RNN should correct location issue.

<span style="color:red">Does removing the noise from a singal solve the segmentation problem?</span>

### Notes, TODO, scratch pad
<span style="color:red">Answer 'Why is frequency domain any better than time?'</span>

* Chiron "BasecRAWller13 uses a NN to learn segementation"


* if we remove the noise do we get something sort of like a one hot representation? i.e. very sparse?