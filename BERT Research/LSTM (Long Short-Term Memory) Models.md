### [LSTM Networks](https://towardsdatascience.com/lstm-networks-a-detailed-explanation-8fae6aefc7f9)
>This post explains long short-term memory (LSTM) networks. I find that the best way to learn a topic is to read many different explanations and so I will link some other resources I found particularly helpful, at the end of this article. I would highly encourage you to check them out for varying perspectives and explanations of LSTMs!

![[LSTM_all_gates.webp]]

### What are LSTMs and why are they useful ?

LSTMs were designed to overcome the long-term dependancy problem faced in RNNs (due to the vanishing gradient problem). LSTMs have feed*back* connections which make them different to more traditional feed*forward*. 

This feedback property enables LSTMs to process entire sequences of data without treating each point independently, but rather retain useful information from previous data in the sequence to help with the processing of new data points. 

Consequently, LSTMs are particularly good at processing sequences of data such as text, speech and general time-series.

An example, If we are try to predict ice-cream sales, they would vary widely depending on the time of year. Being lowest in December and highest in June.

An LSTM network can learn this pattern exists every 12 periods in time, **it doesn't just use the previous prediction**, but rather retains a longer-term context which helps it overcome the long-term dependancy problem faced by other models. 

> This is a simple example, but when facing longer spans of time, LSTMs become increasingly useful.

### How do LSTMs work

![[LSTM_all_gates.webp]]

Firstly, the LTSM depends on three inputs at any particular time step:
- The previous cell state (current long term context / memory
- The previous hidden state (the output at the previous point in time)
- The current input data

LSTMs use a series of 'gates':
these gates control how data enters and leaves the network. Each gate acts like a filter/sieve for the information the cell is receiving. Moreover, **each gate is it's own neural network!**.

Gate Names:
1. Forget Gate
2. Input Gate
3. Output Gate


### Step 1: The Forget Gate: Filtering the Long-Term Memory

![[LSTM_forget_gate.webp]]

The forget gate decides which pieces of information are useful or not. 

The inputs to this gate are the input data and previous hidden state. we want the output of this gate to be (0,1], and in conjunction with the pointwise multiplication it will simmer down any values not worth remembering in the previous cell state and keep those that *are* worth remembering. (it forgets things!)

The forget gate network is trained in this manner and outputs a vector where values closer to 1 when it deems something as useful and closer to 0 when not.
The vector is then sent 'up' and pointwise multiplied with the previous cell state, essentially forgetting things or not changing them.

The forget gate decided which parts of the previous cell state should now be forgotten using the previous hidden state and the current input data.

### Step 2: The Input Gate: Updating Long-Term Memory

![[LSTM_input_gate.webp]]

This step involves the **new memory network** and the **input gate**. 

The goal of this step is to determine what **new information should be *added*** to the cell state using the previous hidden state and input data (same inputs as the forget gate!).

> Note: both the new memory network and input gate are neural networks themselves and both take the same inputs.

#### New Memory Network

A tanh activated neural network which has learned to generate a 'new memory update vector'. this vector in essence contains information of the current input data given the context from the hidden state.

This vector tell us how to update each component in the long term memory.

>Note: we use tanh here because we *want* the values to lie in [-1, 1]. to tell us what what matters more, *and* and what matters less in the cell state. This is necessary in reducing the impact of values in the long-term memory.

However, there is an issue with this step. When we generate the memory vector, we cannot tell if the new data generated is even important. This is the where the input gate comes along.

#### Input Gate

The input gate is a sigmoid activated network which acts as a filter (the same as the forget gate), and determines which values in the memory vector are worth remembering and which not.
It then acts as a filter through pointwise multiplication.

An output close to 0 means we don't want much of that value and closer to 1 means that value is worth remembering.

#### Combining Step

After the outputs of the memory vector and input gate are pointwise multiplied we then pointwise add this filtered memory vector to the long-term memory to achieve our new cell state (updating the long-term memory).

### Step 3: The Output Gate: Updating Hidden State

![[LSTM_output_gate.webp]]

Now that our updates to the long-term memory are complete we can move to the **output gate**, deciding the **new hidden state**.

In comparison to the previous 2 steps we will need an extra piece of information, the freshly updated cell state.

> One might think that we could just output the updated cell state; however, this would be comparable to someone unloading everything they had ever learned about the stock market when only asked if they think it will go up or down tomorrow!

To prevent this we, again, use a filter exactly the same as the forget gate network. The inputs are the same: previous hidden state and current input data and the activation is also sigmoid (to get the filter property of (0,1]).

We want to apply this filter to the newly updated cell state. This ensures only necessary information is passed through to the next cell. However, before passing the long-term memory we apply a tanh on the values to force them into the interval [-1, 1].

Step-by-Step this is the final process:
- Apply the tanh to the current cell state pointwise to acquire the squished cell state which now lies between [-1, 1].
- Pass the previous hidden state and the current input data through a sigmoid activated neural network, to obtain the filter data.
- Apply this filter to the cell state using pointwise multiplication.
- Output the new hidden state!!

### Clarifications and Final Thoughts

Although Step 3 is the final step there are still a few more things that need to be noted.

1. The steps above are repeated many times. For example, if you are trying to predict the following days stock price based on the previous 30 days pricing data, then the steps will be repeated 30 times. In other words your model will have iteratively produced 30 hidden states to predict tomorrow's price.

2. But, the output is still a hidden state. In the stock market example we wanted tomorrow's price, we can't make any money off of tomorrow's hidden state! :joy: And so, to convert this hidden state to the ouput, we actually need to apply a linear layer as the very last step in the LSTM process.
> This linear layer step only happens once, and at the very end, which is why it is often not included in diagram of an LSTM cell.
> In many resources in fact it is not even mentioned at all. Which puzzled the author initially (and probably me too, had he not mentioned it).