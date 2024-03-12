## Feature Hashing for Large Scale Multitask Learning 
##### Kilian Weinberger, Anirban Dasgupta, John Langford, Alex Smola, and Josh Attenberg. Feature hashing for large scale multitask learning. In Proceedings of the 26th Annual International Conference on Machine Learning, ICML’09, page 1113–1120, New York, NY, USA, 2009. Association for Computing Machinery.
---
*I'm going to take a different approach on this paper, because it looks very interesting and also up my alley, considering the algorithms class I'm currently taking. The plan is to write down everything useful, incl. the definitions, major results, concentration inequalities used, etc. I will carefully avoid reading the proofs, and I will try to prove everything myself before reading and veriyfying my proofs, so bear with me...*

## **Abstract**
    Hashing has proven empirically useful for dimensionality reduction and nonparametric estimation. This paper shows (for the first time) exponential tail bounds for feature hashing, and demonstrates success in a new use case: multitask learning with a very large task set
## **Main Contributions**
1. specialized hash functions with unbiased inner products (**Lemma 2**), applicable to kernel methods
2. exponential tail bounds (on distortion by hashing, I believe) that support strong empirical results
3. negligible interference between hashed subspaces w.h.p., good for large scale multi-task learning
4. novel application: collaborative email spam filtering

