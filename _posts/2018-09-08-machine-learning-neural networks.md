---
layout: post
title: 人工神经网络
category: blog
tags: [深度学习]
description: 浅谈。
---

前言：人工神经网络（Artificial Neural Network，即ANN），是20世纪80年代以来人工智能领域兴起的研究热点。它从信息处理角度对人脑神经元网络进行抽象， 建立某种简单模型，按不同的连接方式组成不同的网络。在工程与学术界也常直接简称为神经网络或类神经网络。神经网络是一种运算模型，由大量的节点（或称神经元）之间相互联接构成。每个节点代表一种特定的输出函数，称为激励函数（activation function）。每两个节点间的连接都代表一个对于通过该连接信号的加权值，称之为权重，这相当于人工神经网络的记忆。网络的输出则依网络的连接方式，权重值和激励函数的不同而不同。而网络自身通常都是对自然界某种算法或者函数的逼近，也可能是对一种逻辑策略的表达。 
最近十多年来，人工神经网络的研究工作不断深入，已经取得了很大的进展，其在模式识别、智能机器人、自动控制、预测估计、生物、医学、经济等领域已成功地解决了许多现代计算机难以解决的实际问题，表现出了良好的智能特性。 

## 神经元模型

MP神经元模型是1943年，由WarrenMcCulloch和WalterPitts提出的。在这个模型中，神经元接收到来自n个其他神经元传递过来的输入信号，这些输入信号通过带权重的连接进行传递，神经元接收到的总输入值将与神经元的阈值进行比较，然后通过“激活函数”处理以产生神经元的输出。如图所示：

![](1)

注意：理想中的激活函数是阶跃函数，但是由于阶跃函数具有不光滑、不连续等性质，所以经常采用Sigmoid函数作为激活函数，Sigmoid函数也被称为挤压函数。

把许多个这样的神经元模型按照一定的层次连接起来，就得到了神经网络。 

## 感知机和多层网络 

感知机由两层神经元组成，输入层接受外界输入信号后传递给输出层，输出层是M-P神经元，亦称“阈值逻辑单元”。如图所示：

除此之外，注意到$y=f\left(\mathbf{x}^T\mathbf{w}-\theta\right)=f\left(\sum_iw_ix_i-\theta\right)$，阈值θ可以看做固定输入为-1.0的”哑结点”其对应的权重为wn+1,这样权重和阈值的学习可以统一为权重学习。 

为得到可接受的权向量，我们会从随机的权值开始，反复地应用这个感知机到每个训练样例，只要它误分类样例就修改感知机的权值。重复这个过程，直到感知机正确分类所有的样例。每一步根据感知机训练法则(perceptron training rule)来修改权值，也就是修改与输入 xi 对应的权 wi，法则如下：

![](2)

wi←wi+Δwi
Δwi=η(t−o)xi  

这里 t 是当前训练样例的目标输出，o是感知机的输出，η是一个正的常数称为学习速率(learning rate)。学习速率的作用是缓和每一步调整权的程度，它通常被设为一个小的数值（例如 0.1），而且有时会使其随着权调整次数的增加而衰减。  

需要注意的是，感知机只有输出层神经元进行激活函数处理，即只拥有一层功能神经元，其学习能力非常有限。可以证明，若两类模式是线性可分的，即存在一个线性超平面能将他们分开，即感知机的学习过程一定会收敛。