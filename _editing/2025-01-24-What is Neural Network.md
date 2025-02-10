---
title: AI Basic 1. What is Neural Network?
categories: [AI, Basic]
tags: [AI, Basic, Neural Network, Perceptron, Activation Function, Forward Propagation]
math: true
---

# 개요

지난 기간 동안 갑작스럽게 파견을 다녀온 뒤, 복귀하자마자 새로운 업무를 시작하면서 블로그에 포스트를 올리지 못한 시간이 길어졌습니다. 이제 회사 업무에 어느 정도 적응했기에, 블로그와 공부를 다시 정상화하는 오늘을 기점으로 새해 첫 글을 올리며 시작해 보려 합니다.

인공 신경망(Artificial Neural Network, ANN)은 인간의 뇌 구조와 학습 과정을 모방하여 만들어진 기계 학습 모델로, 데이터 분석, 패턴 인식, 예측 등 다양한 문제를 해결하는 데 활용됩니다. 이에 대한 주요 개념은 인간의 뇌를 구성하는 뉴런(Neuron)의 동작 방식에서 영감을 받았으며, 이를 수학적 모델로 구현하여 데이터 처리와 학습을 가능하게 합니다.

이번 포스트에서는 ANN의 기본 구조와 작동 원리를 설명하며, 뉴런의 계산 과정 및 이를 기반으로 하는 신경망의 동작 방식을 다뤄보겠습니다.

# Neural Network

인간의 뇌는 **뉴런(Neuron)**이라는 작은 단위로 구성되어 있습니다. 뉴런은 서로 연결되어 전기적 신호를 주고받으며, 학습과 의사결정을 가능하게 합니다. 이러한 뉴런은 정보를 받아들여 처리한 후, 다음 뉴런으로 전달하고 이들 사이의 연결을 **시냅스(Synapse)**라고 부릅니다.

인공 신경망도 이와 유사한 구조를 가집니다. 하지만 뉴런 대신 **노드(Node)**가 사용되고, 시냅스 역할을 하는 것은 **가중치(Weight)**라는 숫자 값입니다.

![Neural Network](./assets/img/AI-Basic/Neural_Network.png){: width="500"}
_Comparison of Biological Neuron and Artificial Neuron_

# Architecture of Artificial Neural Network

![Architecture of Artificial Neural Network](./assets/img/AI-Basic/Architecture_of_Artificial_Neural_Network.png){: width="500"}
_Explanation of the Basic Structure of Artificial Neural Network_

인공 신경망의 노드는 **입력(Input)**, **가중치(Weight)**, **편향(Bias)**, **활성화 함수(Activation Function)**, **출력(Output)**으로 구성됩니다. 이들의 역할은 다음과 같습니다.

- **입력(Input)**
    - 외부 데이터나 이전 층의 노드로부터 전달된 값입니다.
    - 입력값은 하나 이상의 값으로 구성됩니다.

- **가중치(Weight)**
    - 각 입력값의 중요도를 조정합니다.
    - 입력값마다 가중치가 곱해지며, 이 값은 학습 과정에서 조정됩니다.

- **편향(Bias)**
    - 뉴런의 활성화 기준을 조정하는 상수입니다.
    - 입력값과 가중치의 합에 더해지며, 뉴런이 출력값을 생성하는 데 필요한 최소 조건을 설정합니다.

- **활성화 함수(Activation Function)**
    - 가중합을 비선형적으로 변환하여 뉴런의 최종 출력값을 생성합니다.
    - 활성화 함수는 신경망이 비선형 문제를 해결할 수 있도록 돕습니다.

- **출력(Output)**
    - 활성화 함수의 결과값을 다음 층의 인공 뉴런으로 전달합니다.
    - 출력값은 연속적이거나 이산적인 값으로 표현됩니다.

그리고 이들이 각각 모여 입력층 **(Input Layer)**, **은닉층 (Hidden Layer)**, **출력층 (Output Layer)**을 이룹니다.

# Process of Calculation in Artificial Neuron

앞서 설명한 구성 요소를 활용하여 인공 뉴런은 입력값으로부터 출력값을 계산합니다. 이 계산의 구체적인 과정은 다음과 같습니다.

- **Collecting Input Values**
    - 입력값이 노드로 들어옵니다.

$$
\mathbf{x} = [x_1, x_2, \dots, x_n]
$$

- **Applying Weight**
    - 입력값에 가중치를 곱한 뒤, 이를 합산하고 편향을 더해 가중합을 계산합니다.

$$
\mathbf{w} = [w_1, w_2, \dots, w_n]
z = \sum_{i=1}^n (w_i \cdot x_i) + b \quad (b: \text{Bias})
$$

- **Applying Activation Function**
    - 가중합에 활성화 함수를 적용하여 출력값을 생성합니다.

$$
\text{Output} = f\left(z = \sum_{i=1}^n (w_i \cdot x_i) + b\right) \quad (f: \text{Activation Function})
$$

- **Forwarding Output**
    - 출력값을 다음 층의 노드로 전달됩니다.