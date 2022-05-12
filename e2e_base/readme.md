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


 ## Attention Is All You Need

reference: https://www.bilibili.com/video/BV1pu411o7BE?spm_id_from=333.1007.top_right_bar_window_custom_collection.content.click

 abstract: the Transformer, based solely on attention mechanisms, dispensing with recurrence and convolutions entirely.
 why named self attention: In a self-attention layer all of the keys, values and queries come from the same place, in this case, the output of the previous layer in the encoder. Each position in the encoder can attend to all positions in the previous layer of the encoder. 输入的 k q v 都是自己

 1. Recurrent models 
 Aligning the positions to steps in computation time, they generate a sequence of hidden states ht, as a function of the previous hidden state ht􀀀1 and the input for position t. This inherently sequential nature precludes parallelization within training examples, which becomes critical at longer sequence lengths, as memory constraints limit batching across examples.

 2. other Self-attention, sometimes called intra-attention is an attention mechanism relating different positions of a single sequence in order to compute a representation of the sequence.

 however, the Transformer is the first transduction model relying entirely on self-attention to compute representations of its input and output without using sequencealigned RNNs or convolution.

 3. Model Architecture
     * Encoder: The encoder is composed of a stack of N = 6 identical layers. Each layer has two sub-layers. The first is a multi-head self-attention mechanism, and the second is a simple, positionwise fully connected feed-forward network.

     * Decoder: The decoder is also composed of a stack of N = 6 identical layers. In addition to the two sub-layers in each encoder layer, the decoder inserts a third sub-layer, which performs multi-head attention over the output of the encoder stack

    * Attention function: can be described as mapping a query and a set of key-value pairs to an output, where the query, keys, values, and output are all vectors. The output is computed as a weighted sum of the values, where the weight assigned to each value is computed by a compatibility function of the query with the corresponding key.

    * layer normalization && batch normalization.   layer normalization is better in this work. 

    * Scaled Dot-Product Attention

    * Multi-Head Attention: Instead of performing a single attention function with dmodel-dimensional keys, values and queries, we found it beneficial to linearly project the queries, keys and values h times with different, learned linear projections to dk, dk and dv dimensions, respectively.

    * Position-wise Feed-Forward Networks: In addition to attention sub-layers, each of the layers in our encoder and decoder contains a fully connected feed-forward network, which is applied to each position separately and identically. This consists of two linear transformations with a ReLU activation in between. 每一个输出的点都独立计算ffn, 不用像 rnn 一样后一个点需要带上前一个点的信息。 全局信息由attention 获得。

    * Positional Encoding: Since our model contains no recurrence and no convolution, in order for the model to make use of the order of the sequence, we must inject some information about the relative or absolute position of the tokens in the sequence. To this end, we add "positional encodings" to the input embeddings at the bottoms of the encoder and decoder stacks. 序列前后时序关系由 这一层来建模，通过增加位置信息。
    
4. why self-attention: One is the total computational complexity per layer. Another is the amount of computation that can be parallelized, as measured by the minimum number of sequential operations required. The third is the path length between long-range dependencies in the network. Learning long-range dependencies is a key challenge in many sequence transduction tasks.


## Conformer: Convolution-augmented Transformer for Speech Recognition

Abstract: Transformer models are good at capturing content-based global interactions, while CNNs exploit local features effectively.

1. While Transformers are good at modeling long-range global context, they are less capable to extract finegrained local feature patterns.
Convolution neural networks (CNNs), on the other hand, exploit local information and are used as the de-facto computational block in vision. They learn
shared position-based kernels over a local window which maintain translation equivariance and are able to capture features like edges and shapes. One

2. Multi-Headed Self-Attention Module: employ multi-headed self-attention (MHSA) while integrating an important technique from Transformer-XL [20], the relative sinusoidal positional encoding scheme. The relative positional encoding allows the self-attention module to generalize better on different input length and the resulting encoder is more robust to the variance of the utterance length.

3. Convolution module. The convolution module contains a pointwise convolution with an expansion factor of 2 projecting the number of channels with a GLU activation layer, followed by a 1-D Depthwise convolution. The 1-D depthwise conv is followed by a Batchnorm and then a swish activation layer.

