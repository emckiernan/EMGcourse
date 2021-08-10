::: {.cell .markdown}
Graphing and exploring EMG data
===============================

### Ana Daniela del Río Pulido`<sup>`{=html}1`</sup>`{=html}, Noel Isaías Plascencia Díaz`<sup>`{=html}2`</sup>`{=html}, Mitsui Myrna Salgado Saito`<sup>`{=html}3`</sup>`{=html}, and Erin C. McKiernan`<sup>`{=html}4`</sup>`{=html} {#ana-daniela-del-río-pulido1-noel-isaías-plascencia-díaz2-mitsui-myrna-salgado-saito3-and-erin-c-mckiernan4}

`<sup>`{=html}1`</sup>`{=html} Licenciatura en Física Biomédica,
Facultad de Ciencias, Universidad Nacional Autónoma de
México`<br/>`{=html} `<sup>`{=html}2`</sup>`{=html} Licenciatura en
Física, Facultad de Ciencias, Universidad Nacional Autónoma de
México`<br/>`{=html} `<sup>`{=html}3`</sup>`{=html} Licenciatura en
Ciencias de la Tierra, Facultad de Ciencias, Universidad Nacional
Autónoma de México`<br/>`{=html} `<sup>`{=html}4`</sup>`{=html}
Departamento de Física, Facultad de Ciencias, Universidad Nacional
Autónoma de México`<br/>`{=html}
:::

::: {.cell .markdown}
Overview
--------

