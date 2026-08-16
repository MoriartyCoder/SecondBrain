
# Problems and reminder

* Weight inits: Distributions/ Variance and why it is important

# Points

## Backpropagation

## Weight Initialisation

### Notes
Why not make all weights 0 at the beginning? Because then then the weights for all neurons in one layer/row of the weight matrix would always be the same and the network would never beginn to learn different properties and thus differentiate. 

Research has shown that a twice continuously differentiable function with random initialisation and sufficiently small constant step size guarantees that that we find a local minima or negative infinity.

When the initial values are too big or small then 

#### Xavier initialisation

The initialisation of the weight matrix is done as follows:
• Random initialisation using standard normal distribution (mean 0 and standard deviation 1).
• These random values are multiplied by the following scalar

$$\sqrt{\frac{1}{n_{l-1}}}$$
OR
$W^[l] = np.random.randn(size\_l, size\_l) * np.sqrt(1 / size\_l - 1)$
* size_l: size of the layer

*This is for one layer! The scalars depend on the size of each layer.*

Xavier initialization is used by default with tanh (or sigmoid activation function) because:
* Keep the activations centered around 0 and so that the gradients do not become too small
* The vanishing gradient problem is not so acute

Xavier initialization is well suited for symmetric activation functions.

#### Kaiming He

$$\sqrt{\frac{2}{n_{l-1}}}$$
OR
$W^[l] = np.random.randn(size\_l, size\_l) * np.sqrt(2 / size\_l - 1)$

### Questions
#### Why is weight initialisation important?
#### Can't you also start with constant parameters?
#### Why take random initial parameters?

#### Why does the bias have not effect on the backpropagation?

#### Why not use Gaussian distribution for weight initialisation?
?? The variance would get bigger because the values would get bigger and bigger with each layer. The activation function (i. e. sigmoid) would make the resulting values very small. This is called the vanishing gradient problem. 

#### What would be ideal for our weights?
We want to choose the weight so that the 𝑧𝑧-values are not too large and not too small. They should fluctuate around 0 with a variance of 1.

#### When should you choose which weight initialization?
Xavier for symmetric activation functions (i. e. Sigmoid and tanh)
Kaiming He-Initialization for ReLU Activation Function

## Optimizer

## CNN

## RNN

### LSTM

## Regularisation

## Over-/Underfitting

## Vanishing/Exploding Gradient

## Transfer Learning

## Batch Norm


# Possible Exam Questions

## 01
(1) What is the difference between a perceptron and a regression function??  
(2) Draw a neural network with 2 inputs, 2 layers with 4 nodes each and 1 numerical output node.  
Add all components!  
(3) What is a feedforward network and when do we call layers fully connected?

## 02
(1) What activation function would you use for a classification problem, which one for a regression
problem?
(2) Draw a neural network with 3 inputs, 1 hidden layer with 4 nodes and one output node that
should be able to learn a regression task. Include all elements and give all notations
(3) For the network of (2), write down the calculations of the forward path in matrix notation for the
second node of the hidden layer.

## 03
Technische Hochschule Ingolstadt | Algorithms for AI 2  
(1) Would a simple regression model be sufficient to learn the AND and the OR problem? Explain  
your answer!  
(2) Calculate the forward path for a given neural network with the following input parameters ...!  
(3) Explain why a neural network can solve the XOR problem and how it's done

## 04
(1) Why are we using backpropagation for neural networks instead of deriving the cost function?
(2) For the following node, write down the chain rule for the gradients
(3) Given the following network, calculate the forward path for data x=... and y=..., calculate the
squared error loss and perform backpropagation to update the weights of the network.

## 06
(1) What would happen if we init all weights of a NN with 0?  
(2) Which weight initialization are you choosing for a Sigmoid activation function?  
(3) What two properties should the initialized weights have and why are they important?  
(4) Can the biases be initialised with zeros? Explain your answer.

## 07
(1) Explain in your own words what momentum with SGD does and how it relates to the physics
term of momentum
(2) Calculate the step updates for the given values using SGD and SGD with momentum
(3) Interpret the results from (2), what effect does momentum has on the result and how does this
benefit optimisation.

