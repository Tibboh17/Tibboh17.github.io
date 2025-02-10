---
title: What is Normalization?
categories: [Database, Fundamentals of Database]
tags: [Database, Data, Normalization, Modeling]
---

# 개요

*Database*에서 효율적이고 일관성 있는 *Data*를 관리하기 위해서는 `Normalization`이 필수적입니다. 이를 우리는 **정규화**라고 하며 *Database*에서 발생하는 이상 현상을 방지하는 과정으로 매우 중요하다고 합니다.

이번 포스트에서는 `Normalization`의 기본 개념과 이와 연결된 다양한 개념들을 다루어 보겠습니다. 또한, 이를 활용한 구체적인 예시를 통해 `Normalization`이 어떻게 적용되는지에 대해 알아보겠습니다.

# Normalization

`Normalization`은 *Database*에서 Data 중복을 둘이고 무결성을 향상 시키기위한 과정입니다. 이 과정은 *Data*가 논리적이고 효율적으로 관리될 수 있도록 *Table*을 재구조화하는 단계입니다.

`Normalization`의 목적은 다음과 같습니다.

- *Data Redundancy* 또는 *Data Duplication*을 줄입니다.
- 성능을 저하시키지 않으면서 *Database*를 더 확장 가능하게 만듭니다.
- *Integrity Constraints*의 적용을 간소화합니다.
- *Table*을 더 논리적이고 직관적으로 만들어 발생할 수 있는 이상 현상을 방지합니다.

*Integrity*와 *Constraints*에 대해서는 추후에 다른 포스트에서 더 자세히 다루겠습니다.

# Anomalies

`Normalization`을 하지 않았을 때, 발생할 수 있는 이상 현상에 대한 구체적인 내용은 다음과 같습니다.

- **Insertion Anomaly**
    - 필수적인 *Field*가 없거나 *Data*가 불완전하여 *Database*에 *Data*를 삽입할 수 없을 때 발생합니다.
- **Update Anomaly**
    - *Database*에서의 *Data*의 수정으로 인해 일관성 문제나 오류가 생길 경우 발생합니다.
- **Deletion Anomaly**
    - *Record*를 삭제하면서 의도치 않은 *Data* 손실이 있을 때 발생합니다.

# Funtional Dependency

`Funtional Dependency`는 *Table* 내 속성 간의 관계를 설명하는 기본 개념입니다. 기호로는 `X → Y`로 표현되며 이는 속성 `X`가 속성 `Y`를 함수적으로 결정한다는 의미입니다. 이때, `X`는 `Determinant`라고 하고, `Y`는 `Dependent`라고 부릅니다. `Funtional Dependency`의 유형은 다음과 같습니다.

- **Fully Functional Dependency**
    - `Y`가 `X`의 전체 집합에 의해 완전히 결정될 때 발생합니다.

- **Partial Dependency**
    - `Y`가 `X`의 일부 속성에만 종속될 때 발생합니다. `2NF`에서 제거해야 하는 종속성입니다.

- **Transitive Dependency**
    - `X → Y`이고 `Y → Z`일 때, `X → Z`가 성립하는 경우입니다. 3NF에서 제거해야 하는 종속성입니다.

이러한 종속성은 **Armstrong’s Axioms**이라 불리는 몇가지 규칙에 따라 추론할 수 있습니다. 이를 통해  종속성을 분석하여 불필요한 종속성을 제거하고 `Table`을 분리하여 `Noramliztion`을 진행합니다. 규칙들은 다음과 같습니다.

- **Reflexive Rule**
    - `X ⊇ Y`이면, `X → Y`이다.
- **Augmentation Rule**
    - `X → Y`이면, `XZ → YZ`이다.
- **Transitive Rule**
    - `X → Y`, `Y → Z`이면, `X → Z`이다.    
- **Union Rule**
    - `X → Y`, `X → Z`이면, `X → YZ`이다.
- **Decomposotion Rule**
    - `X → YZ`이면, `X → Y`, `X → Z`이다.
- **Pseudo Transitive Rule**
    - `X → Y`, `YZ → W`이면, `XZ → W`이다.

# Data Normalization Forms

`Normalization`을 진행할 때, 단계별로 특정한 형태를 만족하도록 합니다. 일반적으로 `First Normal Form`부터 `Third Normal Form`까지 적용하며, 필요에 따라 더 높은 단계까지 갈 수 있습니다.

