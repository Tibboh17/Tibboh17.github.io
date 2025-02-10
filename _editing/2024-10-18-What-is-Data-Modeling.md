---
title: What is Data Modeling?
categories: [Database, Fundamentals of Database]
tags: [Database, Data, Modeling]
---

# 개요

*ERD*와 *Normalization*을 학습하면서, `Data Modeling`에 필수적인 핵심 개념들을 하나씩 살펴보았습니다. `Data Modeling`은 *Database*를 설계할 때 반드시 거쳐야 하는 중요한 과정으로, 이를 통해 Data가 어떻게 구조화되고 효율적으로 관리될 수 있는지를 결정하게 됩니다.

이번 포스트에서는 `Data Modeling`이 무엇인지에 대해 알아보려 합니다. 또한, *ERD*와 *Normalization*이라는 주요 개념이 `Data Modeling`에서 어떻게 사용되는지, 실제로 *Data*를 구조화하는 과정에서 어떤 단계를 거치는지에 대해서도 살펴보겠습니다.

# Data Modeling

`Data Modeling`은 *Data*를 체계적으로 저장하고 관리할 수 있도록 설계하는 과정입니다. 대부분의 업무에서는 방대한 양의 *Data*를 다루기 때문에 이를 효과적으로 처리하기 위해서 *Database*가 필요합니다. 그리고 `Data Modeling`은 이러한 *Database*의 구조를 설계하고, Data의 흐름을 정의하는 작업이라고 할 수 있습니다.

`Data Modeling`의 단계는 크게 세가지로 나뉘게 됩니다. `Conceptual Data Modeling`, `Logical Data Modeling`, 그리고 `Physical Data Modeling`입니다. 이에 대한 자세한 설명은 이제부터 다음 목차에서 다루겠습니다.

# Conceptual Data Modeling

`Data Modeling`의 출발점으로, 조직 내에서 사용되는 다양항 *Data*와 그 흐름을 식별하는 과정입니다. 이 단계에서는 *Data*의 전반적인 구조와 내용을 정의하지만, 세부적인 계획까지는 다루지 않습니다.

정리하자면 조직 전체에서 사용되는 *Data*가 어떠한 것들이 있으며, 그들이 어떻게 서로 연결되고 흐르는지를 높은 수준에서 파악하는 단계라고 할 수 있습니다.

# Logical Data Modeling

한 단계 더 나아가, *Data* 흐름과 *Database*의 내용을 보다 구체적으로 정의하는 과정입니다. 일반적으로 우리가 `Data Model`이라고 부르는 것이 `Logical Data Modeling`을 의미합니다. 이 단계에서는 *Data*의 구조에 대한 세부 정보를 추가하지만, *Database* 자체에 대한 구체적인 스펙은 포함하지 않습니다.

즉, `Logical Data Modeling`은 *Database*의 실제 설계를 위한 기초를 마련하는 단계로, *Data*를 어떻게 나누고 연결할지에 대한 논리적인 틀을 제공합니다.

# Physical Data Modeling

`Logical Data Modeling`을 실제로 구현하는 단계입니다. 이 과정에서는 특정 *DBMS*에 맞춰 이를 구체화하며, *DBMS*의 용어와 기술을 사용해 *Database*를 설계합니다.

이 단계에서 비로소 *Database*가 구현될 시스템과 맞물려 구체적인 *Table*, *Column*, *Index* 등이 정의됩니다.