#### [Bahdanau Attention](https://blog.floydhub.com/attention-mechanism/#bahdanau-att):
The first type of attention, commonly referred to as **Additive Attention**, came from a paper by [Dzmitry Bahdanau](https://arxiv.org/pdf/1409.0473.pdf?ref=blog.floydhub.com). 

> The paper aimed to improve the seq2seq model in machine translation by aligning the decoder with the relevant input sentences and implementing Attention.
> From Bahdanau's paper the step-by-step process is as follows

![[bahdanau_attention.jpg]]

1. **Producing the encoder hidden states** of each element in the input sequence.
2. **Calculating Alignment Scores**: between the previous *decoder* hidden state and each of the *encoder's* hidden states. *(NOTE: the last encoder hidden state can be used as the first hidden state in the decoder)*
3. **Softmaxing the Alignment Scores**: The alignment scores for each encoder hidden state are combined and represented in a single **vector** and subsequently **softmaxed**
4. **Calculating the Context Vector**: The encoder hidden states and their respective alignment scores are *multiplied* to form the **context vector**.
5. **Decoding the Output**: The context vector is concatenated with the previous decoder output and fed into the **Decoder RNN** for that time step along with the previous decoder hidden state to produce a **new output**.
6. The process (Steps 2-5) **repeats** itself for each time step of the decoder until a token (end token?) is produced or output is past the specified length.
High level overview picture:
![[bahdanau_attention_detailed.jpg]]
