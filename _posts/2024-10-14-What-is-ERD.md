---
title: What is DBMS?
categories: [Database, Fundamentals of Database]
tags: [Database, Data, ERD, Modeling]
---

# 개요

`ERD`는 `Entity-Relationship Diagram`의 약자로, *Database* 설계에서 가장 기본적이면서도 중요한 도구입니다. `ERD`는 현실 세계의 *Data* 구조를 시각적으로 표현하여, 어떻게 *Database*를 설계할지 개념적으로 보여주는 역할을 합니다.

특히, 복잡한 *Data* 구조를 직관적으로 이해할 수 있도록 돕는 `ERD`는 개발자와 비개발자 모두에게 효과적인 소통 도구로 사용됩니다. 이를 *Database* 구조를 더 명확하게 파악할 수 있으며, 설계 단계에서 발생할 수 있는 오류를 사전에 방지할 수 있습니다.

이번 글에서는 `ERD`의 구성 요소와 이를 활용한 *Database* 설계 방법에 대해 알아보겠습니다.

# ERD

`ERD`는 `Entity`, `Attribute`, `Relationship`이라는 세 가지 주요 구성 요소로 이루어집니다. 해당 목차에서는 간단한 설명을 하고 세부적인 설명은 다음 목차에서 정리하겠습니다.

- **Entity**
    - `Database`에서 관리해야 할 정보의 대상을 의미합니다. 사람, 사물, 개념 등 현실 세계에서 중요한 요소가 될 수 있으며, 보통 직사각형으로 표현됩니다.

- **Attribute**
    - 각 `Entity`가 가지고 있는 정보는 나타내고 **속성**이라 부릅니다. `Entity`의 특성을 설명하는 역할을 하며 타원형으로 표현되고 `Entity`와 연결됩니다.

- **Relationship**
    - `Entity` 간의 상호작용이나 연결을 나타내는 **관계**를 말하며 마름모로 표현됩니다.

# Entity

- **Entity Set**
    - 동일한 유형의 `Entity` 그룹을 나타냅니다. `Entity Set`에 포함된 `Entity`는 유사한 속성을 가지며, 그 속성의 값도 유사합니다. 
    - 예시로 고객 `Entity Set`은 여러 고객을 포함하며, 각 고객은 이름, 주소 등의 유사한 속성을 가집니다.

- **Strong Entity**
    - *Primary Key*를 가진 `Entity`로, 스스로 고유하게 식별될 수 있고 다른 `Entity`에 의존하지 않으며, 독립적으로 존재할 수 있습니다. `ERD`에서 단일 직사각형으로 표현됩니다.
    - 예시로 학생 `Entity`는 학생 ID라는 *Primary Key*를 가지므로 `Strong Entity`입니다.

- **Weak Entiry**
    - *Primary Key*가 없어, 다른 `Entity`와의 관계를 통해서만 식별될 수 있는 `Entity`입니다. `ERD`에서 이중 직사각형으로 표현됩니다.
    - 예시로 의료 기록은 환자의 ID를 기반으로만 식별되기 때문에 `Weak Entiry`로 간주될 수 있습니다.

# Attribute

- **Simple Attribute**
    - 더 이상 분해할 수 없는 기본적인 속성으로 단일 값을 가지며, 추가로 세분화되지 않습니다. `ERD`에서 단일 타원으로 표현됩니다.
    - 예시로 이름, 전화번호 같은 속성은 `Simple Attribute`입니다.

- **Composite Attribute**
    - 여러 속성으로 구성된 속성으로, 단순 속성들로 분해될 수 있습니다. `ERD`에서 여러 타원으로 둘러싸인 복합 타원으로 표현됩니다.
    - 예시로 주소는 도시, 우편번호 등으로 나눌 수 있는 `Composite Attribute`입니다.

- **Multivalued Attribute**
    - 하나의 `Entity`가 여러 값을 가질 수 있는 속성입니다. `ERD`에서 이중 타원으로 표현됩니다.
    - 예시로 전화번호는 한 사람이 여러 개의 전화번호를 가질 수 있으므로 `Multivalued Attribute`입니다.

- **Derived Attribute**
    - 다른 속성의 값을 기반으로 계산되거나 도출될 수 있는 속성입니다. `ERD`에서 점선 타원으로 표현됩니다.
    - 예시로 나이는 생년월일을 기준으로 계산될 수 있으므로 `Derived Attribute`입니다.

# Relationship

- **One-to-One**
    - 하나의 `Entity` 객체가 다른 하나의 `Entity` 객체와만 연결되는 관계입니다.
    - 예시로 한 사람이 하나의 여권을 가지는 것을 말합니다.

- **One-to-Many**
    - 하나의 `Entity` 객체가 여러 개의 다른 `Entity` 객체와 연결되는 관계입니다.
    - 예시로 한 명의 고객이 여러 개의 주문을 할 수 있는 관계를 말합니다.

- **Many-to-One**
    - 여러 개의 `Entity` 객체가 하나의 `Entity` 객체와 연결되는 관계입니다.
    - 예시로 여러 개의 주문이 하나의 고객과 연결될 수 있습니다.

- **Many-to-Many**
    - 여러 개의 `Entity` 객체가 서로 여러 개의 `Entity` 객체와 연결될 수 있는 관계입니다.
    - 예시로 학생들은 여러 강의를 수강할 수 있고, 하나의 강의에 여러 학생이 등록될 수 있습니다.

# ERD 그리는 법

위 내용들을 통해 `ERD`에 대한 개념을 살펴봤습니다. 이번 목차에서는 그 내용들을 바탕으로 간단한 예시를 통해 `ERD`를 그려가는 과정에 대해 정리해보겠습니다. 먼저, 고객이 주문을 하고, 주문에는 상품이 포함된다는 것을 가정하고 시작하겠습니다.

가정에 따라 `Entity`, `Attribute`, `Relationship`을 정의하면 다음과 같습니다.

- **Entity**
    - 고객
    - 주문
    - 상품

- **Attribute**
    - 고객: 고객 ID(기본 키), 이름, 주소
    - 주문: 주문 ID(기본 키), 주문명, 날짜
    - 상품: 상품 ID(기본 키), 상품명, 가격

- **Relationship**
    - 고객은 주문을 한다는 것은 한 명의 고객이 여러 개의 주문을 할 수 있지만, 한 주문은 한 고객만을 가지는 것을 의미합니다. 따라서, 해당 관계는 `One-to-Many`입니다.
    - 주문에는 상품이 포함된다는 관계는 한 주문에 여러 상품이 포함될 수 있으며, 한 상품은 여러 주문에 포함될 수 있다는 것을 말하기 때문에 `Many-to-Many`입니다.

이와 같이 정의를 한 후, `ERD`를 그리는 것이 가능해집니다. `ERD`를 그려주는 툴은 다양한데 그 중 [DBdiagram.io](https://dbdiagram.io/home)을 사용한 결과와 코드를 첨부하며 글을 마치겠습니다.

![ERD Example](./assets/img/Fundamental-of-Database/ERD-Example.png)

```SQL
Table 고객 {
  고객_ID integer [primary key]
  이름 varchar
  주소 varchar
}

Table 주문 {
  주문_ID integer [primary key]
  주문명 varchar
  날짜 date
}

Table 상품 {
  상품_ID integer [primary key]
  상품명 varchar
  가격 integer
}

Ref: 고객.고객_ID < 주문.주문_ID
Ref: 주문.주문_ID <> 상품.상품_ID
```