### [General Attention](https://blog.floydhub.com/attention-mechanism/)

Im broad terms, Attention is one **component** of a network's architecture, and is in charge of managing and quantifying the **interdependence**:
1. Between the input and output elements (General Attention)
2. Within the input elements (Self Attention)

#### Attention in Sequence to Sequence (seq2seq) Models

*most tutorials will introduce attention with seq2seq models because it was originally introduced to solved the main issue around seq2seq models, with great success*. seq2seq models also known as the **Encoder-Decoder model**

![[seq2seq_overview.jpg]]

**Problem:**
The standard seq2seq model (i.e. encoder-decoder model) is generally unable to accurately process long input sequences, since only the last hidden state of the encoder RNN is used as the context vector for the decoder.
**What Attention Aims to Solve**:
On the other hand, the Attention Mechanism directly addresses this issue. It retains all hidden states of the input sequence during the decoding process. 
It does this by mapping all time steps (each input) with it's respective encoder hidden state. This means that for each output the decoder makes, it has access to all hidden states effectively the entire input sequence and can selectively pick out specific elements from that sequence during output.

There, the mechanism allows the model to focus and place more "attention" on the relevant parts of the input sequence as needed.

#### Types of Attention

![[bahdanau_vs_luong.jpg]]

There are 2 major types of attention mechanisms :
- [[Bahdanau Attention]]
- [[Luong Attention]]

While the underlying principle is the same, the difference lay in their computations and architecture.
