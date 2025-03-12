---
layout: page
title: Metric Embedding for Similarity Learning
description: Training models to embed images in feature spaces where similar images cluster together
img: assets/img/projects/metric-embedding/feature_space.jpg
importance: 1
# category: machine learning
category: fun
related_publications: true
---

This project explores **similarity learning** through metric embedding using the MNIST dataset. The core idea is elegantly simple yet powerful: train a neural network to transform images into a feature space where similar images are positioned closer together and dissimilar ones are pushed apart.

## The Problem: Similarity Beyond Classification

Traditional classification assigns discrete labels to images. But what if we want to know *how similar* two images are? This is where metric embedding shines - it creates a continuous representation where distance directly corresponds to similarity.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/metric-embedding/concept.jpg" title="Metric embedding concept" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Metric embedding transforms images into a feature space where similar items cluster together. The distance between points becomes a direct measure of their similarity.
</div>

## Triplet Loss: The Mathematical Heart

The key to training our embedding model is **triplet loss**. This clever loss function takes three inputs:
- An **anchor** image
- A **positive** image (similar to the anchor)
- A **negative** image (different from the anchor)

The loss function then encourages the network to position the anchor closer to the positive example than to the negative example, with a margin of separation:

$$\mathcal{L}(a, p, n) = \max(0, d(a, p) - d(a, n) + \text{margin})$$

Where $d(x, y)$ is the distance between the embeddings of $x$ and $y$.

<div class="row">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/metric-embedding/triplet_loss.jpg" title="Triplet loss visualization" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Triplet loss pushes the anchor closer to the positive example while pulling it away from the negative example.
</div>

## The Architecture

I implemented a convolutional neural network specifically designed for metric embedding:

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/metric-embedding/architecture.jpg" title="Model architecture" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The SimpleMetricEmbedding architecture uses convolutional layers with normalization to transform input images into an embedding space.
</div>

The core building block is a **BNReLUConv** module that combines:
- Group normalization for stable training
- ReLU activation for non-linearity
- Convolution for feature extraction

The network transforms 28×28 MNIST digit images into a compact embedding vector that captures the essential features for similarity comparison.

## Training Process

Training the model involves:
1. **Data preparation**: Adapting MNIST to provide triplets (anchor, positive, negative)
2. **Embedding extraction**: Passing images through the network to get feature vectors
3. **Loss calculation**: Computing triplet loss to guide the learning process
4. **Backpropagation**: Updating the network weights to minimize the loss

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/metric-embedding/training.jpg" title="Training progress" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Training progress showing the triplet loss decreasing over time as the model learns to create better embeddings.
</div>

## Results and Visualization

The power of this approach becomes evident when we visualize the learned embedding space. Using PCA to reduce the dimensionality, we can see how the model organizes digits in the feature space:

<div class="row">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/metric-embedding/image_space.jpg" title="Raw image space" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/metric-embedding/feature_space.jpg" title="Learned feature space" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left: PCA visualization of raw image pixels. Right: PCA visualization of the learned embedding space. Note how digits are much better separated in the feature space.
</div>

### Classification via Similarity

An interesting application is performing classification by finding the nearest neighbors in the embedding space. We compared:

1. **Image-space KNN**: Classification based on raw pixel distances
2. **Feature-space KNN**: Classification based on distances in the embedding space

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/metric-embedding/accuracy.jpg" title="Classification accuracy comparison" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Classification accuracy comparison between raw image space and embedding space. The metric embedding model significantly outperforms pixel-based classification.
</div>

## Generalizing to Unseen Classes

One of the most fascinating aspects of metric embedding is how well it generalizes to unseen classes. To test this:

1. We trained the model without showing it any examples of the digit "0"
2. We then evaluated how well it could embed and retrieve "0" digits, despite never seeing them during training

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/metric-embedding/zero_embedding.jpg" title="Zero digit embedding" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Even though the model was never trained on the digit "0", it places them in a distinct cluster in the feature space, demonstrating the power of similarity learning to generalize to unseen classes.
</div>

## Applications and Extensions

This technique has broad applications beyond handwritten digits:
- **Image retrieval systems**: Finding visually similar images in large databases
- **Face recognition**: Matching faces across different poses and lighting conditions
- **Anomaly detection**: Identifying unusual patterns that don't match known examples
- **Zero-shot learning**: Recognizing objects from classes not seen during training

## Key Takeaways

This project demonstrates how neural networks can learn meaningful similarity metrics that:
1. Capture complex patterns beyond simple classification
2. Organize data based on inherent similarity rather than arbitrary labels
3. Generalize to new, unseen examples
4. Provide a foundation for various similarity-based applications

The elegance of metric embedding lies in its ability to transform raw data into a structured space where proximity directly corresponds to similarity - a powerful concept that extends far beyond this simple MNIST example.

