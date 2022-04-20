## STATE-OF-THE-ART SPEECH RECOGNITION WITH SEQUENCE-TO-SEQUENCE MODELS

1. we show that word piece models can be used instead of graphemes.
2. We also introduce a multi-head attention architecture, which offers improvements over the commonly-used single-head attention.
3. On the optimization side, we explore synchronous training, scheduled sampling, label smoothing, and minimum word error rate optimization, which are all shown to improve accuracy.

## 3M: Multi-loss, Multi-path and Multi-level Neural Networks for speech

key point: Specifically, multi-loss refers to the joint CTC/AED loss and multi-path denotes the Mixture-of-Experts(MoE) architecture which can effectively increase the model capacity without remarkably increasing computation cost. Multi-level means that we introduce auxiliary loss at multiple level of a deep model to help training. 

1. conformer is a favored choise to train benchmarks for many E2E ASR toolkits(eg. Espnet, Wenet), CTC/attention-based encoder-decoder, also known as CTC/AED, is a popular E2E ASR framework.
2. Several techniques have been proposed to reduce the model complexity, such as knowledge distillation, low-rank decomposition, quantization and pruning, but these methods inevitably suffer from performance degradation.

#### Multi-loss
1. CTC/AED is commonly used in E2E ASR, which effectively utilizes the advantages of both architectures in training and decoding. On one hand, the attention lacks left-to-right constraints, making it difficult to generate proper alignments in the case of noisy data and/or long input sequences. Augmenting the constrained CTC loss based on the forward-backward algorithm can greatly reduce the number of irreuglarly aligned utterances. On the other hand, CTC imposes the conditional independence constraint that output predictions are independent, which is not true for ASR. Attention objective can help eliminate the conditional independence constraint and back propagate the context information to rectify the probability distribution output by CTC. When employing decoding, both attention-based scores and CTC scores can be combined in a rescoring/one-pass beam search to improve the performance.

### Multi-path
1. we propose to apply MoE architecture on Conformer model.

 ### Multi-level
 1. As we know, the lower layers of the Conformer encoder also have a low correlation with grapheme information. Generally, the input of the attention decoder is the top layer of the Conformer encoder. In order to make the lower layers of encoder have a earlier access to grapheme information from the decoder, we propose to integrate separate attention decoders at intermediate layers of the encoder,