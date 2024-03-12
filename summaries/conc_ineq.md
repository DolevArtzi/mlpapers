## **Useful Facts on Probability Spaces, Random Variables, and Concentration Inequalities**
____
> # **Table of Contents**

> ## Section 1: Probability Spaces, Common Distributions
> ### 1.1 Probability Spaces
> 1.1.1. defn\
> 1.1.2. $\sigma$-algebra
> ### 1.2 Random Variables, Expectation, Higher Moments
> 1.2.1. defns.\
> 1.2.2. Discrete Random Variables\
> 1.2.3. Continuous Random Variables\
> 1.2.4. Generating RVs\
> 1.2.5. Applications

> ## Section 2: Convex Distances, Norms, and Related Inequalities
> ### 2.1
> 2.1.1. Hamming variants\
> 2.1.2. Convex Hull/Hypercube\
> 2.1.3 Talagrand's Inequality\
> 2.1.4 Azuma
> ### 2.2
> 2.2.1 Norms, incl. Frobenius\
> 2.2.2 Hölder's Inequality\
> 2.2.3 Cauchy-Schwarz Inequality\
> 2.2.4 C-S applications\
> 2.2.5 Johnson-Lindenstrauss\
> 2.2.6 Bernstein's Inequality

> ## Section 3: Linear Algebra, Sketching, Hashing
> ### 3.1
>  SVD, norms, pseudoinverse, applications, complexity\
>  Countsketch, applications\
>  Hadamard\
>  Matrix Chernoff, applications\
> ### 3.2 
>  balls in bins etc.\
> ### 3.3
>  perfect/universal/fun hashing\

