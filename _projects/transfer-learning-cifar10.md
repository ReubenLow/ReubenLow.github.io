---
layout: page
title: "Transfer Learning for CIFAR-10: ResNet, CLIP and DINOv3"
description: A comparative study of supervised, weakly-supervised and self-supervised pre-trained backbones.
img: assets/img/ai-robotics/tsne-dinov3.png
importance: 7
category: work
---

Mini-project for the AI Robotics module at the Singapore Institute of Technology.

<!-- Objective of the transfer learning study -->

## Objective

To compare three pre-trained vision backbones on the same classification task, each representing a different point in how computer vision models have been built and trained:

- **ResNet-18** (2015), a convolutional network trained with supervised learning on 1.28 million labelled ImageNet images.
- **CLIP ViT-B/16** (2021), a Vision Transformer trained with weak supervision on 400 million image and text pairs, learning by matching images to their captions.
- **DINOv3 ViT-B/16** (2025), a Vision Transformer trained with full self-supervision on 1.7 billion images and no human labels at all, learning by recognising that two crops of the same image show the same thing.

Together these trace two shifts at once: convolutional networks to Transformers, and supervised labelling to self-supervised learning. Holding the task and the training setup fixed isolates the quality of the features each backbone already carries.

## Method

Every backbone was frozen and only a linear classifier was trained on top, which makes this a measurement of the pre-trained features themselves and not of fine-tuning. The head has 7,690 trainable parameters.

CIFAR-10 provides 60,000 images at 32 x 32 across ten classes. The 50,000 training images were split 45,000 for training and 5,000 for validation on a fixed seed, with the 10,000 test images held out entirely. All images were upscaled to 224 x 224 by bilinear interpolation to match what the backbones expect, and normalised with ImageNet statistics. Training used random horizontal flips and random crops with 4-pixel padding, applied on the fly so each epoch sees a slightly different version of every image.

Reproducibility required seeding Python, NumPy, PyTorch and CUDA, and storing the full random number generator state in checkpoints so a run could resume exactly.

## Results

| Metric           | ResNet-18 (published) | CLIP ViT-B/16 | DINOv3 ViT-B/16 |
| ---------------- | --------------------- | ------------- | --------------- |
| Test accuracy    | ~93 to 95%            | 94.87%        | **97.78%**      |
| Test F1 (macro)  | ~93 to 95%            | 94.87%        | **97.78%**      |
| Top-5 accuracy   | ~99.5%                | 99.95%        | **99.98%**      |
| Early stop epoch | n/a                   | 11 / 50       | 43 / 50         |

The ResNet-18 column comes from published benchmarks, not from a run of my own, so it is there for context only. Putting ResNet-18 through this same pipeline for a directly comparable measurement is the obvious next step.

For scale, a ViT-B/16 trained from scratch on CIFAR-10 reaches roughly 67 to 80%. Transformers have none of the spatial assumptions that help a convolutional network learn from small images, so starting from pre-trained features is worth around 20 percentage points here.

The early stopping behaviour is worth noting on its own. CLIP plateaued at epoch 11 while DINOv3 kept improving until epoch 43, which suggests DINOv3's features gave the linear head a richer signal to work with for far longer.

## Feature Separability

Projecting the frozen backbone features with t-SNE shows the result before any classifier is involved. DINOv3 produces tighter, better separated clusters, which is what a linear probe needs.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/ai-robotics/tsne-clip.png" title="t-SNE of CLIP backbone features" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/ai-robotics/tsne-dinov3.png" title="t-SNE of DINOv3 backbone features" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    t-SNE of the frozen backbone features, CLIP on the left and DINOv3 on the right. Click either to enlarge.
</div>

## Where the Errors Are

Every model in the comparison struggles with the same thing. Vehicle classes separate cleanly, and the animal classes do not, with cat and dog the hardest pair. At 32 x 32 they share fur texture, body shape and pose, and bilinear upscaling to 224 x 224 blurs exactly the fine detail that would tell them apart.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/ai-robotics/confusion-clip.png" title="CLIP confusion matrix on the test set" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/ai-robotics/confusion-dinov3.png" title="DINOv3 confusion matrix on the test set" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Test set confusion matrices, CLIP on the left and DINOv3 on the right. Cat and dog account for most of the remaining error in both.
</div>

Counting the cat and dog confusions directly shows the progression: 130 for the ResNet-18 benchmark, 126 for CLIP, and 73 for DINOv3, which is 42 dogs called cats and 31 cats called dogs. Per-class F1 on cat moves the same way, from about 0.88 to 0.89 to 0.94.

## What the Comparison Shows

**Transformers held an advantage on the classes that need global structure.** A convolutional network builds context gradually through stacked local filters, losing spatial detail at each pooling stage. Self-attention relates every patch to every other from the first layer, which suits a task like separating cat from dog where ear shape, body proportion and tail have to be compared across the whole image at once.

**Less human supervision produced better features.** This is the counterintuitive result. ResNet's features are optimised for ImageNet's 1,000 categories and carry that shape. CLIP's come from natural language captions and generalise further. DINOv3's come from no labels at all and transferred best of the three. The practical consequence is that the most expensive part of deploying a vision system, labelling data, is also the part that these results suggest you need least of.

**The best model still depends on the deployment.** ResNet-18 at around 11.7M parameters remains the sensible choice on constrained edge hardware. CLIP brings multimodal abilities such as zero-shot classification that neither of the others has. DINOv3 wins when feature quality is what matters and the compute budget allows it.

<!-- Skills Deployed -->

## Skills Deployed & Responsibilities

- Machine Learning
  - Transfer learning with frozen-backbone linear probes
  - Comparative evaluation under a fixed training configuration
  - Early stopping, learning rate scheduling, data augmentation
- Model Analysis
  - Per-class precision, recall and F1
  - Confusion matrix and error analysis
  - t-SNE feature visualisation
- Reproducibility
  - Seeded runs across Python, NumPy, PyTorch and CUDA
  - Checkpointed RNG state for exact resume
- Tooling
  - PyTorch, CIFAR-10, CLIP and DINOv3 backbones
