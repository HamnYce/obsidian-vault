[What is a Recurrent Neural Networks (RNN)](https://medium.com/swlh/introduction-to-recurrent-neural-networks-rnn-c2374305a630)

### Limits of Feed Forward Networks
In feedforward neural networks (FFNN) it is mostly represent from left to right, starting from the input layer and finishing in the output layer passing through a certain number of hidden layers to the output layer.
From the prediction errors, we update the weights through a process called back propagation. In each iteration the model starts from scratch and does not consider previous inputs.![[FFNN.webp]]
This type of model has demonstrated good results for classification problems.

Another example is translation. You want to train a model that receives Spanish words and outputs english. But there is a problem, to understand the specific word you would need to read the whole sentence. Furthermore, the word 'banco' can be translated to either 'bank' or 'bench'.
So why not feed the model with the full sentence? Because if we did, say by one-hot encoding with all the words in the sentence it would just be a **bag of words** without keeping the structure and relationships in the sentence, consequently losing all meaning, the same would happen for the output layer.

### Recurrent Neural Networks (RNNs)
To solve the problem of sequences, RNNs were designed. Each time the model receives a new word form the sentence it will translate it to english, and this same step will be repeated for the rest of the words. Moreover, it will remember the previous translated word and keep it in memory so when the second word is translated we have context.
![[rnn_rolled_out.webp]]

we see in the image that the model is now facing bottom-to-top, this is because we save the x-axis for time. A time step is the unit of action or time taken for a task. In this case the time step is the translation of a word, for the example above we would have 7 time steps.

In comparison to the FFNN where the neurons in the hidden layer do not talk to each other, the neurons in the RNN do. We connect the neuron from the previous input with the neuron we are at in time. This means that as we are moving through time we also have context.

However, a problem we face with RNNs is long-term dependancies (of which LTSM was made to solve), wherein long inputs; by the time you would have reached the end, the beginning had already been forgotten!

### Types of RNNs

#### One to One
simplest, and makes no use of time as there is no use for memory or state.
![[simple_network.webp]]

#### One to Many
an example would be an image as an input and a description as the output. i.e. the model returns a sequence of words in response to an image.
![[image_processing_rnn.webp]]

#### Many to One
This structure is used for sentiment analysis,. now the input is a sequence and the output is a binary classification of positive/negative or continuous from -1 to 1.
![[RNN_classification.webp]]

#### Many to Many (Same length)
Both the input and the output are now sequences, useful for translation however length of sentences is a problem.
![[rnn_rolled_out.webp]]

#### Many to Many (encoder-decoder)
The second variant of many to many also known as encoder-decoder. This first part of the model receives a sequence and creates a new representation.
The new representation is then decoded by the second part of the model.
This now means the input and output length need not be the same length.![[RNN_simple.webp]]

### Bidirectionality
In the previous explanation we simplified the recurrent layer by creating an arrow from the cell in time step t to time step t+1 so the layer can get information from the past (backwards).
However, to predict the first word we would need information from the rest of the sentence
i.e. the future (forward) from t + k to t + k - 1. This network structure is is known as Bidirectional Recurrent Neural Network.
![[bidirectional_network.webp]]
