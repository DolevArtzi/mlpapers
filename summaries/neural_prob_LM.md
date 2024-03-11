### A Neural Probabilistic Language Model [link](https://www.jmlr.org/papers/volume3/bengio03a/bengio03a.pdf)
##### **Citation**:  Y. Bengio, R Ducharme, Pascal Vincent, and Centre Mathematiques. A neural probabilistic language model. 10 2001.
___
- address curse of dimensionality head on
- distribute probability mass where it matters rather than uniformly in all directions around each training point
- **Statistical Model** represented as conditional probability of next word given all previous ones: $$\hat{P}(w_1^T) = \prod\limits_{t=1}^T \hat{P}(w_t | w_1^{t-1})$$
- here, $w_t$ is the $t^{th}$ word, $w_i^j = (w_i, ..., w_j)$.
- n-Gram model approximation: $\hat{P}(w_1^t|w_1^{t-1}) \approx \hat{P}(w_t|w_{t-n+1}^{t-1})$
- Goodman (2001): combining many tricks for substantial improvements over interpolated trigram models
## Summary of Approach
1. map each word in vocab to *word feature vector* in $\mathbb{R}^m$ (experiments: $m = 30, 60, 100, n = 17000+$)
2. express the joint probability function of word sequences as a smooth function of the feature values in terms of the vectorized words in the sequence
3. simulateously learn the word feature vectors and the parameters of that probability function
- probability function has parameters that can be iteratively tuned to *maximize log-likelihood of the training data*.
- semantically similar words are expected to have similar feature vectors
## Neural Model

### Set up
- training set: $w_1, ..., w_T, w_i \in V$, $V$ is the vocabulary
- goal: learn $f(w_t, ..., w_{t - n + 1} = \hat{P}(w_t|w_1^{t-1}))$ so that it performs well on out-of-sample likelihood
- perplexity: $1/ \hat{P}(w_t|w_1^{t-1})$
- constraint: $f > 0, \sum\limits_{i=1}^{|V|} f(i, w_{t-1}, ..., w_{t - n + 1}) = 1$
- define $C: V \rightarrow \mathbb{R}^m$, maps to distributed feature vectors
- define a function $g$, mapping input sequence of feature vectors to conditional probability distribution for next word in $V$ that's in the  current given context: $(C(w_{t-n+1}), ..., C(w_{t-1}))$
    - Output: vector whose $i^{th}$ element estimates $\hat{P}(w_t = i | w_1^{t-1})$
- $f$ is composed from these two parts:
$$ f(i, w_{t-1}, ..., w_{t - n +1}) = g(i,C(w_{t-n+1}), ..., C(w_{t-1}))$$
- basically, $f(i, \cdot, ...)$ is the estimated probabilty by our neural network $g$, working in the feature space, when we add word $i$ to our $n$-gram context
- $g$ has a parameter, say $\omega$, and the parameter set is $\theta = (C,\omega)$
### Training
$$ L = \frac{1}{T} \sum\limits_t \log f(w_t,w_{t-1}, ..., w_{t-n+1};\theta) + R(\theta), \text{where $R$ is a regularization term}$$
- e.g. $R$ is a weight-decay penalty applied to weights of $g$ and to $C$
- **number of free parameters only scales linearly with $V$ and $n$**
    - the authors note that the sharing factor on $n$ can be improved to sub-linear with e.g. a recurrent neural network (?)
- most experiments in paper: NN has one hidden layer beyond word features mapping, or direct connection from word features to output
    - in other words, two hidden layers: $C$'s layer which has no non-linearity, and $\tanh$ layer.
$$ \text{Log Probs.: } \gamma_{(i)} = b + Wx + U \tanh(d + Hx)$$
#### Training-Related Parameters
- we have $W$ optionally zero (no direct connects), $x$ is the word features layer activation vector, or $(C(w_{t-1}), ... , C(w_{t-n+1}))$, and the $\tanh$ is element-wise 
- $h$: number of hidden units
- free parameters of the model: $b$ (output biases, $|b| = |V|$), hidden layer biases $d$ ($|d| = h$), hidden-to-output matrix $U: |V| \times h$, word features to output weights $W: |V| \times (n-1)m$, the hidden layer weights $H: h \times (n-1)m$ and word features $C: |V| \times m$

$$ \theta = (b,d,W,U,H,C)$$
- number of free parameters: $|V|(1+nm+h) + h(1 + (n-1)m) \in O(|V|(nm+h))$
    - with weight decay, $W, H$ could converge to 0 while $C$ blows up, not observed experimentally with stochastic gradient descent

### Back to Training
- After obtaining log probs., we can normalize with softmax and use that as our estimate: 
$$ \hat{P}(w_t|w_{t-1}, ..., w_{t-n+1}) = \frac{\exp{(\gamma_{w_t})}}{\sum\limits_i \exp{(\gamma_i)}}$$
- Finally, we update our parameters $\theta$ with stochastic gradient descent as follows:
$$ \theta \leftarrow \theta + \epsilon \frac{\delta\log \hat{P}(w_t|w_{t-1},...,w_{t-n+1})}{\delta\theta}$$
- note: the word features $C(j)$ of all words $j$ that don't appear in the input window doesn't need to be updated after each example