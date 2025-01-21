### [What is a Transformer](https://medium.com/inside-machine-learning/what-is-a-transformer-d07dd1fbec04)

Like LSTM models, Transformer is an architecture, namely for transforming one sequence into another one with the help of two parts (Encoder and Decode), but it differs from LSTM because **it does not any imply RNNs**.

![[transformer.webp]]

The encoder is on the left and the decoder is on the right. 

Both Encoder *and* Decoder are composed of modules that can be stacked on top of each other multiple times (described by Nx i the figure)!!! Meaning we can stack 6 encoders on top of each other. 

We see that the modules consist mainly of Multi-Head Attention and Feed Forward layers.

The inputs and outputs are first embedded into an n-dimensional space since we cannot use strings directly.

One slight but important part of the model is the position encoding of the different words. Since we have no recurrent networks we cannot remember how sequences are fed into a model. We need some way to give every part of our sequence a relative position since the information depends on the order of elements. These positions are added to the embedded representation of each word.

### Multi-Head Attention Bricks:

![[multihead_attention_brick.webp]]

#### Left Description:

We begin with the left-description of the attention-mechanism. This is the equation:
![[self_attention.webp]]

- Q is a matrix that contains the query (vector rep. of one word in the sequence).
- K are all the keys (vector reps. of all the words in the sequence)
- V are the values (vector reps. of all the words in the sequence)

for the encoder and decoder, multi-head attention modules, V consists of the same word sequence as Q. However, for the attention module that is taking into account the encoder and decoder sequences, V is different from the sequence rep. by Q.

To simplify a little bit:
![[self_attention_simplified.webp]]

This means that the weights a are defined by how each word of the sequence (represented by Q) is influenced by all the other words in the sequence (represented by K). Additionally, the SoftMax function is applied to the weights a to have a distribution between 0 and 1.

Those weights are then applied to all the words in the sequences that are introduced in V (same vectors as Q for encoder and decoder but different for the module that has encoder and decoder inputs).

#### Right Description

The righthand side describes how the attention-mechanism can be parallelised into multiple mechanism that can be used side by side. The attention mechanism is repeated multiple times with linear projections of Q, K and V.

This allows the system to learn from different representations of Q, K and V, which is beneficial to the model. These linear representations are done by multiplying Q, K and V by weight matrices W that are learned during training.

These three matrices are different for each position of the attention modules in the structure. Depending on whether they are in:
- the encoder
- the decoder
- in-between encoder and decoder

The reason is that we want to attend on the whole encoder input sequence or a part of the decoder input sequence.

the multi-head attention module that connects the encoder and decoder will take into account the encoder input-sequence with the decoder input sequence up to a given point.

>after the attention heads in both the encoder and decoder we have a pointwise feed-forward layer. this little feed-forward network has identical parameters for each position, which can be described as a separated identical transformation of each element from the given sequence.

### Training

>How to train such a 'beast'?

Training and inferring are a bit different for seq2seq models and that is true for transformers too.

We know that when we want to train a model for translation we require a lot of sentence pairs (of the two languages). Let's say we want to translate from English to Arabic.

Our encoded input will be an English sentence and the input for the decoder will be the Arabic sentence, shifted right. why?

the reason is that we do not want our model to learn to copy our decoder input during training, but,  we want to learn that given the encoder sequence and a particular decoder sequence, which has already been seen by our model, we predict the next word/character.

If we don't shift the sequence the model simply learns to 'copy' the decoder input since the target word/character for position *i* would also be the input at position *i*. Thus, our model will needs to predict a target *i* given only word/characters up to position *i - 1*. **This prevents our model from learning the copy/paste task**.

>We fill the shifted decoder sequence with a START token. similarly we append an END token to the input sequence. And it is also appended to the target sequence.

>The process of feeding the correct shifted input into the decoder is also called Teacher-Forcing, as described in [this blog](https://machinelearningmastery.com/teacher-forcing-for-recurrent-neural-networks/).

The target sequence we want for our loss calculations is simply the decoder input (Arabic Sentence) without shifting right and with an end-of-sequence token at the end.

### Inference (i.e. Prediction)

Inferring with those models is a bit different from training, which makes sense because we want to translate an English sentence without having an Arabic sentence. **The trick here is to re-feed our model for each position of the input until and END token is generated or the character limit is reached**.

Step-by-Step this would be:
- Input the encoder sequence (English sentence) and as decoder input only a START token. This will output a sequence where we only take the first element.
- Append this first element to the decoder input sequence and re-feed the decoder this new sequence and encoder with the encoder sequence.
- This will generate a new target sequence where we will take the second element and append it to the decoder input sequence.
- Repeat these steps until an END token is generated or we reach the character limit.

We see that we need multiple runs through our model to translate our sentence.
