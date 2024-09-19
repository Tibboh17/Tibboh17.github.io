---
title: Python으로 SQL 데이터베이스에서 데이터 불러오기
categories: [Project, MMORPG User Log Data]
tags: [Data Analyze, Python, EDA, Pandas, SQLAlchemy]
---

# 개요

일반적으로 `SQL`을 통해 데이터를 조작하고 원하는 데이터를 추출하지만, 이를 시각화하거나 특정한 형태로 만들기 위해서는 다른 기술이 필요합니다. 이번 포스트에서는 `SQL` 데이터베이스에 저장된 데이터를 `Python`으로 가져와서 데이터를 조작하는 과정을 다룰 것입니다.

사용할 데이터는 이전 포스트에서 `SQL` 로컬 서버에 저장한 데이터를 활용할 것이며, 코드에 대한 자세한 설명은 생략할 예정이니 유의해 주시기 바랍니다.

# SQLAlchemy

`Python`에서 `SQL` 데이터를 불러오기 위해, 이전 포스트에서 사용한 SQLAlchemy를 통해 SQL 서버와 연결하겠습니다.

```python
from sqlalchemy import create_engine

engine = create_engine("mysql+pymysql://username:password@host/database")
```

이전 포스트와 같이 자신의 환경에 맞게 다음 내용을 채워줍니다.

- `username`: MySQL 데이터베이스 사용자 이름
- `password`: MySQL 데이터베이스 비밀번호
- `host`: MySQL 서버의 주소 (예: localhost 또는 IP 주소)
- `database`: 연결하려는 MySQL 데이터베이스 이름

# Pandas 

이제 Pandas를 사용하여 SQL 데이터베이스에서 데이터를 데이터 프레임으로 불러오겠습니다.

```python
import pandas as pd

sql_query = "SELECT * FROM your_table"

df = pd.read_sql(sql_query, engine)
print(df.head())
```

`sql_query`를 통해 `SQL` 데이터에서 불러오고자 하는 데이터를 지정할 수 있습니다. 이때, 데이터의 이름을 `your_table` 자리에 입력하면 됩니다. 이 과정을 통해 `Pandas`의 `DataFrame`에 우리가 원하는 데이터를 저장하고, 그 데이터의 일부분을 출력한 결과는 다음과 같습니다.

![Pandas 예시 화면](./assets/img/MMORPG/SQL_Example_2.png)

다음 코드를 통해 `Pandas`를 활용한 데이터의 기본 정보를 확인할 수 있습니다.

```python
df.info()
```

![Pandas 예시 화면](./assets/img/MMORPG/SQL_Example_3.png)

```python
df.describe()
```

![Pandas 예시 화면](./assets/img/MMORPG/SQL_Example_4.png)