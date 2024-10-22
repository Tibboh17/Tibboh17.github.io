---
title: AI NPC 초기 모델
categories: [Project, AI NPC]
tags: [Project, Python, AI, Game, NPC, Gen AI]
---

# 개요

이전에 프로젝트 계획을 작성한 이후, 초기 모델을 구축했습니다. 처음으로 구현한 NPC는 LOST ARK의 루테란입니다. 루테란에 대한 데이터를 수집하고 프롬프트를 작성하면서, 어떤 데이터베이스에 저장하고 이를 어떻게 활용할지에 대한 고민이 구체화되었고, 프로젝트 구성에 대한 이전보다 윤곽도 잡히기 시작했습니다. 이에 대한 과정을 기록하고자 이번 글을 작성하게 되었습니다.

# 프로젝트 구성

현재 프로젝트 폴더는 다음과 같이 구성되어 있습니다.

```markdown
AI-NPC/
├── .env
├── .gitignore
├── README.md
└── npc.py
```

현재 모든 파일이 루트 디렉토리에 위치하고 있습니다. 따라서, `npc.py`에 작성된 코드를 분리하여 별도의 파일로 저장하고, 이와 함께 효율적인 관리가 가능하도록 폴더들을 생성할 계획입니다. 이에 대한 수정안은 다음과 같습니다.

```markdown
AI-NPC/
├── .env
├── .gitignore
├── docs/
│   ├── README.md
└── src/                  
    ├── database.py
    ├── main.py
    └── npc.py
```

# 초기 모델

ChatGPT API를 사용하여 간단히 작성한 코드입니다. 중간에 오류가 발생하였지만, 이는 단순히 API 사용을 위한 추가 결제에 의해 생긴 것이어서 금방 해결되었습니다.

작성한 프롬프트를 바탕으로 대화를 진행한 결과, 처음에는 대화의 맥락이 이어지지 않았습니다. 그러나 `AI_NP`C 함수에 이전 대화를 지속적으로 저장하는 코드를 추가한 이후부터 모델이 주어진 정보를 활용하여 만족스러운 대화를 이어갈 수 있었습니다. 프롬프트의 구체적인 내용은 `Github` 저장소에 있는 코드를 참고하시면 됩니다.

```python
import os
from dotenv import load_dotenv
from openai import OpenAI

load_dotenv()

api_key = os.getenv("OPENAI_API_KEY")
client = OpenAI(api_key=api_key)

data = {
    "Lutheran": "{your_prompt}"
}

messages = [
    {
        "role": "system",
        "content": data["Lutheran"],
    }
]

def AI_NPC(player_input):
    messages.append(
        {
            "role": "user",
            "content": player_input,
        }
    )

    response = chat_completion = client.chat.completions.create(
        messages=messages,
        model="gpt-4o-mini",
    )

    npc_reply = response.choices[0].message.content
    messages.append(
        {
            "role": "assistant",
            "content": npc_reply,
        }
    )

    return npc_reply

while True:
    player_input = input()
    if player_input.lower() == "exit":
        break
    npc_response = AI_NPC(player_input)
    print(npc_response)
```

# NoSQL

프롬프트에 포함되는 내용은 NPC의 스토리, 성격, 말투 등입니다. 현재는 `npc.py`에 데이터가 함께 저장되어 있지만, `NPC`의 수가 늘어날수록 데이터를 관리하기 위해 데이터베이스가 필요하다는 판단이 들었습니다.

수집된 데이터는 자주 수정될 가능성이 있기 때문에 `JSON` 형식으로 저장하고 관리하는 것이 적절하다고 생각하여, `NoSQL`을 사용하려고 합니다. 그중 `MongoDB`를 사용해본 경험이 있기 때문에, 필드를 구체적으로 나누고 정리하게 되면 빠르게 로컬 서버에 데이터를 저장할 계획입니다. 저장된 데이터를 사용할 경우, `database.py`에서 `PyMongo`를 사용해서 불러올 것입니다.