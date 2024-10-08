---
title: 데이터 전처리 및 기초 통계 분석하기
categories: [Project, MMORPG User Log Data]
tags: [Data Analyze, Python, EDA, Pandas, SQLAlchemy, Basic Statistics]
---

# 개요

이전 포스트까지 데이터를 생성하고 SQL 데이터베이스에 저장한 후, `Jupyter Notebook`을 통해 데이터를 다시 불러와 살펴볼 수 있는 환경을 구축했습니다. 이번 포스트에서는 불러온 데이터에 대해 자세히 살펴보고 처리하는 과정을 진행하겠습니다. 예를 들어, 데이터 전처리, 기초 통계 분석 등을 다룰 예정입니다.

# 데이터 불러오기

이전 포스트에서 사용한 코드를 통해 데이터를 다시 불러오겠습니다.

```python
import pandas as pd
from sqlalchemy import create_engine

engine = create_engine("mysql+pymysql://username:password@host/database")
sql_query = "SELECT * FROM your_table"
df = pd.read_sql(sql_query, engine)
```

이제 이 데이터를 사용하여 각 항목을 하나씩 살펴보겠습니다.

# 데이터 전처리

다음 코드들을 통해 데이터의 정보를 확인하겠습니다.

```python
df.shape
```
![Pandas 예시 화면](./assets/img/MMORPG/SQL_Example_5.png)

위 코드를 통해 이 데이터셋에는 14개의 필드와 110000개의 데이터가 포함되어 있는 것을 확인할 수 있습니다.

```python
df.info()
```

![Pandas 예시 화면](./assets/img/MMORPG/SQL_Example_6.png)

이번에는 각 필드에 대한 정보를 확인해보겠습니다. 위 정보에서 `Dtype`을 통해 데이터의 타입을 확인할 수 있습니다. 여기서 주목할 점은 `Timestamp` 필드의 데이터 타입이 `object`인 것 입니다. 이 필드는 이벤트 발생 날짜와 시간을 나타내므로, 날짜 및 시간 형식이어야 합니다. 다음 코드를 통해 `Timestamp`의 데이터 타입을 변환하겠습니다.

```python
df["Timestamp"] = pd.to_datetime(df["Timestamp"])
df.info()
```

![Pandas 예시 화면](./assets/img/MMORPG/SQL_Example_7.png)

코드를 실행한 결과 올바르게 데이터 타입이 변경되었습니다. 

다음으로 결측치를 처리하는 과정을 진행하겠습니다. 위 결과를 다시 보면 `PartyID`와 `GuildID`의 데이터 개수가 부족한 것을 알 수 있습니다. 전체 데이터의 개수보다 약 1000개 정도 부족합니다. 여기서 이 결측치를 어떻게 처리할 것인지에 대해 생각해봐야합니다.

1. **결측치 제거**
    - 이 데이터는 유저의 로그 데이터이므로, 결측치가 있는 행을 제거하면 해당 유저의 다른 필드에 대한 정보들도 사라지게 됩니다. 이는 상당한 데이터 손실을 초래할 수 있으므로, 결측치를 단순히 제거해서는 안 됩니다.

2. **결측치를 특정 값으로 대체**
    - 게임을 이용하는 유저들 중 일부는 파티나 길드에 가입하지 않을 수 있습니다. 따라서 이러한 가정을 바탕으로 결측치를 `0`이나 `None`으로 대체하여 처리하면 데이터 손실 없이 결측치를 관리할 수 있습니다.

3. **추정치를 사용해 대체**
    - `PartyID`와 `GuildID`는 특정 사용자나 그룹에 대해 고유하게 할당된 값이기 때문에, 추정치로 대체하는 것이 적절하지 않습니다. 단순히 평균값이나 중앙값으로 대체하는 것은 의미가 없으며, 올바른 분석을 저해할 수 있습니다.

따라서 결측치를 모두 `0`으로 대체하는 것이 유저가 파티나 길드에 소속되지 않았다는 의미를 명확하게 반영합니다. 이를 위해 다음 코드를 사용하여 결측치를 처리하고, 처리된 결과가 제대로 반영되었는지 결측치의 개수를 통해 확인해보겠습니다.

```python
df["PartyID"].fillna(0, inplace=True)
df["GuildID"].fillna(0, inplace=True)

missing_values_after = df[["PartyID", "GuildID"]].isnull().sum()
print(missing_values_after)
```

![Pandas 예시 화면](./assets/img/MMORPG/SQL_Example_8.png)

