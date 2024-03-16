## `First Foray Into Clustering`
- running example: `israel_clustering.ipynb` (not currently committed)
## `lay of the land`
- we're working with a dictionary called `tweet_texts` of this form:
 ``` python
tweet_texts['1768728482299670745'] = 'BREAKING\n\nPoland France and Germany announce theyre creating an international longrange rocket artillery coalition for Ukraine\n\n'
```
- we have a set called `keywords`. Here is a piece of it:
``` python
list(keywords)[:15]
> ['hebrew', 'zion', 'jewish history', 'meir', 'netanyahu', 'classified', 'israeli', 'transjordan', 'palestine', 'idf', 'canaan', 'jews', 'medina', 'muhammad', 'judaea']
```
- the first thing we'll try is to create `embs : list[np.array]` with shape `|israel_tweets| x |keywords|` such that `embs[i][j]` is the number of occurences of the `jth` keyword in the `ith` tweet in other words, we're treating our keywords as categories and see what we can do from there. This is a crude first attempt, and just uses `collections.Counter` to count how many keywords occur in the text of each tweet


## `rough gameplan`
- embed `t` in `tweet_texts` after a preliminary filtering
- clustering on the embeddings
- then try appending matching categories
    - compare the two 

### `details`
0. Embed tweet texts into vectors
1. Naive `k-means` clustering [[link](https://en.wikipedia.org/wiki/K-means_clustering)]
    - `elbow method` [[link](https://en.wikipedia.org/wiki/Determining_the_number_of_clusters_in_a_data_set)]
        - then swap for `silhouette method`
    -  `preliminary keyword filtering` initialization 
        - then swap for `random partition` init.
2. `Soft (fuzzy) clustering` [[link](https://en.wikipedia.org/wiki/Fuzzy_clustering#Fuzzy_c-means_clustering)]
    - use the best techniques from before
3. Other methods (add)
    - `hierarchical clustering` [[link](https://en.wikipedia.org/wiki/Hierarchical_clustering)]
        - see `divisive clustering`
    - other ways to select optimal number of clusters in python [[link](https://towardsdatascience.com/cheat-sheet-to-implementing-7-methods-for-selecting-optimal-number-of-clusters-in-python-898241e1d6ad)]

#### `questions`: 
- train/test splits

## `k-means clustering`
> Given a set of observations $(x_1,...,x_n)$, where each observation is a $d$-dimensional real vector, **k-means clustering (kmc)** partitions the $n$ observations into $k$ sets $S = \{S_1, S_2, ..., S_k\}$ so as to *minimize the within-cluster sum of squares* (WCSS), or variance. Formally, this means we want to find the following $S$:
> $$\argmin_S \sum\limits_{i=1}^k \sum_{x\in S_i} ||x - \mu_i||_2^2 = \argmin_S \sum\limits_{k=1}^k |S_i| \text{Var}S_i$$
> where $\mu_i$ is the mean/centroid of points in $S_i$, or $$\mu_i = \frac{1}{|S_i|} \sum_{x\in S_i} x$$
> note that this is equivalent to minimizing the *pairwise squared deviations of points in the same cluster*:
> $$\argmin_S \sum\limits_{i=1}^k \frac{1}{|S_i|}\sum_{x,y\in S_i} ||x-y||^2_2$$
>  via the following identity:
> $$ |S_i| \sum_{x\in S_i} ||x- \mu_i||^2_2 = \frac{1}{2} \sum_{x,y \in S_i} ||x-y||^2_2$$
> Since total variance is constant, this is equivalent to **maximizing** the sum of squared deviations between points in **different** clusters (due to the law of total variance [link](add))
### `k-means algorithms`
> #### `[a]g] naive k-means`
> Given an initial set of $k$ means $(m^{(1)}_i)_{i\in[k]}$, the algorithm proceeds by alternating between two steps:
> 1. **Assignment**: Assign each observation to the cluster with the nearest mean (the one with least squared 2-norm). Mathematically,
> $$S_i^{(t)} = \{x_p : ||x_p - m_i^{(t)}||^2_2 \leq ||x_p - m_j^{(t)}||^2_2 \text{  }\forall j \in [k] \}$$
> 2. **Update**: Recalculate centroids for observations assigned to each cluster:
> $$m_i^{(t+1)} = \frac{1}{|S_i^{(t)}|}\sum_{x_j \in S_i^{(t)}} x_j$$
> - multiple initial configs/iterations are usually tried

> ### `the first non-trivial problem I ran into`
> first, let me describe what I've done up to this point
> - I have `israel_tweets`, `keywords` and `embs` as defined above. I have two functions: `naive_k_means`, which is the meat of the code, and a generic `k_means` function. I also normalize each `embs[i]` that is not all `0`s (**should extend**) so that it's total is `1`
> #### `naive_k_means(data,init_ms,k)`
> ##### `params`:
> - `data : list[np.array]`: a list of *embeddings of our data points* of length `n`, with `|data[i]| = k`
> - `init_ms : list[float]`: a list of length `k` such that `init_ms[i]` is the *initial mean for the `ith` cluster*
> - `k : integer`: the *number of categories* to partition our data into
> ##### `returns`: 
> - as I came to write this line of documentation I found a severe bug in my code, which certaintly was a contributor if not the sole contributor to the problem I was facing. Here was my code at the time for `naive_k_means`:
> ``` python
>  def naive_k_means (data,init_ms,k):
>   S = [[]] * k
>   running_means = init_ms[:]
>   changed = 1
>   iters = 0
>   while changed >= 1: 
>       changed = 0
>       min_norm = 2**10 #arbitrary
>       min_idx = 0
>       for x_p in data:
>           l  = []
>           for j in range(k):
>               metric = np.linalg.norm(x_p - running_means[j]) ** 2
>               if metric < min_norm:
>                   min_norm = metric
>                   min_idx = j
>                   changed += 1
>           S[min_idx].append(x_p)
>       for i in range(k):
>           running_means[i] = 1/(len(S[i])) * np.sum(S[i])
>       iters += 1
>   return S
> ```
> I'll tell you the problem. Try to find the bug. It's not too hard to spot, in hindsight. The stopping condition in **[Hartigan 79]** was when you have an iteration with no changes. I tried that, and found that it would take incredibly long (longer than I was willing to let it run), even with `|keywords| = 14` and `|israel_tweets| = 200`: $$\textbf{Problem: my algorithm would not reach the stopping condition even on very small datasets}$$
> - **why?** I think it has to do with the **sparsity of the embeddings matrices** (in other words, even `14` is too many categories for this particular dataset)
> #### `solutions that come to mind`
> - the first thing I thought of, which should likely be the first thing you try if you want some answers about your data asap, is to reduce `k`. The reasons I'm not starting with this seemingly easy fix are first because the way I've written my code just assumes that every dimension is either `n` or `k` long, that is, the number of keywords is used as the number of clusters throughout the code. I can change this, but the second reason not to do this now is that *its way more interesting to figure out how to get around this type of problem in general*
> - that leads me to the first fix I thought of, admittedly while I was pretty tired: instead of waiting for there to be no changes in the assignments of data points to clusters, why not set some sort of sensitivity threshold below which we break from the loop? I haven't programmed this yet, but I have some ideas. One **question** I have is whether to use `S` or `running_means` to determine when we're below the threshold.
> - the next thing I remembered was **importance sampling**, or **leverage score sampling**, which are technique I've been learning in `15-851: algs. for big data` (the namesake of the [code repository](https://github.com/DolevArtzi/algsforbigdata) that this document accompanies) that are very good at dealing with **random sampling over sparse data structures**
> - as I'm writing this now, I realized that it's probably simplest to just **set a constant limit** on the number of iterations of the algorithm. After a moment's reflection, I decided that this probably didn't come to mind, and that's likely informed by the papers I've been reading on this topic, and my experience with the theory of reinforcement learning, where we have convergence considerations frequently, and algorithms like [`ucb for stochastic multi-armed bandits`](https://www.cs.cornell.edu/courses/cs6783/2021fa/lec25.pdf) which may have deterministic, but not constant, iterations, which if I recall correctly often look like $O(n \log n)$ or $O(n \sqrt{n})$. We'll try that first, if fixing the bug I just discovered doesn't immediately work
> #### `the bug`
<details><summary> reveal </summary>

 I append to one of the lists in `S`, `S[min_idx]`, `n` times per iteration of the `while` loop, but I never **remove from or reset** the clusters between iterations of the outer loop, so my clusters can grow exponentially in the number of iterations of the `while` loop. This certainly made my code incredibly slow
</details>

> #### `fixing it`
> - after fixing that bug, my code was immediately significantly faster, but still, every thousand iterations for the first few thousand, there were between 2 and 4 but usually 3 times that we encountered a better cluster in the `for` loop, across all the data. This may not be an entirely correct way to measure change, though, so we'll try something more precise, but first, let's try actually capturing what we want: the number of data points that are reassigned between iterations

> ### `common initialization methods`
> #### `basic methods`
>    - randomly choose points (that may be what `random partition` is, I'm not sure yet)
> - do initial filtering (like what is described above, the first thing I'm trying)
> #### `Hartigan 79` ([one of the original k-means papers [Hartigan 1979]](./HartiganKMeans.pdf))
> - as suggested in  we choose our initial means as follows:
> 1. points are ordered by their distances to the overall mean of the sample
> 2. for cluster $L : L \in [k]$, we choose the $1+(L-1)\cdot(M/K)^{th}$ point to be its initial center
> * guarantees that no cluster will start empty
> #### `random partition`


# weighted sampling (Leverage score, importance sampling): sparse!!!!
>