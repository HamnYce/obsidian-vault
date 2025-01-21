#### [Luong Attention](https://blog.floydhub.com/attention-mechanism/#luong-att):
AKA Luong's Global Attention

The second type of Attention was proposed by [Thang Luong](https://arxiv.org/abs/1508.04025?ref=blog.floydhub.com). It is often referred to as **Multiplicative Attention** and was built on top of Bahdanau's Attention mechanism.

The two main differences are:
1. The way that the alignment score is calculated.
2. The position at which the Attention mechanism is being introduced in the decoder.

There are three main types of Scoring functions produced in Luong's paper. Also the general structure of the Attention Decoder is different for Luong Attention, as the context vector is only utilised after the RNN produced the output for that time step.

![[luong_attention.jpg]]

1. **Producing the Encoder Hidden States**: Encoder produces hidden states for each element in the input sequence.
2. **Decoder RNN**: The previous decoder hidden state and decoder output is pass through the **Decoder RNN** to generate a **new hidden state** for the time step
3. **Calculate Alignment Scores**: using the new decoder hidden state and the encoder hidden states, **alignment scores** are calculated.
4. **Softmaxing the Alignment Scores**: the alignment scores for each encoder hidden state are combined and represented in a single **vector** and subsequently **softmaxed**.
5. **Calculating the Context Vector**: the encoder hidden states and their respective alignment are multiplied to form the **context vector**.
6. **Producing the Final Output**: the context vector is *concatenated* with the decoder hidden state generated in step 2 and passed through a fully connected layer to produce a **new output**.
7. The process (steps 2-6) **repeats** itself for each time step of the decoder until an token is produced or output is past the specified maximum length.