- **First Normal Form (1NF)**
    - 중복된 값이나 반복되는 *Data*를 제거하고, 각 속성에 하나의 값만을 저장하도록 *Table*을 설계합니다.
- **Second Normal Form (2NF)**
    - 하나의 속성이 *Primary Key* 전체에 종속되지 않고 부분적으로만 종속된 경우 해당 속성을 분리해 새로운 *Table*을 만듭니다.
- **Third Normal Form (3NF)**
    - *Primary Key*에 의존하지 않는 속성에 의해 결정되는 다른 속성을 제거하고, 이를 별도의 *Table*로 분리합니다.

기타 단계는 위 내용보다 중요성이 떨어지니 참고하는 정도로만 보시기 바랍니다.

- **Boyce-Codd Normal Form (BCNF)**
    - `Determinant`가 *Candidate Key*가 아닌 경우를 제거하여 모든 `Determinant`가 *Candidate Key*가 되도록 *Table*을 재구조화합니다. *Candidate Key*는 *Table*에서 *Primary Key*로 사용할 수 있는 속성 또는 속성들의 집합을 의미합니다.
- **Fourth Normal Form (4NF)**
    - *Table* 내에 여러 값이 독립적으로 저장되는 상황을 제거합니다. 각 속성이 고유한 값을 가지도록 *Table*을 분리합니다.
- **Fifth Normal Form (5NF)**
    - *Table*을 분리함으로써 여러 관계가 중복되지 않도록 합니다.
- **Sixth Normal Form (6NF)**
    - 중복을 완전히 제거한 상태를 의미합니다.

# Normalization 적용 예시

지금까지 설명된 내용들을 바탕으로 예시를 통해 `Normalization`이 실제로 어떻게 진행되는지 알아보며 글을 마치겠습니다.

- `Normalization`이 되지 않은 *Table*
    - 현재 *Table*은 지나치게 많은 값을 포함하여 매우 비효율적이고 이상 현상을 초래할 수 있습니다. 

| **Student#** | **Advisor** | **Adv-Room** | **Class1** | **Class2** | **Class3** |
|:------------:|:-----------:|:------------:|:----------:|:----------:|:----------:|
|     1022     |    Jones    |      412     |   101-07   |   143-01   |   159-02   |
|     4123     |    Smith    |      216     |   101-07   |   143-01   |   179-04   |

- **First Normal Form (1NF)**
    - 각 수업 정보를 별도의 행으로 분리하여 중복을 제거하고 원자 값으로 변환하였습니다. 하지만 여전히 반복적으로 저장되는 *Data*가 있어, 문제가 완전히 해결된 것은 아닙니다.

| **Student#** | **Advisor** | **Adv-Room** | **Class#** |
|:------------:|:-----------:|:------------:|:----------:|
|     1022     |    Jones    |      412     |   101-07   |
|     1022     |    Jones    |      412     |   143-01   |
|     1022     |    Jones    |      412     |   159-02   |
|     4123     |    Smith    |      216     |   101-07   |
|     4123     |    Smith    |      216     |   143-01   |
|     4123     |    Smith    |      216     |   179-04   |

- **Second Normal Form**
    - `Partial Dependency`을 제거하기 위해 학생과 수업 간의 관계는 별도로 처리하고, 학생과 교수의 정보는 분리된 *Table*에 저장합니다.

| **Student#** | **Advisor** | **Adv-Room** |
|:------------:|:-----------:|:------------:|
|     1022     |    Jones    |      412     |
|     4123     |    Smith    |      216     |

| **Student#** | **Class#** |
|:------------:|:----------:|
|     1022     |   101-07   |
|     1022     |   143-01   |
|     1022     |   159-02   |
|     4123     |   101-07   |
|     4123     |   143-01   |
|     4123     |   179-04   |

- **Third Normal Form**
      - `Transitive Dependency`를 제거하기 위해 교수의 이름과 방 번호는 학번과 직접적으로 관련이 없으므로, 이를 별도의 *Table*로 분리합니다.

| **Student#** | **Advisor** |
|:------------:|:-----------:|
|     1022     |    Jones    |
|     4123     |    Smith    |

| **Student#** | **Class#** |
|:------------:|:----------:|
|     1022     |   101-07   |
|     1022     |   143-01   |
|     1022     |   159-02   |
|     4123     |   101-07   |
|     4123     |   143-01   |
|     4123     |   179-04   |

| **Name** | **Room** | **Dept** |
|:--------:|:--------:|:--------:|
|   Jones  |    412   |    42    |
|   Smith  |    216   |    42    |