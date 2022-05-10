## Neural Machine Translation of Rare Words with Subword Units

1. We show that open-vocabulary neural ma-chine translation is possible by encoding (rare) words via subword units. We find our architecture simpler and more effective than using large vocabularies and back-off dictionaries (Jean et al., 2015; Luong et al., 2015b).

2. We adapt byte pair encoding (BPE) (Gage, 1994), a compression algorithm, to the task of word segmentation. BPE allows for the representation of an open vocabulary through a fixed-size vocabulary of variable-length character sequences, making it a very suitable word segmentation strategy for neural network models.

### Byte Pair Encoding (BPE)
    Byte Pair Encoding (BPE) (Gage, 1994) is a simple data compression technique that iteratively replaces the most frequent pair of bytes in a sequence with a single, unused byte. We adapt this algorithm for word segmentation. Instead of merging frequent pairs of bytes, we merge characters or character sequences.  

    Firstly, we initialize the symbol vocabulary with the character vocabulary, and represent each word as a sequence of characters, plus a special end-ofword symbol ‘·’, which allows us to restore the original tokenization after translation. We iteratively count all symbol pairs and replace each occurrence of the most frequent pair (‘A’, ‘B’) with a new symbol ‘AB’. Each merge operation produces a new symbol which represents a character n-gram. Frequent character n-grams (or whole words) are eventually merged into a single symbol, thus BPE requires no shortlist. The final symbol vocabulary size is equal to the size of the initial vocabulary, plus the number of merge operations – the latter is the only hyperparameter of the algorithm.

## Subword Regularization: Improving Neural Network Translation Models with Multiple Subword Candidates

1.  Byte-Pair-Encoding (BPE)
An advantage of BPE segmentation is that it can effectively balance the vocabulary size and the step size (the number of tokens required to encode the sentence). BPE trains the merged operations only with a frequency of characters. Frequent substrings will be joined early, resulting in common words remaining as one unique symbol. Words consisting of rare character combinations will be split into smaller units, e.g., substrings or characters.  Therefore, only with a small fixed size of vocabulary (usually 16k to 32k), the number of required symbols to encode a sentence will not significantly increase, which is an important feature for an efficient decoding.

One downside is, however, that BPE is based on a greedy and deterministic symbol replacement, which can not provide multiple segmentations with probabilities. It is not trivial to apply BPE to the subword regularization that depends on segmentation probabilities P(xjX).

2. In this paper, we propose a new subword segmentation algorithm based on a unigram language model, which is capable of outputing multiple subword segmentations with probabilities. The unigram language model makes an assumption that each subword occurs independently, and consequently, the probability of a subword sequence