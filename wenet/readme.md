## Unified Streaming and Non-streaming Two-pass End-to-end Model for Speech Recognition

1. Our model adopts the hybrid CTC/attention architecture, in which the conformer layers in the encoder are particularly modified. We propose a dynamic chunk-based attention strategy to allow arbitrary right context length. At inference time, the CTC decoder generates n-best hypotheses in a streaming manner. The inference latency could be easily controlled by only changing the chunk size. The CTC hypotheses are then rescored by the attention decoder to get the final result.

2. To support streaming, we modify the conformer block while bringing negligible performance degradation.

3. Two pass based model[10] was proposed on RNN-T and achieves comparable accuracy to a LAS model. However, RNN-T training is very memory consuming[18] so that we can not use a large batch for training on typical GPUs, which results in a very slow training speed as well as poor performance. Besides, RNN-T training is also unstable.

4. Our proposed U2, a CTC-AED joint model, is trained by combined CTC and AED loss and dynamic chunk attention. It not only unifies the non-streaming and streaming model, giving promising result, but also significantly simplifies the training pipeline, as well as dynamically controls the trade-off between latency and accuracy in streaming applications.

5. Right context usually improves performance compared to pure left attention, however, we should carefully design the number of layers and every right context for each layer to control the final right context for the whole model, and things get more difficult when we use conformer encoder layer, in which convolution through time with right context is used. We adopt a chunk attention in this work.