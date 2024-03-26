# ASR_Wav2Vec-Whisper

## Table of contents
1. [Introduction](#introduction)
2. [Dataset](#dataset)
3. [Data Prepration](#data_prepration)
4. [Model](#model)
    - [Wav2Vec](#wav2vec)
    - [Whisper](#whisper)

5. [Results](#results)


## <a name='introduction'></a> Introduction
This Project is about end-to-end speech recognizer, also known as automatic speech recognition(ASR) to process human speech into a written format. We feed a speech signal spoken in the Serbian language into the model to generate the transcription for that. This project has been done by means of the Speechbrain library which is an open-source all-in-one speech toolkit based on PyTorch. It is designed to make the research and development of speech technology easier. This [tutorial](https://speechbrain.readthedocs.io/en/latest/index.html) will provide you with all the very basic elements needed to start using SpeechBrain for your projects.

## <a name='dataset'></a> Dataset
The database which is used here is the Common Voice database which offers mp3 audio files at 42Hz. 

## <a name='data_prepration'></a> Data Prepration
There is a clips folder that contains all audio files. For the ASR task, you need to go over each tsv file and extract the necessary information. Since the ASR task could be computationally heavy, we only use a sub-sample of the data. Therefore, you need to get only 1 , 1/9 and 1/6 hours of data for training, validation, and test respectively. You could sample that using the sum of the calculated duration for each audio. You have to create 3 JSON files:
- 'train.json'
- 'valid.json'
- 'test.json'

## <a name='model'></a> Model
### <a name='wav2vec'></a> Wav2Vec
We implemented the following Model:
<img src="./Imgs/wav2vec_asr.png" alt="drawing" width="800" height="250"/>

The Encoder model is composed of the following layers:
- linear1 : (wav2vec_output_dim, 1024)
- BatchNorm1d
- LeakyReLU
- Dropout: 0.15
- linear2: ( 1024, 1024)
- BatchNorm1d
- LeakyReLU
- Dropout: 0.15
- linear3: ( 1024, 1024)
- BatchNorm1d
- LeakyReLU


For CTC Linear, we could use a linear layer that converts the previous layer output to output_neurons.
For CTC Loss, you have used ctc_loss from speechbrain.

### <a name='whisper'></a> Whisper
As the second task, we fine-tuned whisper for the Serbian speech recognition task. Whisper is an automatic speech recognition (ASR) system trained on 680,000 hours of multilingual and multitask supervised data collected from the web. The Whisper architecture is a simple end-to-end approach, implemented as an encoder-decoder Transformer. Input audio is split into 30-second chunks, converted into a log-Mel spectrogram, and then passed into an encoder. A decoder is trained to predict the corresponding text caption, intermixed with special tokens that direct the single model to perform tasks such as language identification, phrase-level timestamps, multilingual speech transcription, and to-English speech translation.

<img src="./Imgs/whisper_asr.png" alt="drawing" width="600" height="500"/>

We have implemented the following structure:

<img src="./Imgs/whisper_final_asr.png" alt="drawing" width="700" height="250"/>


## <a name='results'></a> Results
| <center>Metric | <center>Wav2Vec | <center>Whisper |
|:----------:|:----------:|:----------:|
| test loss | <center>4.69 | <center>0.74 |
| test CER | <center>97.02 | <center>24.93 |
| test WER | <center>100 | <center>61.07 |
