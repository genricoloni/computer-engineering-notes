# Introduction to Artificial Neural Networks

## Introduction

### What is a Neural Network?

An **Artificial Neural Network (ANN)** is an abstract simulation of a human nervous system, that contains a collection of neurons connected with each other trough connections called **axons**. The aim of a neural network is to simulate neurons and connections, resembling the human brain.

### Neuron model

Many neurons have a structure called **dendrites** that receive signals from other neurons trough **synapses**. The neuron processes the signals and sends the output signal through the **axon**, which is basically an electrical impulse: it can be **excitatory** or **inhibitory**, and in case of excitatory signals, the neuron will generate informational messages to other neurons.

It's estimated that around 100 billion neurons are in the human brain, and each neuron can be connected to thousands of other neurons: their switching time is significantly slower than the switching time of a computer, but the connectivity is much higher, up to hundreds times more.

## Artificial Neural Networks

### Structure of an ANN

An NNN is composed by a set of **neurons** and **weighted connections** between them, plus a series of thresholds or **activation levels**. During the design of an ANN, we have to take into account the **number and type of neurons**, the **morphology of the network**, the **weights** and the **training examples**, in terms of network inputs and outputs.

### Artificial Neuron

The scheme of an artificial neuron is shown in the following image:

![Artificial Neuron](https://upload.wikimedia.org/wikipedia/commons/thumb/6/60/ArtificialNeuronModel_english.png/400px-ArtificialNeuronModel_english.png){width=400px}){width=400px}

Note that the artificial neuron has a set of **inputs** $x_1, x_2, \ldots, x_n$ and a set of **weights** $w_1, w_2, \ldots, w_n$, which are real number such that, if positive, they are excitatory, and if negative, they are inhibitory; these weights are related to the corresponding inputs. The neuron computes the **weighted sum** of the inputs and weights as a linear combination $a = \sum_{i=1}^{n} w_i \cdot x_i$, that goes into an **activation function** $\phi(a)$. The output of the neuron can be a real number, both non-limited or limited to a certain range, or even a discrete value. Lastly, there also is a binary threshold function such that, if the output is above a certain threshold $\theta_i$, the neuron will fire. In fact, the latter is a generalization of a **step function** for a given threshold. Typically, the threshold is also subtracted from the weighted sum, in order to have the function $y = f(\sum_{i=1}^{n} w_i \cdot x_i - \theta_i) = f(\sum_{i=0}^{n}w_i \cdot x_i)$ **centered in zero**. If a threshold assumes a negative value, is called **bias**, and it's considered as a weight connected to a unit that always outputs 1 (note that in the previous equation we added the index 0 to the sum, which is the bias).

### Activation functions

In the following image, we can see some examples of activation functions:

![Activation functions](../images/03/activationFunctions.png){width=400px}

### Network topology

The topology depends on **how neurons are connected which each other**: if they're arranged in a **hierarchical** way, such that the neurons are connected only with **adjacent layers**, we have a **feedforward network**. If the neurons are connected with **non-adjacent layers**, we have a **recurrent network**. We can see an example of these two types of networks in the following image:

![Feedforward and Recurrent Networks](../images/03/hierarchy.png){width=400px}

Note how the firsts neurons are connected only with the second layer in the feedforward network, while in the recurrent network they this rule is not respected; in particular, in this case, the network is implementing a sort of *memory*, with a structure that resembles a **flip-flop**.

## Network training