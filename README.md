# E2E-VGuard

\[[Demo Page](https://e2e-vguard.github.io/)\]

We introduce E2E-VGuard, a proactive defensive framework, to prevent malicious production LLM-based speech synthesis under end-to-end scenarios. In the designing of E2E-VGuard, we protect our voice from timbre and prounciation perspectives to disrupt the text-prounciation aligment of the pre-trained TTS models, making synthesized speech cannot be heard clearly. For the imperceptibility, we introduce the psychoacoustic model to conceal the generated perturbation for human ears.

![](assest/workflow.jpg)

## Setup

We test our experiments on Ubuntu 20.04. 

The required dependencies can be installed by running the following:

```
conda create --name e2e_vguard python=3.10
conda activate e2e_vguard
pip install -r requirements.txt

sudo apt install ffmpeg
sudo apt install speak

cd monotonic_align
mkdir monotonic_align
python setup.py build_ext --inplace
```



## 1. Download Models

In the Section 3 of our paper, we introduce to perturb voice via various encoders from MFCC and TTS models. Therefore, the first step is to download the pre-trained models for each encoder. For the VITS model, you should download `pretrained_ljs.pth` from [here](https://drive.google.com/drive/folders/1ksarh-cJf3F5eKJjLVWY0X1j1qsQqiS2) and move it to `checkpoints`. Then, you can download other models by the following commands:

```
python download_models.py
```



## 2. Protect

In this repository, we provide protection for individual audio files. You can input the file path of the audio file `input_wav` you want to protect. The output path will be in the same directory as the input path, with the suffix `protected` added. You can follow the instructions below:

```
python protect.py --input_wav data/examples/libritts_5339_1.wav
```

Basic arguments:

+ `--input_wav`: The input audio path to be protected;
+ `--ASR`: The targeted ASR system for text recognition. Default: wav2vec2-base;
+ `--timbre_mode`: The protective mode of timbre prevention. Default: untargeted;
+ `--epsilon`: The perturbation radius. Default: 8;
+ `--epochs`: The optimization epochs of generated perturbation. Default: 500.
