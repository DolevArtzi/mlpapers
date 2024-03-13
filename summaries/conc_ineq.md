## `Dolev's Random Walk Through Fun and Useful Math` 
#### `with applications in probability, algorithms, performance modeling/Markov chains and queueing theory, reinforcement learning, theoretical computer science, linear algebra, and machine learning`
### *github.com/DolevArtzi*
### *Spring 2024*
<style>
.anchor-link {
    display: none;
}
</style>
____
## **Table of Contents**
- Section 0: Brief Introduction Probability Spaces, Sets, and Measure
    - [0.1 basics of probability](#section-01-basics-of-probability)
        - 0.1.1 defns
        - 0.1.2 $\sigma$-algebra
        - [0.1.3 axioms of probability, definitions](#axioms-of-probability-013-hb-30)
        - [0.1.4. union, intersection, conditioning and basic independence](#probability-facts-014-hb-31-40)
        - [0.1.5. basic laws of probability](#basic-laws-of-probability-015)
    - [0.2 set theory basics](#section-02-sets)
        - 0.2.1 useful identities
        - [0.2.2 functions](#strong-functions)
            - injective
            - surjective
        - [0.2.3 counting](#-counting-cardinality-and-countability---strong-counting-cardinality-and-countability-strong)
            - countability
            - [Cantor's diagonalization argument](#ins-theorem-cantors-diagonalization-argument)
            - [examples](#lemma-mathbbr-is-uncountable)
            - applications
- Section 1: Introduction to Discrete and Continuous Random Variables
    - [1.1 discrete random variables, expectation, higher moments](#section-11-discrete-random-variables-expectation-higher-moments)
        - 1.1.1 discrete random variables and combining rvs
            - pdf, cdf, tail
            - [combining RVs](#combining-random-variables)
        - [1.1.2 expectation](#ins-expectation-of-random-variables-112)
            - [expectation of a product](#ins-expectation-of-a-product)
            - [conditional expectation](#ins-conditional-expectation)
            - [countable partitions/law of total expectation](#ins-countable-partitionslaw-of-total-expectation)
        - [1.1.3 variance, and 2nd moment measures](#ins-variance-and-higher-moments)
            - [std/squared coefficient of variation](#ins-other-second-moment-related-metrics)
            - [covariance](#ins-covariance)
    - [1.2 continuous random variables](#ins-continuous-random-variables)
        - 1.3.1 CRVs: basics and moments
        - [1.2.2 Law of Total Probability for CRVs](#ins-law-of-total-probability-for-continuous-crvs)
        - [1.2.3 conditioning on CRVs](#add_link)
    - 1.3 applications
- Section 2: Probability Distributions
    - [2.1 discrete distributions](#section-21-discrete-distributions)
        - [2.1.1 bernoulli](#ins-bernoulli-distribution)
        - [2.1.2 binomial, incl. like poisson](#ins-binomial-distribution)
        - [2.1.3 geometric](#ins-geometric-distribution)
        - [2.1.4 poisson](#ins-poisson-distribution)
    - [2.2 continuous distributions]
        - [2.2.1 uniform](#ins-uniform-distribution)
        - [2.2.2 exponential, incl. applications](#ins-exponential-distribution)
        - [2.2.3 normal, incl. approx](#ins-normal-distribution)
        - [2.2.4 erlangs, application to queueing](#ins-erlang-distribution)
        - [2.2.5 cauchy, applications, additivity]
    - [2.3 generating RVs]
        - [2.3.1 inverse transform]
        - [2.3.2 box-mueller]
        - [2.3.3 python for binomial with window]
        - [2.3.4 applications, comments on complexity/implementation]
    - [2.4 applications]

-  Section 2: Probability Concentration Inequalities
    - Markov's
    - Chebyshev's
    - Hoeffding's
    - Chernoff's, applications
    - CLT, WLLN, SLLN
    - Pretty Chernoff Bounds
    - misc. useful one: sterlings, normal approximations, confidence intervals
>  Tail bounds, e.g. Poissons, Pareto, applications
    - Random Graphs stuff
- Section 3: Markov Chains
- Section 4: Queueing Basics
> ## Section 5: Convex Distances, Norms, and Related Inequalities
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

> ## Section 6: Linear Algebra, Sketching, Hashing
> ### 3.1
>  SVD, norms, pseudoinverse, applications, complexity\
>  Countsketch, applications\
>  Hadamard\
>  Matrix Chernoff, applications
> ### 3.2 
>  balls in bins etc. 
> ### 3.3
>  perfect/universal/fun hashing
___
### Sources
\- Mor's book(s) Pnc, Queueing
\- Bollobas RG
\- [sigma_algbera](https://www.math.lsu.edu/~sengupta/7312s02/sigmaalg.pdf)
\- [convex distances, talagrand,holders](https://jpmastrogiacomo.ca/files/Talagrand_convex_hull_concentration_inequality.pdf)
___



## Section 0.1: Basics of Probability
> A **probability space** $[0.1.1]$ is a triple $(\Omega,\Sigma,P)$, sample space, event space, probability measure, where $\Omega$ is a set, $\Sigma$ is a $\sigma$-field of subsets of $\Omega$, and $P$ is a non-negative measure on $\Sigma$ with $P(\Omega) = 1$ [Bollobás 1]
> \- in the simplest case, $\Omega$ is finite and $\Sigma$ is $\mathcal{P}(\Omega)$
>  A $\sigma$-field $[0.1.2]$ over a collection of subsets of $X$ must contain $\emptyset,$ be closed under complements, and be closed under countable union [source](https://www.math.lsu.edu/~sengupta/7312s02/sigmaalg.pdf)
> \- we also have closure in the sigma algebra under complement and intersection by rewriting terms with complements and unions
    - the extreme cases being $\{\emptyset,X\}$ and $\mathcal{P}(X)$
> #### **Axioms of Probability** $[0.1.3]$ [HB 30] 
> 1. *Non-negativity*: $P[E] \geq 0$ for any event $E$
> 2. *Additivity*: If $E_1, E_2, ...$ is a countable sequence of pairwise disjoint events, then 
> $$P[E_1 \cup E_2 \cup ...] = \sum_i P[E_i]$$
> 3. *Normalization*: $P[\Omega] = 1$
> #### **Probability Facts** $[0.1.4]$ [HB 31-40]
> ##### <ins>**Union**
> $$P[A \cup B] = P[A] + P[B] - P[A\cap B] \tag{defn, \textbf{HB Lemma 2.5}}$$
>$$P[A \cup B] \leq P[A] + P[B] \tag{\textbf{Union Bound (UB)}, \textbf{Lemma 2.5}, Ax. 1}$$
> ##### <ins>**Conditional Probability, Independence**
> $$P[E \mid F] = \frac{P[E\cap F]}{P[F]} \tag{defn \textbf{Conditional Indep.}, assuming $P[F] > 0$}$$
> If $P[F] \neq 0$, events $E,F$ are **independent** if $P[E \mid F] = P[E]$, that is, we gain no information on $E$ by knowing the outcome of $F$. This is equivalent to $P[E\cap F] = P[E]P[F]$ by simple substitutions. 
> $$p_{X,Y}(x,y) = p_X(x) \cdot p_Y(y) \tag{defn. \textbf{Independence}, write $X\perp Y$} $$
> Furthermore, events $A_1, ..., A_n$ are independent if for any subset $S \in [n]$, ($[n] = \{1,...,n\})$, $$P\lbrack \bigcap_{i \in S} A_i \rbrack = \prod_{i \in S} P[A_i] \tag{defn. of independence for finite set}$$
> We also define the less strict, but very often sufficient, **pairwise independence** for $A_1, ..., A_n$ as follows: $$\forall i,j \text{   } P[A_i \cap A_j] = P[A_i]P[A_j] \tag{defn. \textbf{Pairwise Independence}}$$
Events **$A,B$ are independent *given* $C$** if $$P[A \cap B \mid C] = P[A \mid C]P[B \mid C]$$
\- note that independence and conditional independence to not imply each other either way

> #### **Basic Laws of Probability** $[0.1.5]$
> ##### <ins> **Law of Total Probability**
> Let $\{F_i\}_{i\in[n]}$ be a partition of $\Omega$. Then $$P[E] = \sum_{i = 1}^n P[E \cap F_i] = \sum_{i=1}^n P[E \mid F_i]P[F_i]$$
> \- *Proof:* Write $E$ as union over intersections with $F_i$, use mutal exclusivity. This law also holds if $\{F_i\}_i$ partitions $E$, and is extendable from $n$ to countably infinite
> ##### <ins> **Law of Total Probability for Conditional Probability** 
> $$P[A \mid B] = \sum_{i=1}^n P[A\mid B \cap F_i] \cdot P[F_i \mid B]$$
> #### <ins> **Laws of Total Probability for Discrete RVs** </ins>   [**HB 59 Thm. 3.7**]
> For an event $E$, we can write its probability by conditioning on RV $Y$ as follows:
> $$P[E] = \sum_y P[E \cap Y = y] = \sum_y P[E \mid Y = y] f_Y(y)$$
> For rvs $X,Y$, we can express $P[X = k]$ by conditioning on $Y$
> $$P[X = k] = \sum_y P[X = k \cap Y = y] = \sum_y P[X = k \mid Y = y] \cdot f_Y(y)$$
> ##### <ins> **Bayes Law**
> Assuming $P[E] > 0$, $$P[F\mid E] = \frac{P[E\mid F]P[F]}{P[E]}$$
> \- useful in basic problems or teasers

## Section 0.2: Sets
> #### <ins> **Useful Set Identities**
> $$ \left(\bigcap_{i\in\mathcal{I}} A_i\right)  \cup B = \bigcap_{i\in\mathcal{I}}(A_i \cup B) \tag{union o int. $=$ int o union}$$
> $$ \left(\bigcup_{i\in\mathcal{I}} A_i\right)  \cap B = \bigcup_{i\in\mathcal{I}}(A_i \cap B) \tag{int o union $=$ union o int.}$$
> $$ (\bigcap\limits_{i\in\mathcal{I}} A_i)^c = \bigcup_{i\in\mathcal{I}}(A_i^c) \tag{\textbf{First DeMorgan's Law}, comp. of int. is un. of comp}$$
> $$ (\bigcup\limits_{i\in\mathcal{I}} A_i)^c = \bigcap_{i\in\mathcal{I}}(A_i^c) \tag{\textbf{Second DeMorgan's Law}, comp. of un. is int. of comp}$$
> #### <strong> **Functions**
> a function $f$ between sets $A$ and $B$ is a subset of the *Cartesian product* of $A,B$: $f: A \rightarrow B \subseteq A \times B$. We call $A$ the domain, and $B$ the codomain. For each $a \in A$ that $f$ maps to a $b \in B$, we say $a$ is the *argument* and $b$ its *image*. 
> #### **Injective Functions**
> An **injective, or one-to-one** (1:1) function is one where unique arguments have unique images. In math, this means
> $$f \textbf{ injective } \text{ means  } \forall \text{  }a_1 \neq a_2, f(a_1) \neq f(a_2)$$
> #### **Surjective Functions**
> A **surjective, or onto** function is one where each element in the codomain is mapped to by at least one argument in $f$. In math,
> $$f \textbf{  surjective  } \text{ means  }\forall b\text{  } \in \text{codomain}(f), \text{  } \exists a \in \text{domain}(f) \text{ s.t. } f(a) = b$$
> A function which is both 1:1 and onto is called a **bijection** (write $f: A \leftrightarrow B$), which means an inverse, $f^{-1}$ is well-defined since the mapping is unique and the entire codomain is covered [[source](https://www.ee.iitm.ac.in/~krishnaj/EE5110_files/notes/lecture1_set_theory.pdf)].

 ####  <strong> **Counting: Cardinality and Countability** </strong> <span class="anchor-link"> {#strong-counting-cardinality-and-countability-strong}</span>
> ##### [[source](https://www.ee.iitm.ac.in/~krishnaj/EE5110_files/notes/lecture3_cardinality.pdf)]
> \- Two sets are **equicardinal** (have the same cardinality) if there exists a bijection between them.
> $$\exists f:A \hookrightarrow B \text{ injective }  \implies |B| \geq |A|$$
> $$\exists f:A \twoheadrightarrow B \text{ injective but not bijective }  \implies |B| > |A|$$
> A set is **countably infinite** if it is equicardinal with $\mathbb{N}$. A **countable** set is finite or countably infinite.
> #### **Lemma: Countable Union of Countable Sets is Countable**
> *Proof omitted*
> TODO: add example: set of all algebraic number is countable (251)
> #### <ins> **Theorem: Cantor's Diagonalization Argument**
> TODO: add proof
> #### **Lemma:** $\mathbb{R}$ **is Uncountable**
> TODO: add proof
> Power set uncountable, cartesian product of countable is countable, show transcendental numbers countable/uncountable

## Section 1.1: Discrete Random Variables, Expectation, Higher Moments
> \- $[1.1.1]$ a **random variable** (rv) is a real-valued function of the outcome of an experiment involving randomness (HB 50)\
> \- **discrete random variables** (drvs) take on a value from a discrete set, while **continuous random variables** (crvs) take on a value from a continuous set\
> \- For a discrete random variable $X$, we have the following:
> $$p_X(a) = P[X = a] \text{ where } \sum_x p_X(x) = 1 \tag{Probability Mass Function (pmf)}$$
> $$F_X(a) = P[X \leq a] = \sum_{x\leq a} p_X(x) \tag{Cumulative Distr. Function (cdf)}$$
> $$\bar{F}_X(a) = P[X > a] = 1 - F_X(a) \tag{Tail}$$

> #### **Combining Random Variables**
> For two drvs $X,Y$, we define the **joint probability mass function** $p_{X,Y}(x,y)$ as follows:
> $$p_{X,Y}(x,y) = P[X = x \cap Y = y]$$
> \- By the [*Law of Total Probability*](#ins-law-of-total-probability), we have $$p_X(x) = \sum_y p_{X,Y}(x,y) \tag{Marginalization}$$

> #### <ins> **Expectation of Random Variables** $[1.1.2]$
> The **expectation** or the mean of the distribution from which $X$ is drawn of an rv $X$ defined over domain $D$, is: 
> $$E[X] = \sum_{x\in D} x\cdot p_X(x) \tag{defn. \textbf{Expectation for drv}}$$
> #### <ins> **Linearity of Expectation (Fundamental)**
> For any rvs $X$ and $Y$, 
> $$E[X+Y] = E[X] + E[Y]$$
> *Proof*: Write the summations out, split into two double sums, then pull the variable out from the first sum **[HB 69]**.
> #### <ins> **Alternative Definition of Expectation**
> Let the domain of $X$, $D(x),$ be $\mathbb{N}$. Then:
> $$E[X] = \sum_{x=0}^{\infty} P[X>x] \tag{Summing the Tail}$$
> #### <ins> **Expectation of a Product**
> Let $X,Y$ be rvs. The **expectation of $XY$** is defined as follows:
> $$E[XY] = \sum_x\sum_y xy \text{ } p_{X,Y}(x,y)$$
> We also have that $X \perp Y \implies$ $E[XY] = E[X]E[Y]$ (**independence via expectation**)
> #### <ins> **Expectation of a Function/Law of the Unconscious Statistician**
> $$E[g(X)] = \sum_x g(x) \cdot p_X(x) \tag{\textbf{Expectation of a Function}}$$

> #### <ins> **Conditional Expectation** 
> Let $X$ be a drv and let $\Omega$ be countable, including event $A : P[A] > 0$. Then $p_{X\mid A}$ is the **conditional pmf** of $X$ given $A$:
> $$p_{X\mid A}(x) = P[X = x | A] = \frac{P[X = x \cap A]}{P[A]} $$
> This allows us to define the **conditional expectation of $X$ given $A$**:
> $$E[X\mid A] = \sum_{x \in A} x p_{X\mid A}(x)$$

> #### <ins> **Countable Partitions/Law of Total Expectation**
> First, we'll give the important special case **[HB 79 Thm. 4.22]**: if $F_1, F_2, ...$ is a countable partition of $\Omega$, then:
> $$E[X] = \sum_{i=1}^{\infty} E[X \mid F_i] P[F_i] \tag{$(*)$ \textbf{Expectation via Conditioning}}$$
> Given a discrete rv $Y$, if we treat $Y = y$ as an event, then we can write:
> $$E[X] = \sum_y E[X \mid Y = y]\cdot P[Y = y] $$
> *Proof*: Write out the sum, flip order of summation, pull $y$ terms through, apply [defn. of conditional expectation](#ins-conditional-expectation)\
> Now, we'll give the **Law of Total Expectation**:\
> Let $X,Y$ be rvs on the same [probability space](#a-namebasicsa-section-11-basics), where $X$ takes values in $\mathcal{X}$, and $Y$ in $\mathcal{Y}$. Then:
> $$E[E[X\mid Y]] = E[X] \tag{\textbf{Law of Total Expectation}}$$
> [*Proof*](https://statproofbook.github.io/P/mean-tot.html): 
> $$E[E[X\mid Y]] = E \big\lbrack \sum_{x\in \mathcal{X}} x \cdot P[X = x \mid Y]\big\rbrack \tag{1}$$
> $$= \sum_{y \in \mathcal{Y}} [\sum_{x\in \mathcal{X}} x P\big[X = x \mid Y = y]\big]P[Y = y] \tag{2}$$
> $$\sum_{x\in\mathcal{X}}\sum_{y\in\mathcal{Y}} x P[X = x \mid Y = y] P[Y = y] \tag{3}$$
> So far, $(1),(2)$ were by definition, and $(3)$ just switched the order of summation, which you can usually do with finite sums, but I'll point the interested reader to [Fubini's in Discrete Math](https://web.math.ucsb.edu/~cmart07/fubini.pdf). Continuing, we apply the [defn. of conditional probability](#probability-facts-114-hb-31-40) and pull $x$ out from the inner sum to obtain:
> $$E[E[X\mid Y]] = \sum_{x\in\mathcal{X}} x \sum_{y\in \mathcal{Y}} P[X = x \cap Y = y] \tag{4}$$
> We marginalize and notice that this is just $E[X]$:
> $$\sum_{x \in \mathcal{X}} x P[X = x] \tag{5, qed.} = E[X]$$
> Finally, we'll generalize [$(*)$](#ins-countable-partitionslaw-of-total-expectation):
> $$E[g(X)] = \sum_y E[g(X) \mid Y = y]P[Y = y] \tag{\textbf{Expectation of Function via Conditioning}}$$

> #### <ins> **Variance and Higher Moments**
> The $k^{th}$ moment of the rv $X$ is $E[X^k]$. Usually, we only care about the first ($E[X]$) and second moments, and we use the second moment in the form of **variance**, which measures the unnormalized deviation of $X$ from its mean. There are several definitions of variance, all of which are useful in different contexts. 
> $$\text{Var}(x) = E[(X - E[X])^2] \tag{first defn. of variance}$$
> $$\text{Var}(x) = E[X^2] - E[X]^2 \tag{second defn. of variance}$$
> *Proof*: [Linearity of Expectation](#ins-linearity-of-expectation-fundamental)
> #### <ins> **Theorem: Linearity of Variance**
> Let $X$ and $Y$ be rvs such that $X\perp Y$. Then
> $$\text{Var}(X+Y) = \text{Var}(X) + \text{Var}(Y)$$
> This naturally generalizes from 2 to $n$.\
> *Proof*: Write out the definitions, use [independence via expectation](#ins-expectation-of-a-product) \
> \- TODO: add sums vs. copies consideration

> #### <ins> **Other Second-Moment Related Metrics**
> \- the **standard deviation** of $X$ is defined as 
> $$\sigma_X = \text{std}(X) = \sqrt{\text{Var}(X)}$$
> \- the **squared coefficient of variation** of $X$ is defined as
> $$C^2_X = \frac{\text{Var}(X)}{E[X^2]}$$
> This is often useful, as it provides a normalized measure of $X$'s deviation from its mean.
> #### <ins> **Covariance**
> The **covariance** of rvs $X,Y$ is defined in two ways as follows:
> $$\text{Cov}(X,Y) = E[(X - E[X])(Y - E[Y])] \tag{first defn. of covariance}$$
> $$\text{Cov}(X,Y) = E[XY] - E[X]E[Y] \tag{second defn. of covariance}$$
> *Proof*: write it out, use independence\
> \- note that the sign of covariance indicates the direction of correlation between $X$ and $Y$


> #### <ins> **Continuous Random Variables**
> \- For a continuous random variable $X$, we have an analagous definition to the pmf, the **probability density function** (pdf):
> $$P[a \leq X \leq b] = \int_a^b f_X(x)dx$$
> \- naturally, we have normalization (we do in the discrete case as well, just replace $\int$ with $\sum$). For a crv $X$ with domain $D$, say $D = (-\infty,\infty)$, we have
> $$\int_D f_X(x)dx = 1$$
> \- the cdf and tails for crvs are analagous to the discrete case
> #### <ins> **Moments of a CRV**
> The **expected value of a crv** is $$E[X] = \int_{D} x \cdot f_X(x)dx$$
> The **$k^{th}$ moment** is $$E[X^k] = \int_{D} x^k \cdot f_X(x)dx$$ 
> And similarly, $$E[g(X)] = \int_{D} g(x)\cdot f_X(x)dx$$
> Finally, we have the **variance of a crv** $$\text{Var}(X) = \int_D (x-E[X])^2 f_X(x)dx$$

> #### <ins> **Law of Total Probability for Continuous CRVs**
> $$P[A] = \int_{D} P[A \mid X = x] f_X(x) dx \tag{LOTP for CRV, pt. 1}$$
> - **TODO**: add HB ch. 8 and ch. 7 from pg. 148 on





## Section 2.1: Discrete Distributions
> #### <ins> **Bernoulli distribution**
> The Bernoulli distribution represents the outcome of a coin flip with probability $p$ of landing heads. We write $X \sim\text{Bernoulli}(p)$ ($\sim$ means "is distributed as").
> The **pmf of Bernoulli** is as follows:
> $$P[X = 1] = p, P[X = 0] = 1 - p$$
> The **variance of Bernoulli** is 
> $$\text{Var}(\text{Bernoulli}(p)) = p(1-p) \tag{mean is $p$, second moment is still $p$}$$

> #### <ins> **Binomial distribution**
> The **binomial distribution** is parametrized by $n$ and $p$, and it represents the sum of $n$ independent $p$-coin flips, the sum of $n$ independent $\text{Bernoulli}(p)$'s.
> $$P[\text{Binomial}(n,p) = k] = {n\choose k}p^k (1-p)^{n-k} \tag{$k \in [0,n]$}$$
> This is the number of ways to arrange your $k$ successes, times the probability of having exactly $k$ successes in $n$ indep. trials. The **expected value of binomial** is $E[X] = np$ by linearity. Likewise, the **variance of binomial** is easily computed by [linearity of variance for independent rvs](#ins-theorem-linearity-of-variance) to be $n\cdot \text{Var}(\text{Bernoulli}(p)) = np(1-p)$. There isn't an easy way to express the cdf, but we'll see how to compute it in a straightforward way soon

> #### <ins> **Geometric distribution**
> The **geometric distribution** represents the number of trials until success with independent probability $p$ of success per round. Thus, note that it's defined on $[1,\infty)$.
> $$P[\text{Geom(p)} = k] = (1-p)^{k-1}\cdot p$$
> The **cdf of geometric** is defined as expected: $$P[\text{Geom}(p) > k] = (1-p)^k \tag{$k$ failures is all we know}$$
> The calculations for the first and second moments of a geometric are instructive for two very different methods we often use. First, we'll just list the results.
> $$E[\text{Geom}(p)] = \frac{1}{1-p} \tag{\textbf{mean of geometric}}$$
> $$E[\text{Var}(\text{Geom}(p))] = \frac{1-p}{p^2} \tag{\textbf{variance of geometric}}$$

 #### <ins> *Proof* of **mean of geometric**
*Note: This is not difficult, but a good exercise to gain more comfort with solving infinites series.*

 <details><summary>Reveal Hint 1 (New Technique 1)</summary>
 
 How can we do a simple transformation on the summation to obtain it from a known sum? Think calculus. 

 </details>

<details><summary>Reveal Proof</summary>

 $$E[\text{Geom}(p)] = \sum_{k=1}^{\infty} q^{k-1} p \tag{defn., $q := 1-p$}$$
 We'll employ a trick that is good to know: *viewing the sum as a derivative of a known sum*.
 $$\sum_{k=1}^{\infty} q^{k-1}p = p\frac{d}{dq}[\sum_{k=0}^{\infty} q^k]$$
 $$ = p\frac{d}{dq}[\frac{1}{1-q}] = \frac{p}{(1- q)^2} = \frac{1}{p}$$
</details>

 ####  <ins> *Proof* of **variance of geometric**
 *Note: This is a great exercise to try. Think about it, and look at the hints first if you're stuck.*
 <details><summary>Reveal Hint 1</summary>
 
 We're going to use conditioning to simplify the expression for the second moment.
 </details>

 <details><summary>Reveal Hint 2 (New Technique 2)</summary>
 
 Condition on one or more of the possible coin flips. Think about how this changes the distribution (it only changes it a little).
 </details>

  <details><summary>Reveal Hint 3</summary>

[memorylessness of geometrics](#ins-lemma-memorylessness-of-geometrics)
 
 </details>

 <details><summary>Reveal Proof</summary>

 To compute the variance of $X$, where $X \sim \text{Geom}(p)$, we'll start by finding the second moment. To do this, we'll employ our second new trick, a similar flavor of which is useful for many different distributions: *conditioning on the first flip* (in general, conditioning on the first event/occurence). Let $A$ be the event that the first coin flip is heads.
 $$E[X^2] = E[X^2 \mid A]P[A] + E[X^2 \mid A^c]P[A^c] \tag{expectation via conditioning}$$
 See [1.2.2 expectation via conditioning](#ins-countable-partitionslaw-of-total-expectation) as needed. Continuing,
 $$ = 1\cdot P[A] + E[(1+X)^2]P[A^c] \tag{\textbf{Geo. Mem. Lemma} $X \mid A^c \sim 1 + \text{Geom}(p)$}$$
 $$ = p + E[1 + 2X + X^2](1-p)$$
 $$ = p + (1 + \frac{2}{p} + E[X^2])\cdot q \tag{linearity, first moment of geom.}$$
 Taking stock of what we have and further simplifying, we reach
 $$ E[X^2] = \frac{2-p}{p} + qE[X^2] \implies E[X^2] = \frac{2-p}{p^2}$$
 Which means the **variance of geometric** is 
 $$\frac{2-p}{p^2} - \frac{1}{p^2} = \frac{1-p}{p^2}$$
</details>


> #### <ins> **Lemma: Memorylessness of Geometrics**
> Let $X \sim \text{Geom}(p)$ and $Y = X \mid T]$, where $T$ is the event that the first coin toss is tails. Then $$Y \stackrel{d}{=} X + 1 \tag{\textbf{Memorylessness of Geom.}}$$
> This means $X+1$ and $Y$ have the same distribution.\
> *Proof*: $X + 1 \in [2,\infty)$, and $Y \in [2,\infty)$ as well.
> $$\text{For } i \geq 2, P[Y = i] = P[\text{takes $i-1$ attempts from here]}$$
> $$ = (1-p)^{i-2} p \tag{indep. trails, defn.}$$
> $$\text{For } i \geq 2, P[X + 1 = i] = P[X = i - 1]$$
> $$ = (1-p)^{i-2}p \tag{defn. pmf of geometric}$$
> So, we have that $P[Y = a] = P[X = a] \text{ }\forall a \in D(X+1) = D(Y)$
> To see that this is sufficient to conclude $X+1 \stackrel{d}{=} Y$, note that pmf/pdf/cdf all uniquely determine the distribution.
 This is a **Lemma: pdf uniquely determines distribution** we may use again. The cdf uniquely defines a distribution, and it is (usually) differentiable to obtain the pdf (or pmf). With normalization, that handles constants to obtain uniqueness. Later, we'll see things like Laplace transforms also uniquely define distributions. Stay tuned. 




> #### <ins> **Poisson distribution**
> explanation\
> pmf\
> optional cdf/tail\
> mean\
> variance\
> comments
## Section 2.2: Continuous Distributions
> #### <ins> **Uniform distribution**
> explanation\
> pmf\
> optional cdf/tail\
> mean\
> variance\
> comments

> #### <ins> **Exponential distribution**
> explanation\
> pmf\
> optional cdf/tail\
> mean\
> variance\
> comments

> #### <ins> **Normal distribution**
> explanation\
> pmf\
> optional cdf/tail\
> mean\
> variance\
> comments

> #### <ins> **Erlang distribution**
> explanation\
> pmf\
> optional cdf/tail\
> mean\
> variance\
> comments

> #### <ins> **Hyperexponential distribution**
> explanation\
> pmf\
> optional cdf/tail\
> mean\
> variance\
> comments


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
$$ P[A]P[A_t^c]\leq e^{-t^2/4}$$
