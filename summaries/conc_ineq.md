## Useful Facts on Probability Spaces, Random Variables, and Concentration Inequalities
____
# **Table of Contents**
## Section 1: Probability Spaces, Common Distributions
## Section 2: Convex Distances, Norms, and Related Inequalities
### 2.1
- Hamming variants
- Convex Hull/Hypercube
- Talagrand's Inequality
- Azuma
### 2.2
- Norms, incl. Frobenius
- Hölder's Inequality
- Cauchy-Schwarz Inequality, applications
- Bernstein's Inequality
## Section 3: Linear Algebra, Sketching, Hashing
### 3.1
- SVD, norms, pseudoinverse, applications, complexity
- Countsketch, applications
- Hadamard
- Matrix Chernoff, applications
### 3.2 
- balls in bins etc.
### 3.3
- perfect/universal/fun hashing

## Section 4: Probability Concentration Inequalities
- Markov's
- Chebyshev's
- Random Graphs stuff
- Hoeffding's
- Chernoff's, applications
- CLT, WLLN, SLLN
- Pretty Chernoff Bounds
- Tail bounds, e.g. Poissons, Pareto, applications

## **Probability Spaces**
A **probability space** is a triple $(\Omega,\Sigma,P)$, sample space, event space, probability measure, where $\Omega$ is a set, $\Sigma$ is a $\sigma$-field of subsets of $\Omega$, and $P$ is a non-negative measure on $\Sigma$ with $P(\Omega) = 1$ [Bollobás 1]
- in the simplest case, $\Omega$ is finite and $\Sigma$ is $\mathcal{P}(\Omega)$
- as a throwback to my third semester of college, a $\sigma$-field over a collection of subsets of $X$ must contain $\emptyset$, be closed under complements, and be closed under countable union [source](https://www.math.lsu.edu/~sengupta/7312s02/sigmaalg.pdf)
    - we also have closure in the sigma algebra under complement and intersection by rewriting terms with complements and unions
    - the extreme cases being $\{\emptyset,X\}$ and $\mathcal{P}(X)$
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
