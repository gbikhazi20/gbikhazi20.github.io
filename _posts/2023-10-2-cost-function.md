# But what exactly is the cost function?

Last semester, I took my first machine learning class in college. We spent a lot of time learning classical machine learning methods (think linear regressions, logistic regression, k-nearest neighbors, etc.), and eventually made our way to modern neural networks.

From the beginning of the course, I made it a point to try understand all the math behind the algorithms we learned. I only did this as much was possible as there were a few concepts I was not able to cover in full (Im looking at you, [backpropagation](https://en.wikipedia.org/wiki/Backpropagation)). I plan to delve into these as soon as I get the chance. Interestingly enough, though, it was possible to grasp the rest of the material fairly well despite a less than thorough understanding of these few topics.

One of the topics that I was not willing to comprehend only in part was that of the [cost function](https://en.wikipedia.org/wiki/Loss_function). I struggled a lot with this concept and spent a lot of time reading the textbook over and over again until it finally clicked. Before describing my trouble with the cost function and my eventual understanding of it, I'll first give some background on how neural networks work to set the scene.

Neural networks are, in essence, a giant graph of nodes and edges.

<br/>

<p align="left">
<img src="https://raw.githubusercontent.com/gbikhazi20/gbikhazi20.github.io/main/_assets/neural_networks-001.jpg" width="400" height="286">
</p>

<br/>

There is an input layer, which is connected to the hidden layers by edges which have weights. The hidden layers feed into each other, also through edges with weights. Finally, the hidden layers eventually spit out activations in the output layer.

Another way to think about this network is as a function $`f(x) = y`$ where $x$ is an input vector and $y$ is the output of the network in the final layer.

For this function to output anything that makes sense, all its knobs need to be turned to the right settings — aka the right weights and activations in the network must be _learned_.

How on earth do we turn the knobs to the right settings? The activations and weight could seemingly be set to anything, so how do we know where to start? And how do we improve once we start?

It might help to start with a practical example.

One of the most popular examples given to explain the use case of neural networks is that of digit recognition.

Suppose you want to write a computer program that is capable of associating an image of a digit with the actual digit it represents.

<br/>

<p align="left">
<img src="https://raw.githubusercontent.com/gbikhazi20/gbikhazi20.github.io/main/_assets/mnist.jpg" width="400" height="235">
</p>

<br/>

We can represent a black and white image as a vector of numbers, each number representing how dark or bright a particular pixel is. In the popular [MNIST Dataset](https://en.wikipedia.org/wiki/MNIST_database) (samples pictured above), each image is 28x28 pixels large, or 784 pixels in total. An array of length 784 is what we could pass to our network's input layer or our $x$ to our function $f$.

We can set the weights and biases in our network to random weights, do some linear algebra throughout the network, and compute an output based on an input image. But our results will most likely be unuseful. We would ideally like our network to output an array of length 10 that looks like $[0, 0, 0, 0, 1, 0, 0, 0, 0, 0]$ where each position is the network's prediction for whether that digit is in fact the input digit (so this example output array would represent a 4). A randomly tuned network might output something like $[0.5, 0.7, 0.8, 0.7, 1, 0.2, 0.6, 0, 0, 1]$, which does not give a clear indication to any digit being a winner.

We can set our network randomly to start, but to learn the best weights and biases for classification, we'll need a tuning process more clever than picking random numbers. This is where the cost function comes in handy.

The cost function helps us compare how far away our current guess is from reality. Mathematically, this looks like so:

<br/>

<p align="left">
<img src="https://raw.githubusercontent.com/gbikhazi20/gbikhazi20.github.io/main/_assets/cost-function-formula.jpg" width="322" height="67">
</p>

<br/>

$y(x)$ can be thought of as our current guess, or the network's output given an input $x$, and $a$ is the ground truth prediction vector which would come from some training dataset. We find the distance between these two vectors, square it, and sum for every single point in our training dataset.

<br/>

<p align="left">
<img src="https://raw.githubusercontent.com/gbikhazi20/gbikhazi20.github.io/main/_assets/cost-function-graph.jpg" width="500" height="385">
</p>

<br/>

Visually, I find this 3D graph of a convex function to be super helpful.

If we look back to our formula for the cost function, we notice $C$ is a function of $w$ (weights) and $b$ (biases). This makes sense — our loss is dependent on a prediction, and a prediction cannot be made independent of weights and biases in a network.

The graph above displays a $C$ which hinges on $v1$ and $v2$, an arbitrary combination of variables meant to represent weights and biases. Every single point on this graph is the result of calculating $C$ for a very specific combination of weights and biases. As we tune our network we move across this cost function. In reality, this graph will be in far, far higher dimensions that we are not exactly able to visualize, but the main idea remains the same. We have a cost function we are able to move across by tuning the weights and biases in our network, and we would like to move to a minimum in this function to minimize cost or loss (aka increase the accuracy of our predictions!).

What's really interesting is that, for most real world examples, it is practically impossible to calculate $C$ for every combination of the $w$s and $b$s making up the network. In the diagram provided above, the network has 85 edge weights 18 biases to learn (meaning our simple 3 dimensional graph from above would now be in a 103 dimensional space). It would computationally very intensive to map out this function, and this is for a very small network.

Fortunately, we need not panic since some clever techniques in computer science have made it possible to converge to a local minimum in $C$ without having it all mapped out. One of these clever techniques is called [gradient descent](https://en.wikipedia.org/wiki/Gradient_descent). Given any point on the cost function, gradient descent allows us to find the direction to take for each parameter that will take the steepest step down the cost function, which will help us eventually converge to a local minimum. Interestingly, gradient descent does not guarantee converging to the global minimum.

### Sources

https://en.wikipedia.org/wiki/Backpropagation\
https://en.wikipedia.org/wiki/Loss_function\
https://tikz.net/neural_networks/\
https://en.wikipedia.org/wiki/MNIST_database\
http://neuralnetworksanddeeplearning.com/chap1.html
https://en.wikipedia.org/wiki/Gradient_descent\