## **Kernel Methods**
$$ k(x_i,x_j) = \langle\phi(x_i),\phi(x_j)\rangle\tag{1, kernel-trick}$$
- inner product to measure similarity between vectors/objects
where $x_i,x_j \in \chi$ and $\phi$ is a feature extractor
- allows implicit computation through defn. of PSD kernel mtx. defined by $k: \mathbb{'a} \times $ $'a \rightarrow \mathbb{R}$ without directly computing $\phi(\cdot)$... (*add more*)
### **Consideration on Input vs Feature Space Size in Kernel Methods**
    In classification tasks, kernel methods have had success at taking non-linear decision boundaries in the original space to linearly separable feature spaces. Implicit computation of the kernel can be useful when the feature dimension is very large and thus expensive. In practice, however, we often have the opposite problem: not too many features, but too much data/size in the input space, either count or dimensionality
- **Solution**: **hashing-trick** (hashing to a lower dimensional vector space, e.g. $\mathbb{R}^n \rightarrow \mathbb{R}^d$, with $d \ll n$), nicely preserves *sparsity*
## **Hashing Methods**
### Hash Functions (note that this is extremely similar to CountSketch, right?)
- $h: \mathbb{N} \rightarrow [m]$ 
- $\xi: \mathbb{N} \rightarrow \{-1,1\}$, removes bias from (Shi, 2009)
$$ \phi^{(h,\xi)}_i(x) = \sum_{j : h(j) = i} \xi(j) x_j \tag{2, hashing-`trick'}$$
In English, to obtain $x$'s hashed feature map, The $i^{th}$ hashed feature output is a linear combination of all $x_j$'s s.t $h$ hashes $j$ to bucket $i$, 'weighted' by $\xi(j)$'s, which optionally flips the sign of $x_j$ (for $j \in [1,\textbf{len}(x)]$)
In other words, to obtain $(\phi(x))_i$, this protocol just hashes each index $j$ to a bucket in the feature space (indexed by $i$) and sums over the $x_js$  in the $i^{th}$ bucket, *flipping a fair coin for each $x_j$ before summing*
- *add the $\phi_0$ thing*

Then, we define the parameterized inner product as expected by

$$ \langle x,x'\rangle_\phi = \langle\phi^{h,\xi}(x),\phi^{h,\xi}(x')\rangle  \tag{3}$$
Let's look at this expression more carefully. 

$$\lparen \langle x,x' \rangle_{\phi}\rangle \rparen_i = \lparen \sum_{j : h(j) = i}\xi(j)x_j \rparen \lparen \sum_{j : h(j) = i}\xi(j)x'_j \rparen$$
___
### Lemma 2: Unbiased Mean, $O(\frac{1}{m})$ Variance for Unit Vectors Under the Hash Kernel
- $(2.1)$ **Mean**: $E_\phi [\langle x,x'\rangle_\phi] = \langle x,x'\rangle$
- $(2.2)$ **Variance**: $\sigma^2_{x,x'} = \frac{1}{m} \sum_{i \neq j} (x^2_i x^2_j + x_i x_i' x_j x_j')$
- $(2.3)$ Variance implies $\sigma_{x,x'}^2 = O(\frac{1}{m})$ for unit vectors $x,x'$
### *Proof*
We'll start by reproducing $(9)$ here:

$$ \langle x,x' \rangle_\phi = \sum_{i,j} \xi(i)\xi(j)x_i x_j' \delta_{h(i),h(j)} \tag{9}$$
where $\delta_{E}$ is an indicator random variable (irv) for event $E$, and $\delta_{h(i),h(j)}$ is an irv for the event that $i,j$ collide under $h$. Note that $\xi(\cdot) = 0$ by symmetry, and likewise $\xi^2(i) = 1$ $ \forall i$. So, all terms with $i \neq j$ disappear from the summation, and we obtain the following simpler form:

$$ \langle x,x' \rangle_\phi = \sum_i x_i x'_i$$
which completes the proof of $(2.1)$.

$$ Var_\phi [\langle x,x' \rangle_\phi] = E[\langle x,x' \rangle_\phi^2] - E[\langle x,x' \rangle_\phi]^2 $$
Let's modify $(9)$ for the second moment as usual ('squaring' the terms in the summation, but avoiding correlation):

$$ \langle x,x' \rangle_\phi^2 = \sum_{i,j,k,l} \xi(i)\xi(j)\xi(k) \xi(l) x_i x_j' x_k x_l' \delta_{h(i),h(j)} \delta_{h(k),h(l)}\tag{9a}$$
As we often do when proving our hash functions are nice, let's consider $\xi(i)\xi(j)\xi(k)\xi(l)$. Writing $\delta_{x_1,...,x_n}$ as the irv for $\xi(x_1)\cdot ... \cdot \xi(x_n) = 1$ (**I think?**), the authors note that $E_\xi [\xi(i)\xi(j)\xi(k)\xi(l)] = \delta_{ij}\delta_{kl} + (1 - \delta_{ijkl})(\delta_{ik}\delta_{jl} + \delta_{il}\delta_{jk})$ **(???)**
and thus rewrite $E_\phi[\langle x,x' \rangle_\phi^2]$ as 

$$E_\phi[\langle x,x' \rangle_\phi^2] = \sum_{i,k} x_i x_i' x_k x_k' + \sum_{i\neq j}x_i^2x_j'^2 E_h[\delta_{h(i),h(j)}] + \sum_{i\neq j} x_i x_i' x_j x_j' E_h[\delta_{h(i),h(j)}] \tag{9b}$$
We have that $E_h[\delta_{h(i),h(j)}] = \frac{1}{m}, i \neq j$ (this is a reasonable assumption to make about $h$, at least in theory). Noting also that the first term in $(9\text{b})$ is $\langle x,x' \rangle^2$, we rewrite $(9\text{b})$ as 

$$ \langle x,x' \rangle^2 + \frac{1}{m}\left( \sum_{i\neq j}x_i^2x_j'^2  + \sum_{i\neq j} x_i x_i' x_j x_j' \right) \tag{9c}$$
And finally,

$$ Var_\phi[\langle x,x'\rangle_\phi] = \frac{1}{m}\left( \sum_{i\neq j}x_i^2x_j'^2  + \sum_{i\neq j} x_i x_i' x_j x_j' \right) \tag{9d}$$
since the squared inner product term is subtracted off by subtracting the squared mean from the second moment. This completes the proof of $(2.2)$. For unit vectors $x,x'$, we have immediately that the variance is $O(\frac{1}{m})$, which is $(2.3)$, since the summations are bounded by 1.

___
## **Concentration Inequalities**
___
### Aside: See [this](add_link) article about probability spaces, and some concentration inequalities 
