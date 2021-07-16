---
layout: post
title: Privacy-preserving generative deep neural networks support clinical data sharing
date: 2021-07-12 15:53:43
categories: 论文报告
tags: Deep-Learning  Privacy 
mathjax: true
---

## Abstract

在论文中，提出了一种确保生成对抗网络（GAN）框架生成器的（差分）隐私的方法。所得模型可用于生成合成数据，可在该合成数据上训练和验证算法，并在不影响原始数据集隐私的情况下进行算法性能对比。该方法修改了Private Aggregation of Teacher Ensembles (PATE)框架，并将其应用于GAN。修改后的框架（称为PATE-GAN）能够紧密限制任何单个样本对模型的影响，形成严格的差分隐私保证，因此与具有相同保证的模型相比，性能得到了改善。










## Introduction

本文的贡献可以总结如下：（1）修改PATE框架并将其应用于GANs生成合成数据，（2）在实验部分展示，使用各种真实数据集，当使用PATE执行差分隐私，结果比DPGAN获得更高质量的合成数据，（3）提出一种新的新度量来评估生成的合成数据。

## Related works

之前与本文所做的最相关的工作是DPGAN

DPGAN提出了一个修改GAN框架为差分隐私的框架，同时也依赖于后处理定理来改变学习差分隐私生成器的问题来学习差分隐私判别器；他们的工作关键的思想是在训练过程中在判别器的梯度中加入噪声，来创造差分隐私保证。

## Background

- Differential privacy
  - Neighboring Datasets
  - Differential Privacy
  - Post-processing
- Private Aggregation of Teacher Ensembles (PATE)

## Propose method: PATE-GAN

该方法建立在GAN和PATE框架之上。用PATE机制替换GAN判别器，这样的判别器是满足差分隐私的，但需要（可微的）学生模型使得能够反向传播到生成器。在训练之前，我们将数据集划分成$k$个子集，$D_1,...,D_k$，对 $$\forall i$$ ,$$\left|D_i\right|=|D|/k$$

- ### Generator

  生成器G与标准的GAN框架中一样。形式上它是一个函数$G\left(\cdot ; \theta_{G}\right):[0,1] \rightarrow U$，含有$\theta_{G}$参数，以随机噪声$z \sim \operatorname{Unif}([0,1])$作为输入，并输出向量$U=X\times Y$。生成器将接受训练，以尽量减少其生成样本在学生判别器的损失。给定n个$i.i.d$样本$\operatorname{Unif}([0,1]), z_{1}, \ldots, z_{n}$,给定则关于参数的的经验误差定义为：

  $$L G\left(\theta_{G} ; S\right)=\sum_{j=1}^{n} \log \left(1-S\left(G\left(z_{j} ; \theta_{G}\right)\right)\right.$$

- ### Discriminator

  在标准的GAN框架中，有一个单一的判别器D，用和G直接对抗的方式训练，在每次迭代中，G都试图提升它相对于D的损失并且D也试图提升其相对于G的损失。该论文提出的模型，用PATE机制取代了D。这意味着引入了k个教师判别器$T^1,...,T^k$和一个学生判别器$S$，一个明显的差别是，对抗性的训练不再是对称的：教师现在正在训练来减少他们相对$G$的损失，但是$G$则训练以减少其对学生的损失，而学生则训练以减少其对教师的损失。

  #### 教师判别器

  

  #### 学生判别器

## Experiments

- ### Experimental settings

- ### Results with setting B

- ### Varying the privacy constrain

- ### Quantitative analysis on the number of teachers

