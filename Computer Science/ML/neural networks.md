
## Notes:
There is a difference between the conceptual implementation of a neural network (NN) and the mathematical implementation.

You can imagine the conceptual implementation as shown in videos about NNs, where each neuron receives values from the previous neurons. Its step-by-step.

In the mathematical implementation, it's different. Instead of computing for each neuron individually, everything is represented and computed as matrices.

**Input layer:**  
Each row represents a sample, and the columns represent features.

**Hidden layer:**
Each row represents the weights of a neuron. The columns are nothing special. 

Numpy: To use matrix multiplication in numpy use dot().

### Mathematical Explanation
For each step in the forward pass of a neural network, the following rule must be applied:

**result=np.dot(WEIGHT MATRIX,INPUT MATRIX)+BIASES**
#### Example and Shapes

Consider the following matrices:

- **Weight Matrix (WEIGHT MATRIX, wm)**: A matrix of shape 4×5
    - This matrix represents the weights connecting 4 neurons in the current layer to 5 neurons in the previous layer.
- **Input Matrix (INPUT MATRIX, im)**: A matrix of shape 5×1
    - This matrix represents the input features, with 5 values (one for each neuron in the previous layer).
