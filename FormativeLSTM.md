# LSTM Hochreiter and Schmidhuber '97 and Gers and Schmidhuber '01

We're starting this review of syntax-y machine learning papers with [*the* LSTM paper](https://www.bioinf.jku.at/publications/older/2604.pdf "Hochreiter and Schmidhuber '97") and jumping from that to [Gers and Schmidhuber's](https://ieeexplore.ieee.org/document/963769 "IEEE 2001 Paper") inquisition of what formal language types LSTM's can learn. On to business, the introduction to LSTM's...

## 1997 and the beginning of the LSTM machine

### 0:1 Abstract and Intro
Using basic back propagation through time (BPTT), standard RNNs can have a hard time learning long distance relationships. LSTM's can address this by providing a shortcut for error to flow through to long distance history representations in a way no previous architecture could. Important jargon for this includes: constant error carrousels (CECs), gate units. 

It can be difficult when moving from simple feedforward networks to recurrent structures to keep track of the unit of analysis. For this outline at least, let a *node* be a unit in a feed forward network associated with an array of weights to the next layer in the network, and let a *cell* be a unit in a reccurrent network consisting of one or more smaller units, each of which may include multiple feed forward layers connected to various inputs and outputs. 

### 2 Previous Work
I'm skipping this section because this paper outline sequence is basically already a review of previous work. Those interested can read this section in  [the paper itself.](https://www.bioinf.jku.at/publications/older/2604.pdf "Hochreiter and Schmidhuber '97") For now, suffice it to say that other methods of addressing this issues have been tried with limited success. 

### 3 Constant Error Backprop
#### 3.1 Exponentially Decaying Error 

Conventional backpropagation changes the weights for each node in a the network based 1. on the size of the error in the machine's prediction and 2. on the magnitude of the node's contribution to that prediction. The latter value is calculated through a series of dervatives. The contribution of a node in the final layer of a network is simply the derivative of the 

*** this section is unfinished *** 

### 4 Long Short Term Memory 
A very important section which I will definitely come back to.

### 5 Experiments

To test the performance of the new architecture, we need experiements which involve long time lags between signals for all training sequences. When networks are trained with a combination of long and short lags, they can learn to generalize patterns in the shorter signals to larger ones, and LSTMs are designed for the harder task of training only on long lags. Additionally, the authors note that some past architectures have been tested on tasks simple enough to be solved by random weight guessing. The exact process of training with rancom weights, is unclear to me, but suffice it to say that it fails to solve the more complex problems we would like LSTMs to solve, and as such tests which cannot be solved by guessing are included. 

As such, the experiments used in this paper in general involve long minimal time lags, and sparse weight space solutions ie. problems that cannot be solved with guessing. Futher similarities between experimentsa re that all models are trained one datum at a time, all activation functions are logistic sigmoids, all weights are intialized in a range -.2 to .2 or smaller. Architectures use between 1 and 4 cell blocks in accordance with complexity of the problem and or prior knowledge of the memory task. At each step LSTM is compared to networks trained with RTRL, a modified version of which is also used to train the LSTMs themselves. 


There is a line in this section "Net activations are reset after each processed input sequence," and I'm not entirely certain what that means.

Networks are compared in 6 'experiments' in which they predict next characters given the sequence so far and the shape of sequences in the training data. LSTM's performed consistently in all experiements, learning even the most difficult versions of every task. (As of 1997, no other networks had succeeded at tasks 4-6). Architectures never used more than 6 memory cells organized into 1-4 blocks (see table 10 p21), and many tasks may be learnable on smaller architectures, given long training times. The discussion section (6) lists some situations in which LSTMs do not perform well (eg. on smaller sequences), but in general the model learns long distance relationships in sequences of substantial length better than any of its predecessors. The experiments themselves are summarized below.


