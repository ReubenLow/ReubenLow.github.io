---
layout: page
title: "Transfer Learning for CIFAR-10: ResNet, CLIP and DINOv3"
description: A comparative study of supervised, weakly-supervised and self-supervised pre-trained backbones.
img: assets/img/ai-robotics/tsne-dinov3.png
importance: 2
category: work
---

<!-- Objective of the transfer learning study -->

## Objective

Compare three pre-trained vision backbones on the same classification task. Each comes from a different stage in how vision models are built and trained:

- **ResNet-18** (2015), a convolutional network trained with supervised learning on 1.28 million labelled ImageNet images.
- **CLIP ViT-B/16** (2021), a Vision Transformer trained with weak supervision on 400 million image and text pairs, learning by matching images to their captions.
- **DINOv3 ViT-B/16** (2025), a Vision Transformer trained with self-supervision on 1.7 billion images and no human labels, learning by recognising that two crops of the same image show the same thing.

The three span two changes in the field: convolutional networks to Transformers, and supervised labelling to self-supervised learning. Keeping the task and the training configuration identical isolates the quality of the pre-trained features.

Transfer learning matters most where labelled data is expensive. A warehouse sorting robot has to identify objects on a conveyor belt, and labelling thousands of images for every new product line is impractical. A backbone that already encodes general object features can be adapted to new categories from a small set of labelled examples.

## Method

Each backbone was frozen and only a linear classifier was trained on top. The backbone weights never change, so the experiment measures the pre-trained features and not the effect of fine-tuning. The classifier head has 7,690 trainable parameters.

CIFAR-10 provides 60,000 images at 32 x 32 across ten classes. The 50,000 training images were split into 45,000 for training and 5,000 for validation on a fixed seed, with the 10,000 test images held out. All images were upscaled to 224 x 224 by bilinear interpolation to match the input size the backbones expect, and normalised with ImageNet statistics. Training used random horizontal flips and random crops with 4-pixel padding.

Runs were seeded across Python, NumPy, PyTorch and CUDA, and checkpoints stored the full random number generator state so a run could resume exactly.

CLIP and DINOv3 ran through the same data pipeline: the same 45,000/5,000 split on the same seed, the same deterministic evaluation transform, and the same 10,000-image test set. The classifier head and training configuration were identical. The backbone is the only variable between them.

## Results

| Metric     | ResNet-18 | CLIP ViT-B/16 | DINOv3 ViT-B/16 |
| ---------- | --------- | ------------- | --------------- |
| Accuracy   | ~93-95%   | 94.87%        | **97.78%**      |
| Macro F1   | ~93-95%   | 94.87%        | **97.78%**      |
| Top-5      | ~99.5%    | 99.95%        | **99.98%**      |
| Early stop | n/a       | 11 / 50       | 43 / 50         |

I did not run ResNet-18 in this pipeline. Those published baselines train ResNet-18 from scratch on CIFAR-10, with every weight learning and the images left at 32 x 32. This experiment freezes the backbone and trains only the classifier on top. So the ResNet figure measures how well the architecture learns CIFAR-10 from nothing, and the other two measure how good their pre-trained features already are. All three are scored on the same 10,000 test images, but they are not answering the same question. Running ResNet-18 here as a frozen ImageNet-pretrained backbone, under the same linear head, would make all three comparable.

A ViT-B/16 trained from scratch on CIFAR-10 reaches roughly 67 to 80%. Transformers lack the spatial inductive bias that helps a convolutional network learn from a small dataset, so pre-training accounts for about 20 percentage points here.

CLIP stopped improving at epoch 11. DINOv3 continued to epoch 43. The classifier head and the training configuration were identical, so the difference is in how much usable signal each backbone's features carry.

## Feature Separability

Each image passes through the frozen backbone and comes out as a vector of numbers, 768 of them for these models. t-SNE compresses those vectors down to two dimensions so they can be plotted, placing images with similar feature vectors near each other. If a backbone already separates the classes, the plot shows ten distinct clusters. If it does not, the clusters overlap. This inspects the features on their own, before any classifier is trained.

DINOv3's clusters are tighter and more separated than CLIP's, which is what allows a single linear layer to reach 97.78%.

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

All three models fail on the same classes. Vehicle classes separate cleanly. Animal classes do not, and cat and dog are the hardest pair. At 32 x 32 the two share fur texture, body shape and pose, and bilinear upscaling to 224 x 224 blurs the fine detail that separates them.

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

Total cat and dog confusions: 130 for the ResNet-18 benchmark, 126 for CLIP, and 73 for DINOv3, made up of 42 dogs classified as cats and 31 cats classified as dogs. Per-class F1 on cat follows the same order, from about 0.88 to 0.89 to 0.94.

## What the Comparison Shows

**Transformers did better on classes that need global structure.** A convolutional network builds context through stacked local filters and loses spatial detail at each pooling stage. Self-attention relates every patch to every other patch from the first layer. Separating cat from dog requires comparing ear shape, body proportion and tail across the whole image, which is the case where that difference matters.

**Less human supervision produced better features.** ResNet's features are optimised for ImageNet's 1,000 categories. CLIP's come from natural language captions and generalise further. DINOv3's come from no labels at all and transferred best of the three. Labelling is the most expensive part of deploying a vision system, and the backbone trained without labels performed best.

This holds outside benchmarks. NASA's Jet Propulsion Laboratory uses the DINO family of models in Mars exploration robots for terrain mapping and object recognition under tight compute budgets. The World Resources Institute runs DINOv3 over satellite imagery to track deforestation in Kenya at continental scale, with no manually labelled training data.

**Model choice depends on the deployment.** ResNet-18 at around 11.7M parameters remains the practical option on constrained edge hardware. CLIP supports zero-shot classification and image-text matching, which neither of the others does. DINOv3 is the choice where feature quality matters and the compute budget allows it.

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
