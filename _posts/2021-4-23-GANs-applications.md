---
layout: post
title: 'GANs have applications beyond Image Editing'
---

A common view about GANs is that they are heavy computationally demanding
models with weird and untrackable optimization process that somehow manage to generate realistic images with
significantly lower diversity than original data and are mostly useful to make some kind of image editing. 

This post will discuss several recent articles (on layout and keypoints prediction, novel view synthesis, creation of synthetic datasets and labels) that challenge the last point of this view.


![Gans_applications](/images/non_image_editing_gans.png)

<!--more-->

# Why GANs?

Before diving into potential applications, let's discuss a bit why they rely on GANs.

Basically, for unconditional (or class-conditional, but not image2image) generation in image domain we have the following paradigms:

* Adversarial ([StyleGAN2](https://arxiv.org/abs/1912.04958), [BigGAN](https://arxiv.org/abs/1809.11096))
* Gradient estimation or denoising autoencoders ([DDPM](https://hojonathanho.github.io/diffusion/), [DDIM](https://arxiv.org/abs/2010.02502))
* Variational autoencoders ([NVAE](https://proceedings.neurips.cc/paper/2020/hash/e3b21256183cf7c2c7a66be163579d37-Abstract.html))
* Energy-based models ([Improved EBM](https://arxiv.org/abs/2012.01316))
* Autoregressive ([PixelSNAIL](http://proceedings.mlr.press/v80/chen18h.html), [Image-GPT](https://openai.com/blog/image-gpt))
* Invertible normalizing flows with exact log-likelihood estimation ([Glow](https://openai.com/blog/glow))
* Some mixture of the above ([VQ-VAE2](https://arxiv.org/abs/1906.00446), [Taming Transformers](https://compvis.github.io/taming-transformers))

Here I explicitly mentioned examples that currently scale to medium resolutions (like $128 \times 128$) or at least claim to do so (looking at you, VQ-VAE). Now, all of the applications we're to discuss rely on the simplest possible scheme to generate novel images - take pretrained network that samples some feature vector $z$ from a fixed latent distribution $p(z)$ and passes it through multiple layers of transformations.

This leaves behind Autoregressive, Denoising and Energy-based models - they simply need significantly more computation than a single forward pass. VAEs and Flows, although scalable, seem to significantly underperform GANs on medium-to-large resolutions (which is possibly a mixture of the facts that they rely on maximum likelihood estimation and they are less "fun" to work with cause you need to know at least some theory). 

Finally, we can choose to use the last set of "mixed" approaches, but they are just harder to work with. Besides for some, there are no pre-trained checkpoints available (looking at you, VQ-VAE). As a result, to use some pre-trained GAN currently looks like the best idea.

# GANs for feature extraction

To perform unsupervised learning we can extract features of some generative model. This is a very old idea, and dates even back to pre-Imagenet era when Neural Networks were initialized not randomly, but [from a weights of Restricted Boltzman Machine](https://www.semanticscholar.org/paper/To-recognize-shapes%2C-first-learn-to-generate-Hinton/51ff037291582df4c205d4a9cbe6e7dcec8f5973). Since then people tried to mix it with basically any paradigm we've listed previously, and GANs are no exception. For instance, [authors of BigBiGAN](https://arxiv.org/abs/1907.02544) proposed to train (guess what) BigGAN model jointly with an encoder, and learn discriminator to distinguish joint distributions of image-latent pairs, where latents for reals are given by encoder (see fig. 1 in their article).

But in practice all those approaches significantly underperform the alternatives, especially with the recent rise of [self-supervised learning](https://ai.facebook.com/blog/self-supervised-learning-the-dark-matter-of-intelligence). Why?

The authors of "Generative Hierarchical Features from Synthesizing Images" (namely Xu Yinghao, Shen Yujun and others) give a somewhat obvious answer - because we evaluate them mostly on problems that are "too discriminative" (I'll elaborate on that later). 

Namely, they take an existing pre-trained [StyleGAN](https://arxiv.org/abs/1812.04948) model and train a separate resnet-like encoder to predict its adaptive Instance-Norm statistics:

![GH-Feat scheme](/images/ghfeat_framework.jpg)

This scheme relies heavily on the fact how StyleGAN works - it samples random latent $z$, passes it through a deep fully-connected "mapping" network to get the feature vector $w$, then uses it at every layer of "synthesis" network (which receives a constant input) to perform an [adaptive version of instance normalization](https://arxiv.org/abs/1703.06868v2).
The statistics for such normalization are linear projections of $w$, and they are precisely what encoder tries to predict.

So in essence, the authors freeze the generator, replace the mapping part with encoder, and train the whole system on real images as a large adversarial autoencoder (using L2 and VGG perceptual distances, as well as a separate discriminator).

Then they evaluate encoder features. For Imagenet classification, it performs quite bad - the table below shows top-1 accuracy of linear model trained on the features:

![GHFeat on Imagenet](/images/ghfeat_imagenet.png)

Note that current Self-supervised SOTA [SwAV](https://proceedings.neurips.cc//paper/2020/hash/70feb62b69f16e0238f741fab228fec2-Abstract.html) reaches even higher accuracy of $75.3\%$ with the same ResNet-50 architecture, so the gap is actually twice bigger.

However, things change when we switch to keypoints prediction task:

![GHFeat on MAFL Keypoints](/images/ghfeat_mafl_scores.png)

Or the problem of layout prediction (when we need to outline basic separation lines betwen walls, floor and ceiling):

![MoCo vs GHFeat on layout prediction](/images/ghfeat_vs_moco_layout.png)

Here the top and bottom rows correspond to the predictions made by MoCo and GHFeat respectively with roughly similar encoder architectures.

Let me drop some intuitive pseudo-scientific explanation about this. To generate diverse and realistically-looking images the AdaIN statistics $y_s$ should contain as much information about the output as possible. Those will include many finest details like the texture of grass in outdoor images or the exact position of different objects in scene. In contrast, the features extracted from discriminative models (including those trained with Self-Supervision) are expected to have only features that correlate well with the class or instance labels and thus drop unrelated information about the image. Since the representation capacity of any model is limited, we're either left with a model that highly specialized for more abstract problems but fails on some low-level tasks or a model that could be used in a variety of low-level tasks but has less useful features to solve abstract problems.

Overall, the takeway is - if you need to solve a given downstream task, **a network with better classification accuracy does not necessarily perform better on it**. In cases when classical augmentations (like color jittering or random cropping) could make it harder to solve, it might be reasonable to replace your favorite Self-supervised feature extractor with GHFeat.

# Image rendering with GANs

As the previous case sounded quite obvious, here is something that does not - we could use GANs to get 3D object shape from a single image.

To get an intuition of how this works, let us firstly consider the problem of 3D estimation from a multi-view data. That's it - 

# Synthetic datasets and labels

WIP, stay tuned

