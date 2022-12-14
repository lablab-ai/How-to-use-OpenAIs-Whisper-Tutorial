# How to use OpenAIs Whisper - Tutorial

[[Go directly to the Code]](https://github.com/lablab-ai/How-to-use-OpenAIs-Whisper-Tutorial/blob/main/whisper-tutorial.ipynb)
## What is Whisper?

Whisper is an automatic State-of-the-Art speech recognition system from OpenAI that has been trained on 680,000 hours 
of multilingual and multitask supervised data collected from the web. This large and diverse 
dataset leads to improved robustness to accents, background noise and technical language. In 
addition, it enables transcription in multiple languages, as well as translation from those 
languages into English. OpenAI released the models and code to serve as a foundation for building useful
applications that leverage speech recognition. 

## How to use Whisper

The whisper model is available on GitHub. You can download it with the following command directly in the Jupyter notebook:

```python
!pip install git+https://github.com/openai/whisper.git 
```

Whisper needs `ffmpeg` installed on the current machine to work. Maybe you already have it installed, 
but its likely your local machine needs this program to be installed first. 

OpenAI refers to multiple ways to install this package, but we will be using the Scoop package manager.
 [Here](https://www.wikihow.com/Install-FFmpeg-on-Windows) is a tutorial
how to do it manually

In the Jupyter Notebook you can install it with the following command:

```bash
irm get.scoop.sh | iex
scoop install ffmpeg
```

After the installation a restart of is required if you are using your local machine.

Now we can continue. Next we import all necessary libraries:

```python
import whisper
import os
import numpy as np
import torch
```

Using a GPU is the preferred way to use Whisper. If you are using a local machine, you can check 
if you have a GPU available. 
The first line results `False`, if Cuda compatible Nvidia GPU is not available and `True` if it 
is available. The second line of 
code sets the model to preference GPU whenever it is available.

```python
torch.cuda.is_available()
DEVICE = "cuda" if torch.cuda.is_available() else "cpu"
```

Now we can load the Whipser model. The model is loaded with the following command:

```python
model = whisper.load_model("base", device=DEVICE)
print(
    f"Model is {'multilingual' if model.is_multilingual else 'English-only'} "
    f"and has {sum(np.prod(p.shape) for p in model.parameters()):,} parameters."
)
```

Please keep in mind, that there are multiple different models available. You can find all of them 
[here](https://github.com/openai/whisper/blob/main/model-card.md).
Each one of them has tradeoffs between accuracy and speed (compute needed). We will use the 
'base' model for this tutorial.

Next you need to load your audio file you want to transcribe. 

```python
audio = whisper.load_audio("audio.mp3")
audio = whisper.pad_or_trim(audio)
mel = whisper.log_mel_spectrogram(audio).to(model.device)
```

The `detect_language` function detects the language of your audio file:

```python
_, probs = model.detect_language(mel)
print(f"Detected language: {max(probs, key=probs.get)}")
```

We transcribe the first 30 seconds of the audio using the DecodingOptions and the decode command. 
Then print out the result:

```pyhton
options = whisper.DecodingOptions(language="en", without_timestamps=True, fp16 = False)
result = whisper.decode(model, mel, options)
print(result.text)
```

Next we can transcribe the whole audio file.
  
```python
result = model.transcribe("../input/audiofile/audio.mp3")
print(result["text"])
```

This will print out the whole audio file transcribed, after the execution has finished.

**Thank you** for reading. If you enjoyed this tutorial you can find more and continue reading 
[on our tutorial page](https://lablab.ai/t/) - [Fabian Stehle](https://github.com/ezzcodeezzlife), 
Junior Data Scientist at [New Native](https://newnative.ai/)

---

[![Artificial Intelligence Hackathons, tutorials and Boilerplates](https://storage.googleapis.com/lablab-static-eu/images/github/lablab-banner.jpg)](https://lablab.ai)




## Join the LabLab Discord


![Discord Banner 1](https://discordapp.com/api/guilds/877056448956346408/widget.png?style=banner1)  
On lablab discord, we discuss this repo and many other topics related to artificial intelligence! Checkout upcoming [Artificial Intelligence Hackathons](https://lablab.ai) Event


[![Acclerating innovation through acceleration](https://storage.googleapis.com/lablab-static-eu/images/github/nn-group-loggos.jpg)](https://newnative.ai)