## 08
(1) What is the difference between a linear learning rate decay and reduction of learning rate over
time? Draw a graph of both to illustrate the difference.
(2) Calculate the step updates for the given values using SGD and SGD with learning rate decay
as given in the following formulas.
(3) Why is Adam preferred in most deep learning tasks and what drawbacks does it still have?

## 09
(1) Why are we using L1 and L2 regularization with neural networks?
(2) For the given neural network, add L2 regularization to the formulas, calculate the forward path
and the backpropagation for node x
(3) For the following given problem, would you use L1 or L2 regularization? Explain your choice

## 10
(1) How does Dropout reduce overfitting in neural network?
(2) What is early stopping?
(3) What is data augmentation and why are we using it?
(4) How do you apply dropout in pyTorch? Give an example.
(5) Why do we need to make a difference in training and evaluation when using dropout?
Explain how this problem is resolved.

## 11
(1) Why do we use CNNs for Image data und what is the basic idea behind it?
(2) Given the following input and parameters, calculate the output dimension of the convolution
(3) Do you think the current success of AI could also have happened without applying
convolutions to huge input data like images or audio signals?

## 12
(1) Why do we use Batch Norm in neural networks and how does it work
(2) Calculate the forward and backward path for the following layer with batch normalization
(3) Explain what the covariate shift is and how batch norm is helping to avoid it in neural networks
(4) You want to train a model to differentiate between specific kinds of bees. However, your
dataset is very small. How can you use existing datasets for image classification (e.g.,
ImageNet) to help with the training of your network.

## 13
(1) How is a RNN Layer different from FC or CNN Layers?
(2) Draw a LSTM Layer with all it's components and describe what each component is good for.
(3) What advantages does a RNN have compared to a 1D convolution? Explain by using an
example.

## 14
(1) Name one common reason for vanishing gradients. Name one for exploding gradients
(2) What can mathematically be done to avoid vanishing and exploding gradients?
(3) You have trained the following network for solving the following problem (some configuration
given). The network is suffering from severe underfitting. What might be the reason and what
are you going to do against it?

## 15
(1) What would happen if we choose an incorrect hyperparameter like a too small learning rate?
(2) Explain how Bayesian Optimization is used for hyperparameter optimization. What is it's
benefit?
(3) Let there be a neural network that can be configured with 5 different hyperparameters where
each can have 4 different values. Assume that training the network under each configuration
would take 2 hours. How long would it take to test each combination of hyperparameters?
What alternative way could be used to find a good set of hyperparameters?

## 16
(1) What is the major difference between supervised and unsupervised learning problems? Name
3 different examples of unsupervised learning methods.
(2) For the following examples, decide if it is a supervised or unsupervised learning problem
(3) How can unsupervised learning be successful at all, as we don't know the truth?

## 17
(1) What is the difference between hierarchical clustering and non-hierarchical clustering?
(2) You are given the following datapoints. Calculate the first 3 iterations of the k-means clustering
algorithm with k=3 and name the (x,y) coordinates of the final centroids
(3) Explain why DBSCAN is usually delivering better results than k-means clustering

## 18
(1) Why is the reward so important when we design a problem that should be solved by RL?
(2) What is the discounted return and why do we use it instead of the return?
(3) Imagine an autonomous car in a simulation. The simulation returns the current GPS coordinates of the car and knows
the boundaries of the road. Define a MDP that can be used to train a RL agent to steer the car along a given. There are
no other vehicles and the simulation can control the steering and acceleration of the car.

## 19
Write Pseudo-code for table based Q-Learning with an epsilon-greedy strategy.
![[PseudoQLearningCode.png]]

# Possible Exam Questions answered by ChatGPT
## **01**

### (1) Difference Between a Perceptron and a Regression Function

- A **perceptron** uses a step function for activation to classify inputs (binary classification).
- A **regression function** predicts continuous values using linear or nonlinear mappings but does not involve step-function activation.

### (2) Neural Network with 2 Inputs, 2 Layers (4 Nodes Each), 1 Output Node

