---
title: 생성형 AI를 활용하여 데이터 생성
categories: [Project, MMORPG User Log Data]
tags: [Data Analyze, Python, EDA, Pandas, SQL]
---

# 개요

게임 로그 데이터는 보안 문제와 개인정보 보호 때문에 개인이 직접 접근하기 어려운 경우가 많습니다. 또한, 원하는 주제에 대한 데이터를 다루는 연습을 하고 싶어도, 조건에 맞는 데이터를 구하는 것은 쉽지 않은 현실입니다. 이번 포스트에서는 생성형 AI를 활용하여 필요한 데이터를 생성하고, 이를 SQL 로컬 서버에 저장하는 과정을 소개하겠습니다. 이번에 제가 선정한 주제는 MMORPG 유저 로그 데이터입니다.

데이터를 생성하기 위해 제 구독 서비스로 이용 중인 ChatGPT-4를 사용할 예정입니다. 또한, SQL 및 Workbench의 설치와 사용법에 대해서는 다루지 않을 예정이니 이 점을 유의해 주시기 바랍니다.

# 데이터 생성

우선 ChatGPT에게 다음 항목들을 반영하여 데이터를 생성하도록 할 것입니다.

- `Timestamp`: 로그 기록 시간 (YYYY-MM-DD HH 형식)
- `UserID`: 유저의 고유 ID
- `CharacterName`: 캐릭터 이름
- `ActionType`: 행동 유형 (로그인, 로그아웃, 퀘스트 시작, 퀘스트 완료, 아이템 구매, PvP 전투 등)
- `ActionDetails`: 행동에 대한 세부 정보 (예: 퀘스트 이름, 아이템 이름, 전투 결과 등)
- `Location`: 유저의 현재 위치 (맵 이름 또는 좌표)
- `Level`: 캐릭터 레벨
- `XP`: 행동으로 얻은 경험치
- `Currency`: 행동으로 얻거나 사용한 게임 내 화폐
- `PartyID`: 파티 ID (유저가 파티에 속해 있을 경우)
- `GuildID`: 길드 ID (유저가 길드에 속해 있을 경우)
- `Latency`: 네트워크 지연 시간 (ms)
- `Churn`: 게임 이탈 여부 (유저가 30일 이상 게임에 로그인하지 않은 경우 True로 표시)
- `HasPurchased`: 게임 내 구매 여부 (유저가 게임 내에서 한 번이라도 구매를 했을 경우 True로 표시)

이러한 항목들을 반영하여 ChatGPT에 데이터를 생성하도록 요청할 것이며, 생성된 예시 데이터를 확인하여 제 의도에 맞게 올바르게 반영되었는지 검토할 예정입니다.

| Timestamp           | UserID | CharacterName | ActionType    | ActionDetails          | Location        | Level | XP   | Currency | PartyID | GuildID | Latency | Churn | HasPurchased |
|---------------------|--------|---------------|---------------|------------------------|-----------------|-------|------|----------|---------|---------|---------|-------|--------------|
| 2024-09-03 14:23:45 | 10001  | ShadowKnight  | Login         | -                      | StartTown       | 34    | 0    | 0        | NULL    | 2001    | 32      | False | True         |
| 2024-09-03 14:25:10 | 10001  | ShadowKnight  | QuestStart    | "Defeat the Dragon"    | DragonCave      | 34    | 0    | 0        | NULL    | 2001    | 35      | False | True         |
| 2024-09-03 14:50:30 | 10001  | ShadowKnight  | QuestComplete | "Defeat the Dragon"    | DragonCave      | 35    | 5000 | 2000     | NULL    | 2001    | 30      | False | True         |
| 2024-09-03 14:53:12 | 10002  | FireMage      | Purchase      | "Legendary Staff"      | MagicShop       | 28    | 0    | -5000    | NULL    | NULL    | 45      | False | False        |
| 2024-09-03 15:05:44 | 10003  | WindArcher    | PvP           | "Won vs ThunderWarrior"| Arena           | 40    | 300  | 0        | 3002    | NULL    | 28      | True  | True         |
| 2024-09-03 15:10:11 | 10001  | ShadowKnight  | Logout        | -                      | StartTown       | 35    | 0    | 0        | NULL    | 2001    | 33      | False | True         |

예시 데이터가 올바르게 생성되었으므로, 이제 대량의 유저 데이터 생성을 진행할 예정입니다. 저자는 총 10,000명의 데이터를 CSV 파일로 생성할 계획입니다. 생성된 데이터를 프로젝트 디렉토리에 저장한 후, SQL 데이터베이스에 저장하는 작업을 진행할 준비를 하겠습니다.

# 데이터 저장
먼저 우리는 Python을 통해 생성된 데이터를 불러와 이를 SQL 로컬 서버에 있는 데이터베이스에 저장할 계획입니다. 이에 앞서 2개의 Python 라이브러리를 설치하겠습니다.
```shell
pip install pandas
pip install sqlalchemy
```
**Pandas**는 내 컴퓨터나 클라우드 저장소에 있는 데이터를 Python으로 불러오는 데 도움을 주는 라이브러리입니다. **SQLAlchemy**는 Python에서 SQL을 사용할 수 있도록 해주는 도구로, 이번 포스트에서는 SQL 로컬 서버와 Python을 연결해주는 역할을 수행합니다. 설치를 마치고 나면 **Jupyter Notebook** 환경에서 데이터를 불러오는 작업을 진행하겠습니다.

```python
import pandas as pd

file_path = "your file path"
df = pd.read_csv(file_path)
```

위 코드를 통해 ChatGPT를 통해 생성한 데이터를 불러오게 됩니다. 이제 내 컴퓨터에 설치된 SQL 로컬 서버로 데이터를 옮기기위한 준비를 하겠습니다.

```python
from sqlalchemy import create_engine

engine = create_engine("mysql+pymysql://username:password@host/database")
```

위 코드는 먼저 SQLAlchemy에서 데이터베이스 연결을 생성하는 create_engine 함수를 가져옵니다. 그런 다음 MySQL 데이터베이스와 PyMySQL 드라이버를 사용하여 연결을 설정하고, 설정한 내용에 따라 데이터를 데이터베이스에 저장할 수 있는 엔진을 생성합니다. 아래의 요소들에 본인의 정보를 입력하면 됩니다.

- `username`: MySQL 데이터베이스 사용자 이름
- `password`: MySQL 데이터베이스 비밀번호
- `host`: MySQL 서버의 주소 (예: localhost 또는 IP 주소)
- `database`: 연결하려는 MySQL 데이터베이스 이름

정보를 입력한 후, 마지막으로 다음 코드를 통해 데이터를 SQL 서버로 옮기겠습니다.

```python
df.to_sql("tablename", con=engine, if_exists="replace", index=False)
```

`tablename`에 데이터가 담길 테이블의 이름을 설정한 후, 코드를 실행하면 본인의 SQL 데이터베이스에서 해당 데이터가 저장된 모습을 확인할 수 있습니다.

![Workbench 예시 화면](./assets/img/SQL_Example.png)