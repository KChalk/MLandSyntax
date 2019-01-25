# LSTM Hochreiter and Schmidhuber '97 and Gers and Schmidhuber '01

We're starting this review of syntax-y machine learning papers with [*the* LSTM paper](https://www.bioinf.jku.at/publications/older/2604.pdf "Hochreiter and Schmidhuber '97") and jumping from that to [Gers and Schmidhuber's](https://ieeexplore.ieee.org/document/963769 "IEEE 2001 Paper") inquisition of what formal language types LSTM's can learn. On to business, the introduction to LSTM's...

## 1997 and the beginning of the LSTM machine

### 0:1 Abstract and Intro
Using basic back propagation through time (BPTT), standard RNNs can have a hard time learning long distance relationships. LSTM's can address this by providing a shortcut for error to flow through to long distance history representations in a way to previous architecture could. Important jargon for this includes: constant error carrousels (CECs), gate units. 

It can be difficult when moving form simple feedforward networks to recurrent structures to keep track of the unit of analysis. For this outline at least, let a *node* be a unit in a feed forward network associated with an arrray of weights to the next layer in the network, and let a *cell* be a unit in a reccurrent network consisting of one or more smaller units, each of which may include multiple feed forward layers connected to various inputs and outputs. 

### 2 Previous Work
I'm skipping this section because this paper outline sequence is basically already a review of previous work. Those interested can read this section in  [the paper itself.](https://www.bioinf.jku.at/publications/older/2604.pdf "Hochreiter and Schmidhuber '97") For now, suffice it to say that other methods of addressing this issues have been tried with limited success. 

### 3 Constant Error Backprop
#### 3.1 Exponentially Decaying Error 

Conventional backpropagation changes the weights for each node in a the network based 1. on the size of the error in the machine's prediction and 2. on the magnitude of the node's contribution to that prediction. The latter value is calculated through a series of dervatives. The contribution of a node in the final layer of a network is simply the derivative of the 

- I'm not sure whether to talk about the node's contribution or the weights contribution. I'll try to come back to it later this weekend. but this has been an hour of writing... 







--- 

### A Post Script for things I'd like to learn eventually
- How does attention's attempted replacement of RNN's relate to LSTMs?
- Do word embeddings learn TAM and PHI Features (no it's not related, but I still want to remember it)