- 1 Embedded Reber Grammar: 
    - _alphabet_: {B,P,T,S,X,V}
    - _sequence_: " B 1 B [regular language] E 1 E " where '1' is either 'T' or 'P'
    - _goal_: for every time step, correctly identify all symbols which would be gramattical in the next time step.
    - _challenge_: the machine must remember the second symbols over the entire length of the string in addtion to remembering the rest of the regular language. (Both T and P can also occur in the embedded language. See pg 11 figures 3 and 4 for the complete diagram)

- 2 Remembering first character
    - a) *noise free* (50% accuracy or less for RNNs and non LSTM, 97% or better for LSTM) 
        - _alphabet_: {a<sub>1</sub> ... a<sub>p-1</sub>, x, y} 
        - _sequence_: "1, a<sub>1</sub> ... a<sub>p-1</sub>, 1 " where 1 is either 'x' or 'y' 
        - _goal_: predict next symbol
        - _challenge_: learn alaphabet order and remember first symbol for length of input

    - b) *without internal regularity* (only LSTM and Chunking learn when |alphabet| > 100, Chunker 33% accuracy, LSTM 100%)
        - _alphabet_: {a<sub>1</sub> ... a<sub>p-1</sub>, x, y} let R be a randomly selected symbol from a<sub>1</sub>- a<sub>p-1</sub> 
        - _sequence_ : "1, R<sub>1</sub> ... R<sub>p-1</sub>, 1 " where 1 is either 'x' or 'y' 
        - _goal_: predict next symbol, with absolute error < .25 over entire sequence
        - _challenge_: only fist and last symbols are non-random, remember first symbol for length of input, without compression offered by 2a

    - c) *long time lag w/o regularity* (only solved by LSTM)
        - _alphabet_: {a<sub>1</sub> ... a<sub>p</sub>, e, b, x, y}, a<sub>1</sub> ... a<sub>p</sub> are called distractor symbols, b=beginning, e= end. let R be a randomly selected distractor symbol
        - _sequence_: " b, 1, R<sub>1</sub> ... R<sub>q+k</sub>, e, 1 " where 1 is either 'x' or 'y' and R is selected from a subset of the first q distractor symbols. The first q+2 symbols are randomly selected from that subset and every symbol has a 10% chance of being the 'e' symbol. When an 'e' occurs, the next symbol is the same as the second symbol in the sequence and the sequence ends. 
        - _goal_: predict last symbol
        - _challenge_: only very long lag examples are included in training. The expected length of the sequence is q+14. (p14) 
- 3 Noisy signal
    - a) (can be learned by random weight guessing faster than LSTM or any other network)
        - _alphabet_: reals from gaussian with mean 0 and var .2, 1, -1
        - _sequence_: approximately (see paper for details) two 1's followed by ~T reals or two -1's followed by ~T reals
        - _goal_: classify sequence 1 if first digits are 1, and 0 if they are -1. 
    - b) 
        - _alphabet_: reals from gaussian with mean 0 and var .2, 1 +gaussian noise with mean 0 and standard dev .1, -1 +gaussian noise with mean 0 and standard dev .1
        - _sequence_: same as 3a with noisy signals
        - _goal_: classify sequence 1 if first digits are ~1, and 0 if they are ~-1. 
    - c) 
        - _alphabet_: reals from gaussian with mean 0 and var .2, 1 +gaussian noise with mean 0 and standard dev .1, -1 +gaussian noise with mean 0 and standard dev .1
        - _sequence_: same as 3a with noisy signals
        - _goal_: output .2 if first digits are ~1, and .8 if they are ~-1. 
        - _challenge_: training targets are also altered by addition of gaussian noise    