4. Feed forward module. The first linear layer uses an expansion factor of 4 and the second linear layer projects it back to the model dimension. We use swish activation and a pre-norm residual units in feed forward module.

## TRIGGERED ATTENTION FOR END-TO-END SPEECH RECOGNITION

1. The proposed system architecture, named triggered attention (TA), uses a CTC-based classifier to control the activation of an attention-based decoder neural network. This allows for a frame-synchronous decoding scheme with an adjustable look-ahead parameter to control the induced delay and opens the door to streaming recognition with attention-based end-to-end ASR systems.

2. Decoding
Inference with the TA model is realized in a frame-synchronous decoding manner. A best path search with the CTC-based trigger model is used to generate trigger events. Note that the trigger model is generating a character sequence as well but we do not combine this output with the scores of the attention model, which is subject to future work. However, uncertainties of the CTC-based trigger model are taken into account to generate alternative trigger sequences.  This is achieved by tracking the posterior probabilities of the CTC output and whenever the softmax score of an alternative path is higher than a manually chosen threshold, a new trigger event is initiated. We chose a threshold of 0.2 throughout all experiments, which was experimentally determined using the Wall Street Journal (WSJ) data sets but not further optimized for the other ASR tasks.  Trigger events of low confidence can be skipped by the attentionbased decoder by bypassing the attention decoder states of the previous time step. After the attention model is triggered, the decoding is similar to a conventional beam-search algorithm [4]. Note that the problem of misalignment is almost nonexistent for TA decoding, because we are only attending to past encoder frames (relative to the trigger event) and the length of the output sequence is mostly determined by the trigger model. Thus, penalty factors and other terms that are often used in label-synchronous decoding to supervise the length of the generated output sequence are not required [4].  However, since hypotheses of different length can be in the decoding beam, we require an adjusted pruning concept using sequencelength- normalized scores instead of unnormalized probabilities to determine the best hypotheses that are kept.


## Streaming Chunk-Aware Multihead Attention for Online End-to-End Speech Recognition

1. In this work, we propose a novel online E2E-ASR system by using Streaming Chunk-Aware Multihead Attention (SCAMA) and a latency control memory equipped self-attention network (LC-SAN-M).  LC-SAN-M uses chunk-level input to control the latency of encoder.

2. Currently, there exists three popular end-toend approaches, namely connectionist temporal classification (CTC) [1], recurrent neural network transducer (RNN-T) [2], and attention based encoder-decoder (AED)

3. The most representative attention based model is the socalled LAS [5], which consists of a pyramidal bidirectional long short term memory (BLSTM) based encoder and an attentionequipped LSTM based decoder. The encoder transfers raw acoustic feature into higher-level representation, and the decoder with attention mechanism predicts the next output symbol based on the previous predictions in an auto-regressive manner.  The attention module inside decoder is used to compute dynamic soft alignments and produce context vectors. As originally defined, the soft attention needs to attend entire input sequences at each output timestep. As a result, soft attention based E2E model is inapplicable to online speech recognition, since it has to wait until the input sequence has been processed before it can generate output.

4. The LC-SAN-M based encoder uses chunk-level input to control the encoder latency. For SCAMA, we use a jointly trained predictor to predict the number of tokens in each chunk and control the activation of an attention-based decoder. Compared to triggered attention (TA), the predictor is trained using a cross-entropy loss instead of CTC loss. More importantly, prediction of token number in chunk-level inputs can achieve very high accuracy, thus eliminating the mismatch between training and testing.

5. Decoding Strategy
For encoder-decoder based E2E-ASR, the inference is terminated when an end-of-sentence (<eos>) token is predicted. For streaming E2E-ASR, one of the issues is that the decoder may predict the <eos> token too early or too late [26]. In our works, we also find that the decoder may generate the <eos> token too early, especially when the chunk size is small. We propose a trick during beam search based decoding to handle this problem.  During inference, if the decoder generate an <eos> token with the input is not the last chunk, we will use the previous token and historical information to predict the next token instead of <eos>. As to the last chunk, if the predicted token number is N, the total decoding steps will be 1 to N +2. The inference is terminated when the decoder generated an <eos> token or decoded for N + 2 steps in the last chunk.
