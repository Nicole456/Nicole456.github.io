---
layout: post
title: Scalable private learning with pate
date: 2021-07-14 15:53:43
categories: 论文报告
tags: Deep-Learning  Privacy  PATE
mathjax: true
---
## Abstract

- 到目前为止，PATE只被评估于像MNIST这样简单的分类任务上，这使得它在应用于更大规模的学习任务和真实数据集时的效用还不清楚。
- 在这项工作中展示了PATE如何扩展到具有大量输出类和未处理的、不平衡的错误训练数据的学习任务。为此引入了新的噪声聚合机制，它们更有选择性，添加更少的噪声，并证明它们更严格的差分隐私保证。
- 新机制建立在两个见解之上：使用更集中的噪音增加了教师达成一致的机会，如果缺乏共识，不需要给学生答案。使用的共识答案更有可能是正确的，提供更好直观的隐私，并产生较低的隐私成本。
- 评估显示，新机制在所有方法上都改进了原始的PATE，并能够以高效用和非常强的隐私的规模扩展到更大的任务（ε<1.0）。










## Introduction

Private Aggregation of Teacher Ensembles, PATE的优势是能够从对不同数据训练的独立“教师”模型的聚合共识中学习，既提供直观的隐私保障，又能够不知道底层的机器学习技术。然而，到目前为止（这篇文章之前），PATE只应用于简单的任务，比如MNIST，而没有任何现实的、更大规模的评估。

本论文提出的技术允许PATE应用在更大的规模上建立更准确的模型，由于教师的独立共识及其差分隐私保障，提高了PATE的直观隐私保护。如实验所示，结果是隐私、实用性和实用性的提高——这是一个不寻常的联合改进。

本文的主要技术贡献是聚合教师答案的新机制，它们更有选择性和增加更少的噪音。为了更有选择性，新机制利用了在PATE聚合中隐私和效用之间的一些好的协同效应。为了增加更小的噪声，新PATE聚合机制是高斯噪声的样本，因为该分布的尾部比在原始路径工作中使用的拉普拉斯噪声减小得要快得多。

## Related work

**差分隐私**现在是隐私的黄金标准。它提供了一个严格的框架，其模型对对手的能力很少做出假设，允许差分隐私算法有效地应对强大的对手。

......

## Background and Overview

1. ### The PATE Framework

   PATE框架含有三个关键部分: (1) n个teacher models的集成 (2) 一个聚合机制 (3) 一个 student model.

   - Teacher models

   - Aggregation mechanism

   - Student model

     >每一个额外的集成预测都会增加隐私成本，因此不能用于**无界查询**。固定查询固定了隐私成本，并降低了分析模型参数以恢复训练数据的攻击的价值

2. ### Differential Privacy 

   差分隐私要求限制算法输出分布对其输入的小扰动的敏感性。

   Definition 1        $$\operatorname{Pr}[\mathcal{M}(D) \in S] \leq e^{\varepsilon} \cdot \operatorname{Pr}\left[\mathcal{M}\left(D^{\prime}\right) \in S\right]+\delta$$

3. ###  RÉNYI DIFFERENTIAL PRIVACY

   限制PATE隐私损失方法，通过限制查询的每个标签的隐私成本并使用强大的组合定理得出总成本，产生的是松散的隐私保障。因此使用了**与数据相关的隐私分析**。

   Definition 2       

   *Rényi divergence* *of order* *λ* *between two distributions* *P* and *Q* *is defifined as:*

   $$D_{\lambda}(P \| Q) \triangleq \frac{1}{\lambda-1} \log \mathbb{E}_{x \sim Q}\left[(P(x) / Q(x))^{\lambda}\right]=\frac{1}{\lambda-1} \log \mathbb{E}_{x \sim P}\left[(P(x) / Q(x))^{\lambda-1}\right]$$

   Definition 3       

   （略）

## Improved aggregation mechanism for PATE

- PATE提供的隐私保障源于对聚合步骤的设计和分析。
- 本节介绍改进的PATE聚合机制：首先用高斯噪声代替了教师投票中添加的拉普拉斯噪声，从而适应了与数据相关的隐私分析；接下来，我们描述Confident和Interactive聚合器，它们以保护隐私的方式选择值得回答的查询：隐私预算在查询选择和答案计算之间共享。聚合器使用不同的启发式方法来选择查询：之前的策略不考虑学生的预测，而现在的则考虑学生预测。