- 4 Adding Problem
    - _alphabet_: (x,y) where x is a random real form [-1,1] and y -1 for first and last chars, 1 for marked chars, and 0 elsewise
    - _sequence_: (a<sub>1</sub>, -1), (a<sub>2</sub>, 0),  ..., (X<sub>1</sub>, 1) ... (X<sub>2</sub>, 1) ... (a<sub>N-1</sub>, 0), (a<sub>N</sub>, -1) where N is between T and T+(T/10), X<sub>1</sub> is randomly positioned amoung the first 10 elements, and X<sub>2</sub> is randomly positioned among the first T/2 elements. (T is 100, 500, or 1000)
    - _goal_: calculate .5 * (X<sub>1</sub> + X<sub>2</sub>)/4 (sum of  X's scaled to 0,1)
    - _challenge_: remember sum of two continuous values over a long interval

- 5 Multiplication Problem
    - _alphabet_: (x,y) where x is a random real form [0,1] and y -1 for first and last chars, 1 for marked chars, and 0 elsewise
    - _sequence_: (a<sub>1</sub>, -1), (a<sub>2</sub>, 0),  ..., (X<sub>1</sub>, 1) ... (X<sub>2</sub>, 1) ... (a<sub>N-1</sub>, 0), (a<sub>N</sub>, -1) where N is between T and T+(T/10), X<sub>1</sub> is randomly positioned amoung the first 10 elements, and X<sub>2</sub> is randomly positioned among the first T/2 elements. (T = 100)
    - _goal_: calculate X<sub>1</sub> * X<sub>2</sub>
    - _challenge_: remember product of two continuous values over a long interval. (see p18 for why this is harder than addition)

- 6 Temporal Order
    - a) Two symbols 
        - _alphabet_: E, B, a, b , c, d, X, Y 
        - _sequence_: B, [abcd]\*, X or Y, [abcd]\*, X or Y, [abcd]\*, E
        - _goal_: produce output dependent on observed permutation of X and Y. XX-> Q, XY -> R, YX -> S, YY-> U
    - b) Three symbols 
        - _alphabet_: E, B, a, b , c, d, X, Y 
        - _sequence_: B, [abcd]\*, X or Y, [abcd]\*, X or Y, [abcd]\*, X or Y, [abcd]\*, E
        - _goal_: produce output dependent on observed permutation of X's and Y's. XXX-> Q, XXY -> R, XYX -> S...


## 2001 and LSTMs on Context Free/Sensitive Grammars

This paper reviews the basics of LSTM architecture and adds to it a few inovations, specifically forget gates and peephole connections. Roughly, forget gates allow the machine to reset the LSTM's long distance memory when information becomes out of date, and peephole connections allow gates to access that memory before the output of gates is combined with the cell state to calculate cell output. Using this architecture, the authors empirically show that LSTMs can learn structures as complex as context sensitive grammars in terms of the Chomsky lanugae heirarchy. 

### Experiments

In each of three experiements, LSTMS are fed sequences one symbol at a time and required to predict all possible characters in at the next timestep. The three languages are:

- a<sup>n</sup> b<sup>n</sup>
    - trained on n's between 1 and N and tested on n between 1 and M where M is larger than N but less than 1000. 
- a<sup>n</sup> b<sup>m</sup>B<sup>m</sup> A<sup>n</sup>
    - trained on 
        - n and m from 1 to 11 constrained such that n+m less than 12 
        - n and m from 1 to 11 (unconstrained)
    - tested on n and m from 1 to 50
- a<sup>n</sup> b<sup>n</sup>c<sup>n</sup>
    - trained on 
        - n between 1 and 500 
        - n between N-1 and N  (N is between 1 and 500, but within a training set, all lengths are similar) 
    - tested on n and m from 1 to 500

Each sequence also starts with 'S' and ends with 'T'. So for the context sensitive language, given 'S', the correct prediction is 'a/T', after which inputs will be 'a' and outputs should be 'a/b' until a 'b' is seen, after which point the network should accurately predict the exact number of 'b's and 'c's needed to finish the sequence before outputing 'T'. 


### Results

LSTMs were able to learn each of the langauges, though results vary in terms of the necessary amount of training and the size of the test strings to which the network generalized well. The remainder of the paper is worth exploring for analysis of how the network learned succeed at each of these tasks. 


