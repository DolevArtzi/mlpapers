# Feature Hashing for Large Scale Multitask Learning
*I'm going to take a different approach on this paper, because it looks very interesting and also up my alley, considering the algorithms class I'm currently taking. The plan is to write down everything useful, incl. the definitions, major results, concentration inequalities used, etc. I will carefully avoid reading the proofs, and I will try to prove everything myself before reading and veriyfying my proofs, so bear with me...*

## **Abstract**
    Hashing has proven empirically useful for dimensionality reduction and nonparametric estimation. This paper shows (for the first time) exponential tail bounds for feature hashing, and demonstrates success in a new use case: multitask learning with a very large task set
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
Then, we define the parameterized inner product as expected by
$$ \langle x,x'\rangle_\phi = \langle\phi^{h,\xi}(x),\phi^{h,\xi}(x')\rangle  \tag{3}$$

### Lemma 2: Unbiased Mean, $O(\frac{1}{m})$ Variance for Unit Vectors
- Mean: $E_\phi [\langle x,x'\rangle_\phi] = \langle x,x'\rangle$
- Variance: $\sigma^2_{x,x'} = \frac{1}{m} \sum_{i \neq j} (x^2_i x^2_j + x_i x_i' x_j x_j')$