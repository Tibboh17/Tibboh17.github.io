---
title: AI Basic 1. What is Neural Network?
categories: [AI, Basic]
tags: [AI, Basic, Neural Network, Perceptron, Activation Function, Forward Propagation]
math: true
---

# 개요

# Neural Network

인간의 뇌는 **뉴런(신경 세포)**이라는 작은 단위로 구성되어 있습니다. 뉴런은 서로 연결되어 전기적 신호를 주고받으며, 학습과 의사결정을 가능하게 합니다. 이러한 뉴런은 정보를 받아들여 처리한 후, 다음 뉴런으로 전달하고 이들 사이의 연결을 **시냅스**라고 부릅니다.

인공 신경망도 이와 유사한 구조를 가집니다. 하지만 뉴런 대신 **노드(Node)**가 사용되고, 시냅스 역할을 하는 것은 **가중치(Weight)**라는 숫자 값입니다.

![Neural Network](./assets/img/AI-Basic/Neural_Network.png)
_Comparison of Biological Neuron and Artificial Neuron_

# Architecture of Artificial Neural Network

![Architecture of Artificial Neural Network](./assets/img/AI-Basic/Architecture_of_Artificial_Neural_Network.png){: width="884" height="881"}
_Explanation of the Basic Structure of Artificial Neural Network_

인공 신경망의 노드는 **입력(Input)**, **가중치(Weight)**, **편향(Bias)**, **활성화 함수(Activation Function)**, **출력(Output)**으로 구성됩니다. 이들의 역할은 다음과 같습니다.

- **입력(Input)**
    - 외부 데이터나 이전 층의 노드로부터 전달된 값입니다.
    - 입력값은 하나 이상의 값으로 구성됩니다.

- **가중치(Weight)**
- **편향(Bias)**
- **활성화 함수(Activation Function)**
- **출력(Output)**

그리고 이들이 각각 모여 입력층 **(Input Layer)**, **은닉층 (Hidden Layer)**, **출력층 (Output Layer)**을 이룹니다.