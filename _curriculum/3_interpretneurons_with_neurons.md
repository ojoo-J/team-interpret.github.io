---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
    - name: Youngju Joung
      affiliations:
        name: KAIST
    - name: Enver Menadjiev
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: true
disqus_comments: false
date: 2023-08-14
featured: true
title: '1.3 Interpret Neurons with Neurons'
description:  '📙 Chapter 1. Neuron <br> <em> Can you find meaningful neurons with neurons?</em>'
img: "/assets/img/neurons_interpret_neurons/interpretation_neuron.png"
---


## Interpret Neurons with Neurons


Inside of a deep neural network, numerous neurons exist, and each of them activated by some specific patterns. We can easily assume the existence of such neurons in the complex inside, but finding such neurons requires expensive efforts such as Network Dissection and  CLIP Dissection. Unlike visualizing the detected region of a neuron, a much easier way is learning a classifier to verify the semantic meaning of neurons so that the weight of the classifier reveals the relationship between the activation of the neuron and a specific pattern. 
To further understand the easiness of such a method, assume the situation where we want to find a neuron detecting “The Stripe Pattern in Zebra”.

* **Difficulty 1**: there are numerous neurons in a network which are fired with a zebra image. <br> (Multiple neurons are activated by a single image)  
* **Difficulty 2**: we don’t know how much to be activated for the pattern.  <br> (The proper threshold for activation)
* **Difficulty 3**:  The proportion of activation might not be 100%. In what threshold, can we Difficulty that the neuron has the semantic pattern? <br> (How much # Activated Samples / # Total Samples? )

These difficulties has the same goal: 
> How can we determine the features of a neuron with the activation patterns?

To solve this problem with another deep neural network, we must consider two types of neurons, a neuron capturing a semantic pattern and a neuron interpreting the activation of the neuron. Two neurons have the following domain and co-domain.
* Pattern neuron : Input → Activation Score ($x \mapsto h$)
* Interpretation neuron :  Activation Score → Concept ($h \mapsto C$) 

## Pattern Neuron 

<figure style="text-align:left; display:block;width:100;" >
<img src="/assets/img/neurons_interpret_neurons/feature_neuron.png" style="width:100%">
<figcaption markdown="1">
Figure. Several inputs are mapped to the neuron's output. They leave activation signals.  With the assumption that the neuron has semantic meaning, large value of `F` will be considered to have more degree of a feature. 
</figcaption>
</figure>


A pattern neuron has positively correlated activation patterns with respect to a specific semantic image, such as strip patterns. As the intensity of the activation is stronger, the activation of the neuron is higher. On the other hand, if the pattern does not exist in the input, the neuron must not propagate signals.  In addition, this assumption could be stochastic as the activation patterns could have noise. 

> **Formulation for a pattern neuron** 
* $x$ : an input 
* $h$ : activation of the neuron
* $f_n$ : a function maps input $x$ to activation $h$.
* $c_n$ : the concept related to neuron $n$ 


## Interpretation Neurons

The interpretation neuron takes activation score as an input and outputs the decision of concept with a separate neuron. Note that the interpretation neuron is not internal neurons of the original network. They are separate neurons which will be trained for the interpretation of activation values. 


<figure style="text-align:left; display:block;width:100;" >
<img src="/assets/img/neurons_interpret_neurons/interpretation_neuron.png" style="width:100%">
<figcaption>
Figure. Several inputs are mapped to the neuron and leave activations. The interpretation neuron will be trained to classify the concept patterns with the activation signals. 
</figcaption>
</figure>

The interpretation neuron is a function $g$ which can have the most simplest form as follows:

$$
g_n = \sigma (wh+b) 
$$

Here $\sigma$ is a sigmoid function to be used for the decision of a concept. With such formulation we can interpret the decision as follows:
If there is positive correlation between $h$ and concept $c$, the weight will be positive 
If there is no correlation between the activation and concept, the weight will be close to zero. 

> **Formulation for an interpretation neuron**
* $h$ : an input 
* $c$ : the concept to be verified for an input
* $g$ : a mapping from $h$ to $c$. 
* $w,b$ : weight and bias for the classifier $g$. 

This classifier can help us to mitigate the difficulties 2 and 3 (The proper threshold for activation, and How much # Activated Samples / # Total Samples?). However, difficulty 1 (numerous neurons) is not solved with learning a classifier with input size 1. To solve it, we further formulate a classifier with high-dimensional input space.  




## Interpretation Neuron for Multiple Neurons 

There is not much difference between 1D input space and high-dimensional input space. We just widen the input space. However, this simple trick will ease the interpretation of a neuron and the combination of several neurons.  

> **Formulation for an interpretation neuron**
* $\mathbb{h}$ : an input 
* $c$ : the concept to be verified for an input
* $g$ : a mapping from $h$ to $c$. 
* $W, \mathbf{b}$ : weight and bias for the classifier $g$. 

Most previous work uses the representation of a layer for an input. The verification of a concept is done by learning a classifier whose input is a layer-representation and the output is a boolean for a specific concept. Note that when we can actually learn this, we may say there is a concept in activations. Even though this formulation has probed neurons with layer-representation, we don’t have to limit the function. We can learn a classifier whose input is the whole activation of a network. In this case, we can verify the neurons at once! 




## Probing 

The verification of neurons is called *probing*. We probe the semantic information of  neurons. The overall algorithm is as follows:

Prepare samples
Positive  : Inputs which have concepts in its input space.  (Label 1) 
Negative : random or opposite conceptual images. (Label 0)  
Gather all activations of a network with samples in Step 1. 
Replace inputs in Step1 with the activations in Step2 
Train a classifier whose input is the activation score and the concept labels. 
Check the accuracy to see the positive correlation between the activations and the concept. 
Check the weight to verify the important neurons. 


### Linear Probing 

If you use a linear classifier in probing, it is called linear probing. 
The advantage of linear probing is the ease of weight interpretation.  As we want to verify the semantic information of neurons, we must know the relation between each neuron and the concept. The weight can provide this information

### Non-linear Probing  

If you use a non-linear classifier in probing, it is called linear probing. 
 The advantage of non-linear probing is that the concept could be complex and the linear relationship between neurons and concepts may not be enough, but the non-linear relationship may be.



## Interpret Chess Concepts

Chess has many meaningful concepts in board states such as “Checkmate”, “ “. 
Thomas et al. <d-cite key="mcgrath2022acquisition"/> investigate the concepts in chess with AlphaZero model.
Note that the network obtains only a board state and outputs preferred action and there is no explicit encode of concepts. Therefore, verification of neurons is about the existence of meaningful chess concepts in the internal representations. 


<figure style="text-align:left; display:block;width:100;" >
<img src="/assets/img/neurons_interpret_neurons/chess_concepts.png" style="width:100%">
<figcaption markdown="1">
Figure.  *What-when-where* plot to visualize the emergence of concepts in the network. Note that the z-axis is the accuracy of the probing network. 
</figcaption>
</figure>


<figure style="text-align:left; display:block;width:100;" >
<img src="/assets/img/neurons_interpret_neurons/regression.png" style="width:100%">
<figcaption markdown="1">
Figure. Regression probing. 
</figcaption>
</figure>


## Testing with Concept Activation Vector 

What natural question followed by the concept is the usefulness. 
That is, how important the concept stripe is for the class zebra. Similarly, how much is the concept red important for the class firefighter? To answer this question, we need a qualitative measure. Testing with Concept Activation Vector (TCAV) <d-cite key="kim2018interpretability"/> is a measure which counts the number of samples that increased the class logic as the activation intervened with concept direction. 


Assume that we obtained the directional vector $v$  for the concept $C$. If the direction increases the concept, we get intervene the activation with the direction by adding,
$f_l(x) + \epsilon v_C^l$. The answered question in TCAV paper is as follows: 

> If the conceptual information added to the representation, will it increase the classification logit? 

For example, if we increase the black and white stripe patterns, the likelihood of zebra class will be increased. TCAV is a quantitative measure to this question and answers it with the ratio of samples which increased the logic. 


### Conceptual Sensitivity of class k to concept C 

$$ 
\begin{aligned}
S_{C,k,l}(\mathbf{x}) 
&= \lim_{\epsilon \rightarrow 0} \frac{h_{l,k}(f_l (\mathbf{x}) + \epsilon v_C^l) - h_{l,k}(f_l(\mathbf{x}))}{\epsilon} \\
&= \nabla h_{l,k}(f_l(\mathbf{x})) \cdot v_C^{l}
\end{aligned}
$$


### TCAV 

Now the authors defined TCAV as follows. 

$$
\mathrm{TCAV}_{Q_{C, k,l}} = \frac{\vert \{ x\in X_k : S_{C,k,l} (x) > 0  \}\vert }{\vert X_k \vert}
$$