- **Inputs**: $x_1, x_2$​
- **Hidden Layers**: 2 layers with 4 nodes each
- **Output Node**: 1 numerical output node
- Include weights $W^{(1)}, W^{(2)}, W^{(3)}$, biases $b^{(1)}, b^{(2)}, b^{(3)}$, and activation functions $\sigma$.

---

### (3) What is a Feedforward Network? What are Fully Connected Layers?

- A **feedforward network** passes information unidirectionally from input to output, without feedback loops.
- **Fully connected layers** have every node in one layer connected to every node in the next.

---

## **02**

### (1) Activation Functions for Classification and Regression

- **Classification**: Sigmoid, softmax, or ReLU
- **Regression**: Linear or ReLU

### (2) Neural Network for Regression Task

- **Inputs**: $x_1, x_2, x_3$​
- **Hidden Layer**: 4 nodes with weights $W^{(1)}, biases b^{(1)}$, and activation $\sigma$.
- **Output Layer**: Single output node for numerical prediction with weights $W^{(2)}$ and biases $b^{(2)}$.

### (3) Forward Path for the 2nd Node of the Hidden Layer

$$z_2^{(1)} = \sum_{i=1}^{3} W_{2i}^{(1)} x_i + b_2^{(1)}​$$

---

## **03**

### (1) Can a Simple Regression Model Learn AND/OR Problems?

- A simple regression model can learn the **OR problem** (linearly separable) but cannot learn the **AND problem** (not linearly separable) without a non-linear transformation.

### (2) Forward Path Calculation

Requires specific input weights, biases, and activations to compute the numerical results.

### (3) Why Can Neural Networks Solve XOR Problems?

- XOR is not linearly separable. Neural networks use hidden layers and non-linear activations to construct non-linear decision boundaries.

---

## **04**

### (1) Why Backpropagation Instead of Direct Cost Function Derivation?

- Backpropagation efficiently computes gradients for all weights and biases using the chain rule, avoiding the computational overhead of direct derivation for each parameter.

### (2) Chain Rule for Gradients

For a node z:

$$\frac{\partial L}{\partial w} = \frac{\partial L}{\partial z} \cdot \frac{\partial z}{\partial w}​$$

### (3) Forward Path, Loss, Backpropagation

Use given network details to compute the forward pass, squared error loss, and update gradients via backpropagation.

---

## **06**

### (1) What Happens if We Initialize All Weights with Zero?

- All neurons in a layer learn the same features, leading to symmetry and no differentiation in updates.

### (2) Weight Initialization for Sigmoid Activation

- Use **Xavier initialization** to maintain variance through layers.

### (3) Properties of Initialized Weights

1. **Randomness**: Breaks symmetry and enables diversity in learning.
2. **Proper Scale**: Prevents exploding or vanishing gradients.

### (4) Can Biases Be Initialized with Zeros?

- Yes, biases can be zero-initialized since they do not rely on inputs and do not affect symmetry.

---

## **07**

### (1) What Does Momentum in SGD Do?

- Momentum incorporates a fraction of the previous update to smooth updates and accelerate convergence, mimicking inertia in physics.

### (2) Step Updates with SGD and Momentum

Requires specific input values for computation.

### (3) Interpretation of Momentum

- Momentum reduces oscillations, speeds up convergence, and helps escape shallow minima.

---

## **08**

### (1) Linear Decay vs. Reduction Over Time

- **Linear Decay**: Reduces the learning rate by a fixed amount after each step.
- **Reduction Over Time**: Scales the learning rate exponentially with time.

### (2) Step Updates with SGD and Learning Rate Decay

Formulas depend on provided details.

### (3) Why Is Adam Preferred? Drawbacks?

- **Advantages**: Combines momentum and adaptive learning rates.
- **Drawbacks**: Sensitive to hyperparameters and prone to overfitting.

---

## **09**

### (1) Why Use L1/L2 Regularization?

- **L1 Regularization**: Promotes sparsity by penalizing absolute weights.
- **L2 Regularization**: Penalizes large weights to prevent overfitting.

### (2) L2 Regularization in Formulas

Add $\frac{\lambda}{2} \|W\|^2$ to the loss function:

$$L = \text{MSE} + \frac{\lambda}{2} \|W\|^2$$

