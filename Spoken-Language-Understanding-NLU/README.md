
## Table of contents
1. [Introduction](#introduction)
2. [Model](#model)
3. [Results](#results)


## <a name='introduction'></a> Introduction
This Project is about the process of extracting meaning from spoken sentences. The input for a spoken language system can be either a recording of speech or a transcription of a spoken sentence. The output is a semantic representation, often in the form of an ontology, which contains all the relevant information. For example, a user might say something like:

`"I want you to remind me that my next meeting with my girlfriend is at eight PM next Sunday." `

The desired output would be a semantic representation that includes the scenario, action, and entities, such as the event name, relation, time, and date:

`"{'scenario': 'calendar'| 'action': 'set'| 'entities': [{'type': 'event_name'| 'filler': 'meeting'}| {'type': 'relation'| 'filler': 'girlfriend'}| {'type': 'time'| 'filler': 'eight pm'}| {'type': 'date'| 'filler': 'next Sunday'}]}" `

Once we have the semantic representation, we can easily use it to fill a database with relevant information. This technology has many applications, such as phone reservations or as a preprocessing step for a dialogue system.

Here, we use this technology to accomplish simple tasks, such as setting up timers and alarms, performing basic computations, and so on. In our case, we will have in input sentences like:

`SET THE TIMER FOR ONE MINUTE AND FIFTY SIX SECONDS`

and we have to generate output semantics representations like:

`['intent': 'SetTimer', 'slots': ['hours': 0, 'minutes': 1, 'seconds': 56]]`

This project has been done by means of the Speechbrain library which is an open-source all-in-one speech toolkit based on PyTorch. It is designed to make the research and development of speech technology easier. This [tutorial](https://speechbrain.readthedocs.io/en/latest/index.html) will provide you with all the very basic elements needed to start using SpeechBrain for your projects.



## <a name='model'></a> Model
we will build an attention-based encoder-decoder architecture that turns the spoken files of the users into the output semantics. As shown in the figure, we are going to use a bidirectional LSTM encoder, unidirectional GRU decoder, and Bahdanau's attention between them. The encoder processes its inputs using two bidirectional LSTM layers. It's worth noting that we can use bidirectional networks in the encoder without any issues. Finally, we apply a linear layer that computes the final encoded outputs.

<img src="./Imgs/encoder.png" alt="drawing" width="300" height="500"/>

The decoder is the following which is a recurrent neural network that predicts the next token in a sequence based on the previous token. An attention mechanism connects the last layers of the encoder and the decoder. In this case, the output sequence is an intent and associated slots

<img src="./Imgs/decoder.png" alt="drawing" width="300" height="400"/>


During validation and testing, we use beam-search to find the most likely output sequence based on the decoder output probabilities. To train the system, we use negative log-likelihood as the objective function.








## <a name='results'></a> Results
| <center>Metric | <center>SLU |
|:----------:|:----------:|
| test loss | <center>0.731 |
| test CER | <center>2.73 |
| test WER | <center>6.55 |
| test SER | <center>27.08 |