> ## Section 4: Probability Concentration Inequalities
>  Markov's\
>  Chebyshev's\
>  Random Graphs stuff\
>  Hoeffding's\
>  Chernoff's, applications\
>  CLT, WLLN, SLLN\
>  Pretty Chernoff Bounds\
>  Tail bounds, e.g. Poissons, Pareto, applications
___
### Sources
\- Mor's book(s) Pnc, Queueing
\- Bollobas RG
\- [sigma_algbera](https://www.math.lsu.edu/~sengupta/7312s02/sigmaalg.pdf)
\- [convex distances, talagrand,holders](https://jpmastrogiacomo.ca/files/Talagrand_convex_hull_concentration_inequality.pdf)
___



## Section 1.1: **Probability Spaces**
> A **probability space** $[1.1.1]$ is a triple $(\Omega,\Sigma,P)$, sample space, event space, probability measure, where $\Omega$ is a set, $\Sigma$ is a $\sigma$-field of subsets of $\Omega$, and $P$ is a non-negative measure on $\Sigma$ with $P(\Omega) = 1$ [Bollobás 1]

\- in the simplest case, $\Omega$ is finite and $\Sigma$ is $\mathcal{P}(\Omega)$

>  A $\sigma$-field $[1.1.2]$ over a collection of subsets of $X$ must contain $\emptyset$, be closed under complements, and be closed under countable union [source](https://www.math.lsu.edu/~sengupta/7312s02/sigmaalg.pdf)

\- we also have closure in the sigma algebra under complement and intersection by rewriting terms with complements and unions
    - the extreme cases being $\{\emptyset,X\}$ and $\mathcal{P}(X)$
> #### **Axioms of Probability** [HB 30] 
> 1. *Non-negativity*: $P[E] \geq 0$ for any event $E$
> 2. *Additivity*: If $E_1, E_2, ...$ is a countable sequence of pairwise disjoint events, then 
> $$P[E_1 \cup E_2 \cup ...] = \sum_i P[E_i]$$
> 3. *Normalization*: $P[\Omega] = 1$

> #### **Probability Facts** [HB 31-40]
> ##### <ins>**Union**
> $$P[A \cup B] = P[A] + P[B] - P[A\cap B] \tag{defn, \textbf{HB Lemma 2.5}}$$
>$$P[A \cup B] \leq P[A] + P[B] \tag{\textbf{Union Bound (UB)}, \textbf{Lemma 2.5}, Ax. 1}$$
> ##### <ins>**Conditional Probability, Independence**
> $$P[E \mid F] = \frac{P[E\cap F]}{P[F]} \tag{defn \textbf{Conditional Indep.}, assuming $P[F] > 0$}$$
> If $P[F] \neq 0$, events $E,F$ are **independent** if $P[E \mid F] = P[E]$, that is, we gain no information on $E$ by knowing the outcome of $F$. This is equivalent to $P[E\cap F] = P[E]P[F]$ by simple substitutions. Furthermore, events $A_1, ..., A_n$ are independent if for any subset $S \in [n]$, ($[n] = \{1,...,n\})$, $$P\lbrack \bigcap_{i \in S} A_i \rbrack = \prod_{i \in S} P[A_i] \tag{defn. of independence for finite set}$$
> We also define the less strict, but very often sufficient, **pairwise independence** for $A_1, ..., A_n$ as follows: $$\forall i,j \text{   } P[A_i \cap A_j] = P[A_i]P[A_j] \tag{defn. \textbf{Pairwise Independence}}$$
Finally, **$A,B$ are independent *given* $C$** if $$P[A \cap B \mid C] = P[A \mid C]P[B \mid C]$$

\- note that independence and conditional independence to not imply each other either way
> #### **Basic Laws of Probability**
> ##### <ins> **Law of Total Probability**
> Let $\{F_i\}_{i\in[n]}$ be a partition of $\Omega$. Then $$P[E] = \sum_{i = 1}^n P[E \cap F_i] = \sum_{i=1}^n P[E \mid F_i]P[F_i]$$
> \- *Proof:* Write $E$ as union over intersections with $F_i$, use mutal exclusivity. This law also holds if $\{F_i\}_i$ partitions $E$, and is extendable from $n$ to countably infinite
> ##### <ins> **Law of Total Probability for Conditional Probability**
> $$P[A \mid B] = \sum_{i=1}^n P[A\mid B \cap F_i] \cdot P[F_i \mid B]$$
> ##### <ins> **Bayes Law**
> Assuming $P[E] > 0$, $$P[F\mid E] = \frac{P[E\mid F]P[F]}{P[E]}$$
> \- useful in basic problems or teasers

## Section 1.2: Random Variables, Expectation, Higher Moments
- $[1.2.1]$ a **random variable** (rv) is a real-valued function of the outcome of an experiment involving randomness (HB 50)
- For a discrete random variable (drv) $X$, we have the following:
$$p_X(a) = P[X = a] \text{ where } \sum_x p_X(x) = 1 \tag{Probability Mass Function (pmf)}$$
$$F_X(a) = P[X \leq a] = \sum_{x\leq a} p_X(x) \tag{Cumulative Distr. Function (cdf)}$$
$$\bar{F}_X(a) = P[X > a] = 1 - F_X(a) \tag{Tail}$$
For two drvs $X,Y$, we define the **joint probability mass function** $p_{X,Y}(x,y)$ as follows:
$$p_{X,Y}(x,y) = P[X = x \cap Y = y]$$
## **Convex Distances and Related Inequalities** [source](https://jpmastrogiacomo.ca/files/Talagrand_convex_hull_concentration_inequality.pdf)
- we are considering the probabilities of random variables landing in subsets of the sample space, where the sample space is itself a product of sample spaces
    - we want to understand the probability an RV in $\Omega$ is in the subset $A$
### Convex Distance
#### **First Definition**
Let $x,y \in \Omega$ be length-$n$ vectors. The **Hamming Distance** of $x,y$ is defined as: $$d(x,y) = \sum_{i=1}^n \mathbf{1}_{x_i \neq x_j} \tag{the number of elements that differ}$$
We generalize to the **$\alpha$-weighted Hamming Distance**, for any $\alpha \in \mathbb{R}^n$:
$$ d_\alpha(x,y) = \sum_{i=1}^n \alpha_i \mathbf{1}_{x_i \neq x_j}$$
For a set $A \in \Omega$, we define the distance as the shortest distance to the set:
$$ d_\alpha(A,x) = \inf_{y \in A} d_\alpha(x,y)$$
Finally, the **convex distance** is defined by maximizing over unit 2-norm $\alpha$:
$$\rho(A,x) = \sup_{|\alpha|_2 = 1} d_\alpha(A,x)$$
We also define, for any $t \geq 0$, the set $A_t$ as follows:
$$ A_t = \{x \in \Omega : \rho(A,x) \leq t \} $$
#### **Second Equivalent Definition** (see **Convex Hull** and **Hypercube** as needed)
add

### **Convex Hull**
add

### **Hypercube**
add

### **Talagrand's Inequality**
- henceforth, we'll write $P[A]$ for $P[X \in A]$ for an RV $X \in \Omega$
#### **Theorem 1** 
$$ \int_\Omega \text{exp}\lbrack\frac{1}{4}\rho^2(A,x)\rbrack \leq \frac{1}{P[A]} $$
#### *Proof* The proof is quite complicated. I'll give an overview of the important parts.
* Induction on dimension $n$. In the base case, $\rho(A,x) = \mathbf{1}_{x\notin A}$, and the math works out to be $P[A] + P[A^c]e^{\frac{1}{4}} \leq \frac{1}{P[A]}$

#### **Theorem 2 (Talagrand's Inequality)**
$$ P[A]P[A_t^c]\leq e^{-t^2/4}
