# Tokenizer
- [2016]BPE
- [2018]unigram language model
- [2018]SentencePiece


## SentencePiece
Drawback of previous works:
1. detokenization is usually not reversibly convertible so that language-dependent.
2. 
it's composed to **Normalizer**, **Trainer**, **Encoder**(tokenization), and **Decoder**(detokenization).
- Normalizer: 
- Decoder: lossless tokenization; treat the in- put text just as a sequence of Unicode characters