결과에서 알 수 있듯이, 모든 결측치가 제대로 대체되어 총 결측치의 개수가 `0`이 되었다는 것을 확인할 수 있습니다.

# 기초 통계 분석

지금까지 원활한 분석을 위해 데이터를 처리하는 과정을 진행했습니다. 이제 이 데이터를 바탕으로 분석에 유의미한 필드에 대해 기초적인 통계 수치를 확인하겠습니다. `PartyID`와 `GuildID`는 기초 통계 자료가 아닌 가입 여부에 대해 비교해 보겠습니다.

```python
df.describe()
```

![Pandas 예시 화면](./assets/img/MMORPG/SQL_Example_9.png)

1. **Timestamp** (시간 정보)
    - 기초 통계 자료를 통해 데이터는 2024년 1월부터 9월까지 약 9개월에 걸쳐 수집되었음을 알 수 있습니다.

2. **Level** (레벨)
    - 사용자들의 평균 레벨은 약 25로 나타나고 있으며, 이는 중간 수준입니다. 레벨 1인 사용자도 존재하고, 최대 레벨은 50입니다. 25% 이상의 사용자들은 레벨 38 이상에 도달해 있으며, 상위 50%는 레벨 25 이상입니다. 이는 사용자들이 게임을 어느 정도 깊이 있게 플레이하고 있음을 나타내며, 고레벨 사용자들이 상당히 존재함을 보여줍니다.

3. **XP** (획득 경험치)
    - 유저들은 평균적으로 887.79의 일일 경험치를 획득하고 있습니다. 그러나 경험치의 분포는 불균형합니다. 50% 이상의 사용자들은 경험치가 0으로 나타나는 반면, 상위 25%의 사용자는 최소 290의 경험치를 획득했으며, 최대로 획득한 경험치는 9998입니다. 이는 경험치가 특정 활동을 통해 얻어지는 것에 비해 많은 사용자가 경험치 활동에 참여하지 않는다는 추측을 가능하게 합니다.

4. **Currency** (게임 내 화폐)
    - 평균 화폐 소비량이 -2.13이라는 것은 대부분의 사용자들이 화폐를 벌기보다는 소모하는 경향이 있음을 의미합니다. 하위 50%의 사용자들은 화폐를 벌지 않거나 사용하지 않은 것으로 나타나며, 상위 25%의 사용자들은 0 이상의 화폐를 획득하고 있습니다. 단, 화폐 사용의 분포는 화폐를 많이 벌거나 사용한 소수의 사용자에게 집중될 수 있으므로, 상세한 지표를 통한 추가 분석이 필요하다고 여겨집니다.

5. **PartyID** (파티 고유 ID)
    - 파티에 가입한 캐릭터는 108958개이고, 가입하지 않은 경우는 1042개입니다. 대부분의 캐릭터가 파티 플레이를 하는 경향이 있음을 알 수 있습니다.

6. **GuildID** (길드 고유 ID)
    - 길드에 가입한 캐릭터는 108858개이고, 가입하지 않은 경우는 1142개입니다. 대부분의 캐릭터가 길드에 가입하여 게임 내 커뮤니티 활동을 하는 경향이 있음을 알 수 있습니다.

7. **Latency** (지연 시간)
    - 지연 시간은 대부분 사용자들에게 안정적으로 나타납니다. 평균 지연 시간은 85ms로, 상위 25%는 118ms 이상의 지연 시간을 겪고 있습니다. 최대값이 150ms인 것으로 보아, 일부 사용자들은 네트워크 연결 문제로 인해 다소의 어려움을 겪을 수 있음을 시사합니다.

8. **Churn** (사용자 이탈 여부)
    - 평균값이 0.549이라는 것은 약 54.9%의 사용자가 이탈한 것으로 보입니다. 즉, 데이터에 포함된 사용자 중 1/2이 넘는 비율이 게임을 중단한 상태입니다. 따라서 사용자 이탈을 방지하기 위한 추가 분석이 필요할 수 있습니다.

9. **HasPurchased** (구매 여부)
    - 약 86.7%의 사용자가 게임 내에서 구매를 한 경험이 있습니다. 이는 대부분의 사용자들이 게임 내에서 화폐 또는 아이템을 구매하는 경향이 있음을 나타냅니다. 구매 여부는 게임의 수익성과 밀접한 관련이 있으며, 구매 사용자와 비구매 사용자 간의 행동 차이를 분석하는 것도 유의미할 수 있습니다.