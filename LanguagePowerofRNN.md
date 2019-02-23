# RNN Formal Language Comptuational Power.

This week's paper is a short one are both by Weiss, Goldberg, and Yahav: ["On the practical Power of Finite Precision RNNs of for language recognition"](https://arxiv.org/abs/1805.04908).

## Power of Finite Precision RNNs

Theoretically, RNN's are Turing complete. In practice, machines are limited in their performance at varying levels of the Chosky Hierarchy by computational power and numerical presicion. With such limitations, different RNN variations are have vary in formal language learning capacity. This paper considers 4 RNN variations: Elman RNN (SRNN) with a squashing activation, SRNN with ReLU, LSTM, and GRU, focusing on the latter two machines, shows that LSTM and GRU are not equivalent, though one might assume them to be. 

Section 2 of this paper has a beautiful explanation of RNN's and the functions used in GRU and LSTM models, for those who are looking for such things. This section also mentions that SRNN's have been proven capable of representing finite state languages (presumably with the computation and precision restrictions mentioned above), and because both GRU and LSTM are strictly stronger than SRNN, the paper jumps straight to consideration context sensitive and context free languages and those recognized by k-counter machines. 

K-counter machines cut across Chomsky's language hierarchy. These are finite state automata with some number, k, of counters which the machine's transition rules can increment, decrement,ignore, or compare to 0. K-counter machines can recognize the context free a^n b^n and the context sensitve a^n b^n c^n, but not the context sensitive language of palindromes S -> x | aSa | bSb. (pg 3) The remainder of the papers shows that LSTM and ReLU SRNN can recognize k-counter languages, while squashed SRNN and GRU cannot. The difference in internal state between GRU and LSTM is shown quote nicely in figure 1. 

Section 4 covers the theoretical power of the 4 machines. Broadly, the squashing activations in GRU and SRNN mean that infinite counting cannot occur with the finite precision which is a part of the premise of this paper. SRNN with ReLU, can perform unbounded counting because ReLU has no upper bound, and LSTMs are able to count by adding 1 or -1 to cell memory dimenions when appropriate as determined by inputs. 

Section 5 illustrates this theory with experimental results, in which LSTMs learned generalized well to large n on both a^nb^n (n<=256) and a^nb^nc^n (n<=100), while GRU's were able to recognize these for some n's (n<=38 and n<10) but did not generalize well and did not have internal states which could be shown to be counting. 

They do not report any investiation of RNN perfmormance on palindromes.