1. ### The GNMax Aggregator and Its Privacy Guarantee

   a Gaussian NoisyMax (GNMax)

   $$\mathcal{M}_{\sigma}(x) \triangleq \underset{i}{\operatorname{argmax}}\left\{n_{i}(x)+\mathcal{N}\left(0, \sigma^{2}\right)\right\}$$

   $\mathcal{N}\left(0, \sigma^{2}\right)$为均值为0和方差为$\sigma^{2}$的高斯分布。聚合器为每个投票计数加上高斯噪声后，输出含有噪声的占多数的类。接下来，占多数的类更普遍地指的是在类中获得最多的教师投票的类。

   高斯分布比之前所使用的拉普拉斯分布更集中，当类的数量m很大时，这个集中直接提高了聚合的效用。 对于任意输入以及$\lambda>1$ GNMax 机制满足$\left(\lambda, \lambda / \sigma^{2}\right)-\mathrm{RDP}$。

   同时组合定理的直接应用导致了松散的隐私界限。为了改进这些方法，进行了一个仔细的数据依赖性分析

   （略）

2. ### The Confident-GNMax Aggregator

   在本节中提出了对GNMax聚合器的改进，能够过滤掉教师没有足够强烈共识的查询。这个过滤使教师能够避免回答有较高隐私成本的查询。为了选择具有压倒性一致的查询，算法检查多个投票是否超过阈值*T*。为了加强本步骤中的隐私性，在加入具有方差的高斯噪声$\sigma^{2}_1$后进行了比较。然后，对于通过噪声阈值检查的查询，聚合器继续使用方差$\sigma^{2}_2$较小的常规GNMax机制。对于没有通过噪声阈值检查的查询，聚合器只需返回⊥，而学生在其训练中放弃了这个样本。

   ![image-20210714112640346](..\..\images\Scalable private learning with pate\01.png)

   在实践中，我们通常选择更高的$\sigma_1$值相对于$\sigma_2$。这是因为我们总是付出噪声阈值检查的成本，而未意识到共识是强大的好处。

   这个聚合器的隐私成本是直观的：为每个查询的阈值检查付出隐私成本，并且仅为通过检查的查询进行GNMax步骤。在之前的工作中，该聚合机制为每个查询付出了隐私成本，无论是昂贵与否。相比之下，confident聚合器根据阈值花费了小得多的隐私成本

3. ### The Interactive-GNMax Aggregator

   虽然confident的聚合器不需要隐私成本高的查询，但它忽略了学生可能收到的对学习贡献很少而反过来又对它的效用有贡献的标签。根据学生目前对其公共训练数据的预测，设计了一个交互式聚合器，它可以丢弃学生已经自信地预测与教师相同的标签的查询。

   给定一组查询，交互式聚合器（算法2）通过比较学生的预测与每类的教师投票来选择所回答的查询。

   该方法首先以一种保护隐私的方式将学生的预测与教师的投票进行比较，然后(a)加强学生对给定查询的预测，或(b)为学生提供一个由教师预测的新标签。  

   ![image-20210714112640346](..\..\images\Scalable private learning with pate\02.png)

   在实践中，confident聚合器可以用来在学生无法做出有意义的预测时开始训练学生，在学生熟练掌握后可以用Interactive聚合器完成训练。

## Experimental evaluation

1. ### Experimental Setup

   - MNIST, SVHN, and the UCI Adult databases.

     比较分析与我们的confident-GNMax聚合器和最早在PATE中使用的LNMax所实现的效用-隐私权衡。

   - Glyph

     这个光学字符识别任务比以前所有的PATE应用多一个数量级。Glyph数据集也具有许多真实任务共享的特征：例如，它不平衡，一些输入被错误标记。任务是将输入分类为用于生成它们的150个Unicode符号之一。

2. ### Comparing the LNMax and GNMax Mechanisms

   对于尾部衰减比拉普拉斯分布更快的高斯分布，我们期望在使用新机制时有更好的效用（尽管涉及更多隐私）
   分析）。

3. ### Student Training with the GNMax Aggregation Mechanisms

4. ### Noisy Threshold Checks and Privacy Costs

## Conclusions