### (3) L1 or L2 for a Given Problem?

- Use **L1** for sparsity (e.g., feature selection) and **L2** for regularizing large weights.

---

## **10**

### (1) How Does Dropout Reduce Overfitting?

- Randomly drops nodes during training, preventing over-reliance on specific features.

### (2) What Is Early Stopping?

- Stops training when validation loss stops improving.

### (3) What Is Data Augmentation?

- Expands dataset diversity with transformations (e.g., flipping, rotation).

### (4) Dropout in PyTorch
```python
import torch.nn as nn
dropout = nn.Dropout(p=0.5)
```
### (5) Difference in Training/Evaluation with Dropout

- Dropout is applied during training but disabled during evaluation using `model.eval()`.
## **11**

### (1) Why Use CNNs for Image Data?

- CNNs exploit spatial hierarchies and reduce parameter count using local connections and shared weights.

### (2) Output Dimension of Convolution

$$\text{Output} = \frac{\text{Input} - \text{Kernel} + 2 \cdot \text{Padding}}{\text{Stride}} + 1$$

### (3) Could AI Succeed Without Convolutions?

- Convolutions are crucial for processing high-dimensional data like images efficiently.

---

## **12**

### (1) Why Use Batch Norm? How Does It Work?

- Batch Norm normalizes activations to stabilize and accelerate learning.

### (2) Batch Norm Forward/Backward Path

Specific formulas depend on network details.

### (3) Covariate Shift and Batch Norm

- **Covariate Shift**: Changes in input distributions during training.
- Batch Norm mitigates this by standardizing activations.

### (4) Training with Small Dataset

- Use **transfer learning** with pre-trained models like ImageNet.

---

## **13**

### (1) RNN vs. FC/CNN Layers

- RNNs process sequential data, maintaining temporal dependencies.

### (2) LSTM Layer Components

Include forget gate, input gate, cell state, and output gate. Each addresses sequential dependencies.

### (3) Advantages of RNNs

- Capture temporal features better than 1D convolutions (e.g., text generation).

---

## **14**

### (1) Reasons for Vanishing/Exploding Gradients

- **Vanishing**: Sigmoid/tanh activations.
- **Exploding**: Large weight initialization.

### (2) Avoiding Gradient Issues

- Use ReLU, gradient clipping, or better initialization methods.

### (3) Addressing Underfitting

- Increase model complexity, improve data quality, or enhance training time.

---

## **15**

### (1) Effect of Incorrect Hyperparameters (e.g., Small Learning Rate)

- Slow convergence, potential stagnation in local minima.

### (2) Bayesian Optimization for Hyperparameters

- Uses probabilistic modeling to efficiently find optimal hyperparameters.

### (3) Exhaustive Search Duration

- Total combinations: $54=6255^4$. Each takes 2 hours: $625×2=1250$ hours.
- Alternative: Use **random search** or **Bayesian optimization**.

---

## **16**

### (1) Difference: Supervised vs. Unsupervised Learning

- **Supervised**: Labeled data (e.g., classification).
- **Unsupervised**: Unlabeled data (e.g., clustering).

### (2) Examples of Supervised/Unsupervised Problems

Specific examples required.

### (3) Success of Unsupervised Learning

- Discovers hidden structures without labels.

---

## **17**

### (1) Hierarchical vs. Non-Hierarchical Clustering

- **Hierarchical**: Builds a tree of clusters.
- **Non-Hierarchical**: Pre-defines number of clusters (e.g., k-means).

### (2) K-Means Clustering

Compute iterations based on input data.

### (3) DBSCAN Advantages

- Handles noise and non-spherical clusters better.

---

## **18**

### (1) Importance of Reward in RL

- Guides the agent by defining the objective.

### (2) Discounted Return

$$G_t = R_t + \gamma R_{t+1} + \gamma^2 R_{t+2} + \ldots$$

Ensures convergence by prioritising immediate rewards.

### (3) MDP for Autonomous Car

- **States**: GPS coordinates.
- **Actions**: Steering, acceleration.
- **Rewards**: Staying on the road.
- **Transitions**: Physics simulation.