---
title: 'Chapter 4: Convolutional neural network'
description:
  'This chapter will introduce CNNs'
prev: /chapter3
next: /chapter5
type: chapter
id: 4
---

<exercise id="26" title="What is a CNN?" type="slides">

<slides source="chapter4_01_cnn">
</slides>

</exercise>

<exercise id="27"  title="Hands on - CNNs">
    Use a convolutional neural network for classifying the 32 by 32 colored images.
    The first convolutional layer should have 16 filters with a 3 by 3 kernel, followed by a max pooling.
    The second convolutional layer should have 32 filters with a 3 by 3 kernel.
    The last convolutional layer should be connected via a flatten layer to a dense layer with 32 neurons.
    Throughout the hidden layers, relu activations are used
<codeblock id="04_01">

Use a relu activation for the hidden layers.

</codeblock>
</exercise>

<exercise id="28" title="Batch Norm and Spatial Dropout" type="slides">

<slides source="chapter4_02_batch_norm_spatial_dropout">
</slides>
</exercise>

<exercise id="29"  title="Hands on - CNNs (2)">
    This given network is overfitting. Therefore one decides to use Spatial Dropouts for the convolutional layers as well as Dropouts for the fully connected layers.

    Dropout rates are 0.2, 0.5, 0.5 - Fill in the blanks

<codeblock id="04_02">

You can specify the dropout rates in the parameters of the layer.

</codeblock>
</exercise>

<exercise id="30" title="Performance comparison">
    Execute the notebook convolutional_neural_network.ipynb.
    What are the results comparing simple_nn (model 1), batch_norm_nn (model 2) and dropout_nn (model 3)
<choice id=1>

<opt text="Model 3 is performing worse because the network is very small (underfitting) and dropping out essential information is decreasing the performance dramatically" correct="true">

Correct!

</opt>

<opt text="Model 2 performs best because the learning curve is smoother">

This is not the correct answer

</opt>

<opt text="Model 1 is performing worst as it does not use dropout and batch normalization">

This is not the correct answer

</opt>
</choice>
Set the epochs to 5.
Change the batch size on model 1 from 8 to 1. In a second run from 1 to 20000.
Watch the accuracy and time per epoch the network needs

<choice id=2>
<opt text="When batch size of 1 (stochastic gradient descent) is used, the network trains faster">

This is not correct!

</opt>

<opt text="The network trains faster when the batch size is large, but it takes more epochs to train" correct="true">

Good job!

</opt>

<opt text="The network trains fastest with batch normalization">

This is not the correct answer

</opt>

<opt text="The network trains faster when the batch size the dataset size">

This is not the correct answer. Even in this small example, the computer goes Out of memory (OOM)

</opt>

</choice>

</exercise>

<exercise id="31" title="Some tricks for deep learning" type="slides">
    <slides source="chapter4_03_some_tricks_with_neural_networks">
</slides>

</exercise>

<exercise id="32" title="Playing with the code - Analyse different CNNs">

Use the Cifar 10 Datasets. Try out different settings of a CNN with: 

a) Different Kernel sizes 
b) Different Number of Layers
c) Dropout/Spatial Dropout
d) Different Hyperparameters: BatchSizes, Optimizer, Learning Rates

... to get a feeling for CNNs

Hint: It is worthwhile to compare SGD (Stochastics Gradient Descent) to Adam, when the same architecture with the same batch size and number of epochs is trained?

</exercise>