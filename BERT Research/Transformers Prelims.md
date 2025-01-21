[The illustrated Transformer](https://jalammar.github.io/illustrated-transformer/)
[Transformer Neural Network: Step-By-Step Breakdown of the Beast](https://towardsdatascience.com/transformer-neural-network-step-by-step-breakdown-of-the-beast-b3e096dc857f)

### Tranformers!

>The Transformer Neural Network is a novel architecture that aims to solve ==sequence-to-sequence tasks== while handling long-range dependencies with ease. It was proposed in the paper [_“Attention Is All You Need”_](Attention%20Is%20All%20You%20Need.pdf) 2017. It is the current state-of-the-art technique in the field of NLP.

### Recap & Motivations

#### [[RNNs (Recurrent Neural Networks)]]

Are feed forward networks rolled out over time.

![[tanh_activated_rnn.png]]

Unlike [[FFNNs (Feed Forward Neural Networks)]], are designed to take a **series of inputs** with not predetermined limit on size. "Series" as in any input with relationships between in-between it's members, or has some form of influence on them.


![[rolled_out_rnn.png]]

Basic FF networks "remember" things too, but they remember things learnt during training.

On the other hand, RNNs learn things during training, as well as things learnt from prior inputs.

It is used in a variety of models (elaboration on each on in [[Terms]]):
1. Vector-Sequence Model
2. Sequence-Vector Model
3. Sequence-to-Sequence Model (seq2seq)

**Disadvantages**:
- Slow to train
- long sequences lead to vanishing gradient, i.e. a struggle with long term dependencies. A nicer way of putting it is, it does not have a long memory.

>For **Eg**. “The clouds are in the \_\_\_\_.”
>
>It is obvious that the next word will be sky, as it is linked with the clouds. Here we see the distance between clouds and the predicted word is less so RNN can predict it easily.
>
>_But, for another example,_
>
>“I grew up in Germany with my parents, I spent many years and have proper knowledge about their culture, that’s why I speak fluent \_\_\_\_.”
>
>Here the predicted word is German, which is directly connected with Germany, but the distance between Germany and the predicted word is more in this case, so it is difficult for RNN to predict.
>
>_So, Unfortunately, as that gap grows, RNNs become unable to connect, as their memory fades with distance._


#### [[LSTM (Long Short-Term Memory) Models]]

![[LSTM_network.webp]]

A Special kind of RNN, specifically tailored to deal with the vanishing gradient problem. They are capable of learning long-term dependancies. Remembering information for longer periods of time is practically their default behaviour, not something they struggle to learn.

![[LSTM_cell_state.webp]]

The LSTM unlike normal neurons have a branch of information that passes by relatively untouched, skipping the long processing of the current cell. This "bypassing", allows the memory to be retained for longer periods of time (note: longer).

While it does improve the problem of the vanishing gradient, and introduce room for longer-term dependancies than what were capable with RNNs they begin to struggle with even longer dependancies. 

> like it will do good till 100 words, but for like 1,000 words, it starts to lose its grip.

Like a simple RNNs they are very slow to train if not slower.

>**They take input sequentially one by one, which is not able to use up GPU’s very well, which are designed for parallel computation.**

How can we parallelise sequential data?

We have 2 issues to solve:
- Vanishing gradient
- Slow training

First we solve the vanishing gradient problem:

### Attention

*It answers the question of what part of the input we should focus on, where to pay attention.*

##### Simple Seq2Seq

![[seq2seq.gif]]

We see that for each step of the encoder or decoder, the RNN is processing its inputs and generating an output for that time step. It updates the hidden state based on the current input and previous outputs its seen. The hidden state is the context vector.

##### Seq2Seq with Attention

![[seq2seq_attention.gif]]

We see already now that we are passing alot more information to the decoder.

The context vector turned out to be problematic for these types of models. It begins to struggle with longer inputs.

This is where the solution came in, [[Attention Is All You Need.pdf]].

The attention model has two main differences:
- compared to the seq2seq model it passes a lot more data to the decoder. previously only the final hidden state in the encoder is passed along. In this new model we pass all the hidden states (even the intermediate ones) to the decoder.
- The decoder part does an extra step now before producing it's output.

The last step:
1. checks each hidden state of the encoder most associated with a particular word of the input sentence.
2. gives each hidden state a score.
3. Then each hidden state is multiplied by its respective softmax score, thus amplifying hidden states with high scores, and drowning out hidden states with low scores.

![[general_attention.webp]]

This scoring is done at each time step on the decoder side.

1. The attention decoder layer takes the embedding of the \<END> token, and an initial decoder hidden state. The RNN processes its input and produces an output and a new hidden state vector(h4)
3. now the encoder hidden states and the h4 vector are used to calculate a context vector C4 for this time step. This is where the attention concept is applied, and why it's called the Attention Step
4. We concatenate (h4) and C4 into one vector
5. Now this vector is passed into a feed-forward neural network, the output of the feed-forward neural networks indicates the output word of this time step.
6. repeat these steps for the next cell in the decoder until either an \<END> token is generated or the limit has been reached.


![[seq2seq_attention_detailed.gif]]

That is the basics of attention. The main thing i would say is the scoring of the hidden states.

### Parallelisation i.e. [[Transformers]]!

