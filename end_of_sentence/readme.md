## FASTEMIT: LOW-LATENCY STREAMING ASR WITH SEQUENCE-LEVEL EMISSION REGULARIZATION

1. Existing approaches including Early and Late Penalties [1] and Constrained Alignments [2, 3] penalize emission delay by manipulating per-token or per-frame probability prediction in sequence transducer models [4]. While being successful in reducing delay, these approaches suffer from significant accuracy regression and also require additional word alignment information from an existing model.

2. Recently there are some approaches trying to regularize or penalize the emission delay.  For example, Li et al. [1] proposed Early and Late Penalties to enforce the prediction of </s> (end of sentence) within a reasonable time window given by a voice activity detector (VAD). Constrained Alignments [2, 3] were also proposed by extending the penalty terms to each word, based on speech-text alignment information [14] generated from an existing speech model.

......   其他公式原理部分较难

## JOINT ENDPOINTING AND DECODING WITH END-TO-END MODELS

1. A conventional approach is to close the microphone as soon as a VAD system observes speech followed by a long silence interval. However, silence detection and endpointing are fundamentally different tasks, and the criterion used during VAD training may not be optimal. In particular, it ignores potential acoustic cues such as filler sounds, speaking rhythm or fundamental frequency to inform the decision of whether a human talker intends to continue speaking after a given pause. In [5, 6], an end-of-query (EOQ) classifier is trained to directly predict whether or not the user has finished speaking at a given time

2. there are at least two drawbacks to using a VAD or EOQbased endpointing system. First, both the VAD and EOQ classifiers make endpointing decisions based solely on acoustic information, while ignoring information from the language model ([7] explored decoder events as extra features to gain more information). Second, these classifiers are trained independently from the rest of the components of the ASR pipeline, namely the acoustic model, pronunciation model and language model. In this paper, we focus on an endpointing solution to address both of these issues by training an end-to-end (E2E) model to do decoding and endpointing jointly.

3. To expand RNN-T with endpointing decisions, we incorporate a special symbol h/si which indicates the end of the utterance as part of the expected label sequence. 