The objective of this data analysis practical is to graph and explore
EMG recordings made from different skeletal muscles during various
physical activities. The recordings to be analyzed can be found in our
GitHub repository (<https://github.com/emckiernan/electrophys>), or new
recordings can be obtained by performing the \'EMG basics\' laboratory
practical from this series.
:::

::: {.cell .markdown}
Setting up the notebook
-----------------------

We begin by setting up the Jupyter notebook and importing the Python
modules for plotting figures. We include commands to view plots in the
notebook, and to create figures with good resolution and large labels.
:::

::: {.cell .code execution_count="62" collapsed="false"}
``` {.python}
# command to view figures in Jupyter notebook
%matplotlib inline 

# import plotting module 
import matplotlib.pyplot as plt 

# commands to create high-resolution figures with large labels
%config InlineBackend.figure_formats = {'png', 'retina'} 
plt.rcParams['axes.labelsize'] = 18 # fontsize for figure labels
plt.rcParams['axes.titlesize'] = 20 # fontsize for figure titles
plt.rcParams['font.size'] = 16 # fontsize for figure numbers
plt.rcParams['lines.linewidth'] = 1.6 # line width for plotting
```
:::

::: {.cell .markdown}
Next, we import various modules for extracting data and scientific
computing.
:::

::: {.cell .code execution_count="63" collapsed="true"}
``` {.python}
import numpy as np
import scipy as sc
import wave
```
:::

::: {.cell .markdown}
Extracting and graphing the data
--------------------------------

EMG recordings were obtained using the Backyard Brains EMG Spiker Box,
and are saved as audio files in .wav format. So, we first have to open
the .wav files and extract the data. We can also extract the number of
recording channels and sampling rate.
:::

::: {.cell .code execution_count="64" collapsed="false"}
``` {.python}
# open .wav file by specifying the path and filename
record = wave.open('../data/S10_EMG_calf_intermittent.wav', 'r')

# extract number of channels, sample rate, data
numChannels = record.getnchannels() # number of channels
N = record.getnframes() # humber of frames
sampleRate = record.getframerate() # sampling rate
dstr = record.readframes(N * numChannels)
waveData = np.frombuffer(dstr, np.int16)

# get the time window
timeEMG=np.linspace(0, len(waveData)/sampleRate, num=len(waveData))

# calculate frequency
freq = 1/np.mean(np.diff(timeEMG))

print('The recording has %d channel(s).' % (numChannels))
print('The sampling rate of the recording is %d Hz.' % (sampleRate))
```

::: {.output .stream .stdout}
    The recording has 1 channel(s).
    The sampling rate of the recording is 44100 Hz.
:::
:::

::: {.cell .markdown}
Now, let\'s plot the EMG. The following recording was made from the calf
muscle during repeated calf raises.
:::

::: {.cell .code execution_count="60" collapsed="false"}
``` {.python}
plt.figure(figsize=(18,6))
plt.xlabel(r'time (s)')
plt.ylabel(r'voltage ($\mu$V)')
plt.plot(timeEMG,waveData, 'b')
plt.xlim(0,max(timeEMG));
```

::: {.output .display_data}
![](0606fd79453f34c4dfbbf73adf82cc0b3d69ce9f.png){height="384"
width="1113"}
:::
:::

::: {.cell .markdown}
We can define a function to allow us to quickly extract and graph
different EMG recordings. The input to the function is the name of the
file we want to analyze.
:::

::: {.cell .code execution_count="57" collapsed="true"}
``` {.python}
def EMG(file):
    # open .wav file by specifying the path and filename
    record = wave.open(file)
    # extract number of channels, sample rate, data
    numChannels = record.getnchannels() # number of channels
    N = record.getnframes() # number of frames
    sampleRate = record.getframerate() # sampling rate
    # extract data from the .wav file
    dstr = record.readframes(N * numChannels)
    waveData = np.frombuffer(dstr, np.int16)
    # print the number of channels and sample rate
    print('The recording has %d channel(s).' % (numChannels))
    print('The sampling rate of the recording is %d Hz.' % (sampleRate))
    # calculate time window
    timeEMG=np.linspace(0, len(waveData)/sampleRate, num=len(waveData))
    # calculate frequency
    freq = 1/np.mean(np.diff(timeEMG))
    # plot EMG
    plt.figure(figsize=(18,6))
    plt.xlabel(r'time (s)')
    plt.ylabel(r'voltage ($\mu$V)')
    plt.plot(timeEMG,waveData, 'b')
    plt.xlim(0,max(timeEMG));
    
    return
```
:::

::: {.cell .markdown}
Exploring the activity of different muscles
-------------------------------------------

Using our function, we can now dive into our repository of recordings
and look at the EMGs of different muscles recorded during various
physical activities.

### Bicep muscle

The following EMG was recorded from the bicep muscle during repeated (3)
contractions with intermittent rest periods.
:::

::: {.cell .code execution_count="65" collapsed="false"}
``` {.python}
EMG(file='../data/S10_EMG_bicep_intermittent.wav')
```

::: {.output .stream .stdout}
    The recording has 1 channel(s).
    The sampling rate of the recording is 44100 Hz.
:::

::: {.output .display_data}
![](74095974c5b8eb6b8b9daca48212a27e91a822af.png){height="384"
width="1103"}
:::
:::

::: {.cell .markdown}
The following EMG was recorded from the bicep as the subject sustained a
contraction and gradually increased the amount of force (i.e.,
recruitment).
:::

::: {.cell .code execution_count="28" collapsed="false"}
``` {.python}
EMG(file='../data/S10_EMG_bicep_recruitment.wav')
```

::: {.output .stream .stdout}
    The recording has 1 channel(s).
    The sampling rate of the recording is 44100 Hz.
:::

::: {.output .display_data}
![](4fd006d5ed72cf51540a82da09804198ddbd4d2e.png){height="384"
width="1113"}
:::
:::

::: {.cell .markdown}
### Study questions and exercises:

-   How do the recordings differ when we look at intermittent versus
    sustained muscle contractions?
-   Which features of the EMG recording change as the subject increases
    the force of contraction, and how? In other words, how might we
    quantify motor unit recruitment?
-   What other exercises could you carry out while recording from the
    bicep muscle, and how would you expect the EMG recordings to look?
-   If you have recording equipment available, record from your own
    bicep muscle and then extract and graph your data.
:::

::: {.cell .markdown}
### Tricep muscle

The following recording was made from the tricep muscle while the
subject performed tricep dips.
:::

::: {.cell .code execution_count="36" collapsed="false"}
``` {.python}
EMG(file='../data/S1_EMG_tricep_dips.wav')
```

::: {.output .stream .stdout}
    The recording has 1 channel(s).
    The sampling rate of the recording is 44100 Hz.
:::

::: {.output .display_data}
![](a5e774cebdb63d82399e4b5e7ece0c34aa7d91aa.png){height="384"
width="1113"}
:::
:::

::: {.cell .markdown}
This next recording is more complex. It was made from the tricep muscle
and included the following sequence of activities: (1) for the first 10
seconds, the subject was at rest, (2) then for the following 10 seconds,
the subject turned their arm to activate the tricep isometrically, (3)
then the subject changed position, and (4) finally, during the last 20
seconds, the subject repeatedly lifted and lowered a bottle with water
inside (i.e., similar to an overhead tricep extension).
:::

::: {.cell .code execution_count="38" collapsed="false"}
``` {.python}
EMG(file='../data/S1_EMG_tricep_twistWeight.wav')
```

::: {.output .stream .stdout}
    The recording has 1 channel(s).
    The sampling rate of the recording is 44100 Hz.
:::

::: {.output .display_data}
![](8b3dac44db9f80a668ea446e1db880f27f1b006f.png){height="384"
width="1113"}
:::
:::

::: {.cell .markdown}
### Study questions and exercises: {#study-questions-and-exercises}

-   Why do you think the tricep dip recording shows a high level of
    continuous activity despite the fact that the dips involve an
    up-and-down movement?
-   How is the tricep dip activity different from the repeated tricep
    extension at the end of the second recording, and why?
-   What other exercises could you carry out while recording from the
    tricep muscle, and how would you expect the EMG recordings to look?
-   If you have recording equipment available, record from your own
    tricep muscle and then extract and graph your data.
:::

::: {.cell .markdown}
### Forearm muscles

The following recording was made from the forearm while the subject
repeatedly pressed a hand gripper exercise device. It shows five
contractions in total, with the last one sustained for a longer period
of time than the previous four.
:::

::: {.cell .code execution_count="39" collapsed="false"}
``` {.python}
EMG(file='../data/S2_EMG_forearm_grip.wav')
```

::: {.output .stream .stdout}
    The recording has 1 channel(s).
    The sampling rate of the recording is 44100 Hz.
:::

::: {.output .display_data}
![](6b9a17cf02b8f18607a10dbc56fb46ea8ba25ad1.png){height="384"
width="1113"}
:::
:::

::: {.cell .markdown}
The next recording was made from the forearm while the subject
participated in an arm wrestling match. Around second 14, the subject
lost the match.
:::

::: {.cell .code execution_count="42" collapsed="false"}
``` {.python}
EMG(file='../data/S2_EMG_forearm_wrestle.wav')
```

::: {.output .stream .stdout}
    The recording has 1 channel(s).
    The sampling rate of the recording is 44100 Hz.
:::

::: {.output .display_data}
![](b20af3988d1795f9e7fabb5bee2a3a6769e5adbd.png){height="384"
width="1113"}
:::
:::

::: {.cell .markdown}
#### Study questions and exercises: {#study-questions-and-exercises}

-   What muscles are found in the forearm, and which could be
    contributing to the EMG signal?
-   What other muscles, besides those in the forearm, might be activated
    during an arm wrestling match? Would you expect to see differences
    in their EMGs during this activity?
-   What other exercises could you carry out while recording from the
    forearm muscles, and how would you expect the EMG recordings to
    look?
-   If you have recording equipment available, record from your own
    forearm and then extract and graph your data.
:::

::: {.cell .markdown}
### Jaw muscles

The following EMG was recorded from the jaw muscles. For the first 50
seconds, the subject chewed a soft, gummy candy. Then there were 10
seconds of rest. And finally, for the last ten seconds the subject made
a forced smile.
:::

::: {.cell .code execution_count="43" collapsed="false"}
``` {.python}
EMG(file='../data/S3_EMG_jawMuscle_chewSmile.wav')
```

::: {.output .stream .stdout}
    The recording has 1 channel(s).
    The sampling rate of the recording is 44100 Hz.
:::

::: {.output .display_data}
![](1169a4851657e307453b2a075e92df644e458796.png){height="384"
width="1113"}
:::
:::

::: {.cell .markdown}
### Study questions and exercises: {#study-questions-and-exercises}

-   Which muscles control chewing and smiling, and which could be
    contributing to the EMG signal?
-   What differences do you see in the EMGs recorded when the subject is
    chewing versus smiling, and what do you think causes these
    differences?
-   What other activites could you carry out while recording from the
    jaw or other facial muscles, and how would you expect the EMG
    recordings to look?
-   If you have recording equipment available, record from your own jaw
    muscles, try experimenting with different electrode placements on
    the face, and then extract and graph your data.
:::

::: {.cell .markdown}
### Abdominal muscles

The following EMG recording was made from the abdominal muscles,
specifically rectus abdominis, during lying leg raises.
:::

::: {.cell .code execution_count="47" collapsed="false"}
``` {.python}
EMG(file='../data/S5_EMG_abs_legLift3.wav')
```

::: {.output .stream .stdout}
    The recording has 1 channel(s).
    The sampling rate of the recording is 10000 Hz.
:::

::: {.output .display_data}
![](70a31ed700a4c7e874cd78ea64402bec0b4d8ce6.png){height="385"
width="1103"}
:::
:::

::: {.cell .markdown}
During the next recording, the subject performed a sustained plank.
:::

::: {.cell .code execution_count="48" collapsed="false"}
``` {.python}
EMG(file='../data/S5_EMG_abs_plank.wav')
```

::: {.output .stream .stdout}
    The recording has 1 channel(s).
    The sampling rate of the recording is 10000 Hz.
:::

::: {.output .display_data}
![](4fc6c8e44af3a923dae136695a8fb44e55442a2d.png){height="384"
width="1103"}
:::
:::

::: {.cell .markdown}
### Study questions and exercises: {#study-questions-and-exercises}

-   What do you think would happen to the EMG recordings if the subject
    performed more lying leg raises, or tried to hold the plank for
    longer? In other words, how might fatigue show up on the two
    recordings?
-   How could we quantify fatigue, and how might the quantification be
    different for the two types of exercise?
-   What other exercises could you do to activate the abdominal muscles,
    or which other ab muscles could you record from, and how would you
    expect the EMG recordings to look?
-   If you have recording equipment available, record from your own
    abdominal muscles, try experimenting with different electrode
    placements and exercises, and then extract and graph your data.
:::

::: {.cell .markdown}
### Quadricep muscles

The following EMG was recorded from the quadricep muscles while the
subject performed a one-legged squat, also known as a Bulgarian squat.
The subject also had a backpack filled with books on during the exercise
for added weight.
:::

::: {.cell .code execution_count="52" collapsed="false"}
``` {.python}
EMG(file='../data/S9_EMG_quadricep_oneLegSquat1.wav')
```

::: {.output .stream .stdout}
    The recording has 1 channel(s).
    The sampling rate of the recording is 10000 Hz.
:::

::: {.output .display_data}
![](56f4d83bd949630a8b380ebfd427e82c2d77c401.png){height="386"
width="1103"}
:::
:::

::: {.cell .markdown}
In the next recording, the subject sustained a wall squat, maintaining
the backpack on top of the legs for added weight.
:::

::: {.cell .code execution_count="61" collapsed="false"}
``` {.python}
EMG(file='../data/S9_EMG_quadricep_squatSustained.wav')
```

::: {.output .stream .stdout}
    The recording has 1 channel(s).
    The sampling rate of the recording is 10000 Hz.
:::

::: {.output .display_data}
![](87b3c09bc9e03699f5c6a040e17bac80d5bd4382.png){height="384"
width="1113"}
:::
:::

::: {.cell .markdown}
### Study questions and exercises: {#study-questions-and-exercises}

-   What muscles make up the quadricep group, and which could be
    contributing to the EMG signal?
-   What factors could explain the fluctuations seen in the two
    recordings? What might have caused the increased amplitude at the
    end of the wall squat recording?
-   What other exercises could you perform while recording from the
    quadricep muscles, and how would you expect the EMG recordings to
    look?
-   If you have recording equipment available, record from your own
    quadriceps, try experimenting with different electrode placements,
    and then extract and graph your data.
:::

::: {.cell .markdown}
You can explore more recordings from our repository. Or, record your own
EMGs and experiment with electrode placement and different exercises to
see how it affects the signal.
:::

::: {.cell .markdown}
### Funding

This work was supported by UNAM-DGAPA-PAPIME PE213817 and PE213219.
:::
