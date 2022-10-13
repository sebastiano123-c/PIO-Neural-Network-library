# **PIO-Neural-Network-library**
A light portable and easy-to-use Back Propagation Neural Network (BPNN) library for your projects.

# Installation
1) Clone this repository
```
clone git https://github.com/sebastiano123-c/PIO-Neural-Network-library.git
```
2) Browse into the `/build`, and copy the `BPNN` folder and paste it into your project `/lib` directory.

3) In your project platform.ini copy and paste these lines
```ini
build_flags =
  -Ilib/BPNN
  -Llib/BPNN
 	-lBPNN
```

# Usage
1) Write in your project `/src/main.cpp` file
```cpp
#include "BPNN.h"
```

2) BPNN library does NOT contain any class.
This is because class methods callings tends to be slower than direct calling.
So, you have to declare the following vectors and objects.
For more details see Wiki.
```cpp
// Define learning rate and momentum factor.
// These quantities have to be tuned basing on each case.
const float learningRate = 0.21f;
const float momentumFactor = 0.06f;

// Define the number of neurons for each layer.
// There are 3 layers: input, hidden, and output.
std::vector<int> structure = {1, 3, 1}; 

// Define the activation function type:
//  from input -> hidden layer: sigmoid 
//  from hidden -> output layer: sigmoid
// (see BPNN.h for other activation functions)
std::vector<const char *> activationType = {"sigmoid", "sigmoid"};

// Define number of layers.
const int numberOfLayers = structure.size();

// Define the layers' input states.
std::vector<std::vector<float>> zL(numberOfLayers);

// Define the layers' output states.
std::vector<std::vector<float>> aL(numberOfLayers);

// Define the bias vectors (rows of the matrix).
std::vector<std::vector<float>> bias(numberOfLayers - 1);

// Define the weights matrices (rows of the tensor).
std::vector<std::vector<std::vector<float>>> weights(numberOfLayers - 1);

// Define the deltas of the gradients: has the same dimension of
// bias and weights.
std::vector<std::vector<float>> deltaBias(numberOfLayers - 1);
std::vector<std::vector<std::vector<float>>> deltaWeights(numberOfLayers - 1);

// Define input and desired states.
std::vector<float> inputState(structure.at(0));
std::vector<float> desiredState(structure.at(-1));
```

3) Initialize these objects.
Each layers transition is represented by a matrix, which entries have to be initialized. 
Typically, random number generation is used.
```cpp
setup()
{

    // your setup ...

    init(structure,
        zL,
        aL,
        bias,
        deltaBias,
        weights,
        deltaWeights,
        1.0F,           // random range 
        10000);         // finesse of the random range
}
```

4) Forward-propagate the new loop values
```cpp
loop()
{
    // your loop ...
    
    inputState = {0.6}; // define input state

    void forwardPropagation(structure,
                            inputState,
                            &zL,
                            &aL,
                            &bias,
                            &weights,
                            activationFunctionName)
    
    // loop continues ...
```

4) Back-propagate the new loop values
```cpp
    // your loop ...

    desiredState = {0.5};   // define your desired output

    void backPropagation(structure,
                        desiredState,
                        &zL,
                        &aL,
                        &bias,
                        &deltaBias,
                        &weights,
                        &deltaWeights,
                        learningRate,
                        momentumFactor,
                        learningType,
                        activationFunctionName);

    // aL.at(-1) is the output of the neural network
    Serial.println(aL[2][0]);

} // end loop
```
