### [Self Attention](https://towardsdatascience.com/illustrated-self-attention-2d627e33b20a)
also known as scaled dot-product attention *(this is a score function, there are others like dot product attention and additive/concat)*. **self attention allows the model to weight the importance of each word in the input sequence when computing representations, enabling it to capture long-range dependancies effectively.**
>What do BERT, RoBERTa, ALBERT, SpanBERT, DistilBERT, SesameBERT, SemBERT, SciBERT, BioBERT, MobileBERT, TinyBERT and CamemBERT all have in common? And I’m not looking for the answer “BERT” 🤭.

>Answer: **self-attention**

We are not only talking about architectures with the name "BERT" but, more correctly, **Transformer-based** architectures.

Transformer-based architectures, which are primarily used in modelling language understanding tasks, deliberately avoid using recurrence in neural networks and instead depend entirely on **self-attention**! mechanisms to draw global dependancies between inputs and outputs (capturing the long range dependancies).

#### Mechanism
The self-attention mechanism involves three essential components: queries, keys, and values. These components are derived from the input sequence and transformed into query, key, and value vectors through linear transformations.

we essentially have weights for each query, key and value calculation. We then matmul each input with the weights to get the query, key and value vectors.

1. The similarity between a query vector and a key vector determines the attention weight, representing the relevance of each key with respect to the given query. (1 query to all keys for each input).
2. The attention weights are calculated using the dot product between the query and key vectors, scaled by the square root of the dimension of the key vectors.
3. The resulting attention weights are then used to weight the corresponding value vectors.


Finally, the weighted value vectors are summed to obtain the final output.
Illustration:
Step 1, 2 & 3: prepare inputs, initialise weights, derive key, query and value
![[self_attention_0.gif]]

Step 4: calculate attention scores for Input 1
	- to derive the attention scores we start by taking the dot product of the given query (red) with all the keys (orange). Because there are 3 key representations we obtain 3 attention scores.
Step 5: calculate the [[Softmax]] of the attention scores (blue)
Step 6: Multiply the scores with the values
![[self_attention_1.gif]]

Step 7: sum the weighted values (yellow, element-wise) to get Output 1
Step 8: repeat for all other inputs
![[self_attention_2.gif]]

> Note: the dimension of **query** and **key** must always be the same because of the dot product score function. However, the dimension of **value** may be different from **query** and **key**. The resulting output will consequently follow the dimension of **value**.

#### Advantage of Self-Attention
The advantage lay in its ability to model dependancies between all elements in a sequence, rather than fixed content windows or convolutional operations that are limited by receptive fields?
By assigning different attention weights to different elements based on their relevance to each other, self-attention can capture complex patterns and long-range dependancies in the data.

And because of this property self-attention has become an integral part of state-of-the-art models such as the **Transformer architecture** (which has achieved significant advancements in various NLP tasks)
