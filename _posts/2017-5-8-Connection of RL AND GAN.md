---
layout: post
title:  Connection of RL AND GAN
desc: 我的博客
keywords: 'blog,Machine Learning,AI'
date: 2017-5-8T00:00:00.000Z
categories:
  - Machine Learning
tags:
  - Machine Learning
  - AI
icon: fa-book
---


## 目录
**欢迎在文章下方评论，建议用电脑看**

* 目录
{:toc}

# Connection of RL AND GAN

## Connecting generative adversarial network and actor-critic methods

[原文](https://arxiv.org/pdf/1610.01945.pdf)

### 概述

Actor-critic methods  : 许多RL方法 (e.g., policy gradient) 只作用于policy 或者 value function。Actor-critic方法则结合了policy-only和value function-only 的方法。 其中critic用来近似或者估计value function，actor 被称为policy structure, 主要用来选择action。Actor-critic是一个on-policy的学习过程。Critic模型的结果用来帮助提高actor policy的性能。

GAN和actor-critic具有许多相似之处。Actor-critic模型中的actor功能类似于GAN中的generator， 他们都是用来take an action or generate a sample。Actor-critic模型中的critic则类似于GAN中的discriminator, 主要用来评估 actor or generator 的输出。这篇论文主要贡献在于从不同的角度来说明了GAN和actor－critic模型的相同与不同点，从而鼓励研究GAN的学者和研究actor-critic模型的学者合作研发出通用、稳定、可扩展的算法，或者从各自的研究中获取灵感。


<img src="{{ site.img_path }}/Machine Learning/Connection_RL_AND_GAN.jpg" alt="header1" style="height:auto!important;width:auto%;max-width:1020px;"/>


## bilevel optimization problems

Both GANs and AC can be seen as bilevel or two-time-scale optimization problems, where one model is optimized with respect to the optimum of another model:

<img src="{{ site.img_path }}/Machine Learning/Connection_RL_AND_GAN1.jpg" alt="header1" style="height:auto!important;width:auto%;max-width:1020px;"/>

他们的共同思想都是互相优化，其中一个模型的优化关于另个的最优。

In some AC methods, the critic provides a lower-variance baseline for policy gradient methods than estimating the value from returns. In this case even a bad estimate of the value function can be useful, as the policy gradient will be unbiased no matter what baseline is used. In other AC methods, the policy is updated with respect to the approximate value function, in which case pathologies similar to those in GANs can result. If the policy is optimized with respect to an incorrect value function, it may lead to a bad policy which never fully explores the space, preventing a good value function from being found and leading to degenerate solutions.

有一些AC方法会可以给policy gradient methods提供比评估价值函数更小的基准方差的价值回报，这个时候就算是不好的评估也是有用的，因为。其他一些评估价值函数会直接影响policy gradient methods，这一类就类似于GANS。也就是不好的评估回报会导致不好的引导。

## GANs as a kind of Actor-Critic

GANs can be seen as a modified actor-critic method with blind actors in stateless MDPs.

<img src="{{ site.img_path }}/Machine Learning/Connection_RL_AND_GAN3.jpg" alt="header1" style="height:auto!important;width:auto%;max-width:1020px;"/>

<img src="{{ site.img_path }}/Machine Learning/Connection_RL_AND_GAN4.jpg" alt="header1" style="height:auto!important;width:auto%;max-width:1020px;"/>


GANS可以看作是状态无限，然后actors是blind的一种特殊的MDP。原因是：在这里动作集合就是在每个像素生成器所给的值，状态就是生成的图片，因为生成图片的是无关联的，也就是现在生成的图片和未来生成的图片无关，这就导致了状态的无限性。actors是blind是因为它不知道环境的任何状态知识。还有就是当给系统展示real image的时候，actor应该参数不变，*在GANS体现在达到均衡吧，这个时候生成器就不改变它的权重了（我觉得）*。在actor-critic中体现的就是reword为一是，参数不需要太大的改变。

当然他们之间存在不同点：不同点在于首先所用的loss函数不同，gradient的来源也不同，更新actor-critic更像是正交而不是对抗的（在最后一段），GANS对环境是部分观测的，具体看第二段和第三段。


## Stabilizing Strategies

Having reviewed the basics of GANs, actor-critic algorithms and their extensions, here we discuss the “tricks of the trade" used by each community to make them work. The different methods are summarized in Table 1. While not meant as an exhaustive list, we have included those that we believe have either made the largest impact in their fields or have the greatest potential for crossover between fields.

<img src="{{ site.img_path }}/Machine Learning/Connection_RL_AND_GAN2.jpg" alt="header1" style="height:auto!important;width:auto%;max-width:1020px;"/>

## An actor-critic algorithm for sequence prediction

[原文](https://arxiv.org/pdf/1607.07086.pdf)


The contributions of the paper can be summarized as follows: 1) we describe how RL methodology like the actor-critic approach can be applied to supervised learning problems with structured outputs; and 2) we investigate the performance and behavior of the new method on both a synthetic task and a real-world task of machine translation, demonstrating the improvements over maximum-likelihood and REINFORCE brought by the actor-critic training.

## 创新点

（1）提出**第一个使用强化学习例如actor-critic方法来训练神经网络生成序列。**（2）当前对数似然训练方法受限于他们的训练和测试之间的差异模式,即RNN在training时接受ground truth input，但testing时却接受自己之前的output，这两个setting不一致会导致error accumulate，这篇论文**解决这个问题，这个问题也叫做exposure bias问题。**这使得一个更接近训练阶段的test过程，允许直接优化为特定任务的评分，如BLEU。


















