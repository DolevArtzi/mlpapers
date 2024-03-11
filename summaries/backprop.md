### Learning Representation by Back-Propogating Errors
##### add citation
____
- distinguished from earlier methods by the ability to create useful new features while learning
- learning procedure decides when to activate hidden units to achieve desired input/output behavior, and this amounts to assigning them representations
### Forward Pass
- two key equations:
    - let $j$ be a unit of the NN, $x_j$ the total input to $j$, outputs $y_i$ that are connected to $j$ and the corresponding weights $w_{ji}$
    - can change these up if you want, only req. is bounded derivatives
    - note: linearly combining inputs to unit before non-linearity simplifies learning (worth investigating why?)
$$ x_j = \sum\limits_i y_i w_{ji} \tag{1, input for a given unit}$$
$$ y_j = \frac{1}{1+e^{-x_j}} \tag{2, output for a given unit}$$
- can add biases wherever you want
![fig 1](images/backprop_img1.png)
    - explain this after coming to understand it
- we aim to minimize the total distance of a finite set of desired outputs to our set of calculated outputs, given a set of input vectors
- we define error as follows:
    - $c$ is an index over input-output pairs, $j$ over output units, $y$ is the actual state of an output unit and $d$ the desired state
$$ E = \frac{1}{2} \sum_c \sum_j (y_{j,c} - d_{j,c})^2$$
### Backward Pass
- to minimize error with gradient descent, we propogate its derivatives down the layers of our network, need partial derivatives
- For a fixed $c$, dropping $c$ for brevity, we have 
$$ \frac{\partial E}{\partial y_j} = y_j - d_j \tag{3}$$
which chain rules to 
$$ \frac{\partial E}{\partial x_j} = \frac{\partial E}{\partial y_j} \frac{d y_j}{d x_j} \tag{4}$$ 
- Recalling eq. 2, we have $\frac{d y_j}{d x_j} = - \frac{-x_j e^{-x_j}}{(1+e^{-x_j})^2}$