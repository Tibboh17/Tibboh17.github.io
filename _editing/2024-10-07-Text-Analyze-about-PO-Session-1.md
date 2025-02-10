---
title: PO Session 1 스크립트 분석
categories: [Project, Text Analyze]
tags: [Data Analyze, Python, Text]
---

# 개요

항상 수치적인 부분에만 신경썼기에 자연어 처리와 같은 텍스트 분석에 큰 관심을 두지 않았습니다. 특히, 통계 대학원 진학을 준비했을 때는 자연어 처리 분야를 전혀 고려하지 않았었습니다. 아무래도 관심 분야가 수치에 매우 민감한 곳이다 보니 그랬던 것 같습니다. 

그러나 요즘 자주 사용하는 생성형 AI는 텍스트 기반의 처리와 분석을 통해 만들어지는 것이고, 저에게 많은 인사이트를 제공해주시는 멘토님께서 텍스트 분석에 대한 책을 내시고 이에 대한 강의도 올리셨기 때문에, 직접 해보면 좋을 것 같다는 생각이 들었습니다.

따라서, 이번 포스트에서는 멘토님의 추천으로 시청하고 글로 정리한 Toss 리더의 PO Session 영상 스크립트를 분석하려고 합니다. 텍스트 분석에 대한 내용은 잘 모르기에 빈도수를 통해 간단한 분석을 진행할 것이며 작성된 코드에 대한 설명은 따로 하지 않을 것임을 먼저 알립니다. 그리고 영상마다 새로운 분석 기법을 적용해보는 글을 이어가겠습니다.

# YouTube 스크립트

이번 영상의 `YouTube` 스크립트는 [DownSub](https://downsub.com/)을 통해 다운로드받았습니다. 사용 방법은 영상의 주소를 입력하여 다운로드받는 것으로 매우 간단합니다. 영상과 함께 스크린샷을 첨부하니 이를 따라 진행해보시길 바랍니다.

{% include embed/youtube.html id="tcrr2QiXt9M" %}

![Screenshot Example 1](./assets/img/Text-Analyze/PO-Session-1-Script-Download-Example-1.png)

![Screenshot Example 2](./assets/img/Text-Analyze/PO-Session-1-Script-Download-Example-2.png)

다른 방식으로도 스크립트를 얻을 수 있기 때문에, 앞으로 계속해서 영상들의 텍스트를 분석할 때마다 새로운 방법으로 스크립트를 불러와보겠습니다.

# 단어 빈도수 분석

저장한 스크립트를 `Python`을 통해 단어 단위로 분리해 빈도수를 확인해보겠습니다.

```python
import collections

path = "./datasets/토스 리더가 말하는 PO가 꼭 알아야할 개념.txt"

with open(path, "r", encoding="utf-8") as file:
    script = file.read()

words = script.split()
word_count = collections.Counter(words)
common_words = word_count.most_common(20)

for word, count in common_words:
    print(word, count)
```

위 코드를 실행하면 결과는 다음과 같습니다.

![Result 1](./assets/img/Text-Analyze/PO-Session-1-Result-1.png)

이 중에서 `Carrying`이라는 단어가 자주 등장하는 것은 이 영상에서 `Carrying Capacity`가 주요 주제임을 보여줍니다. 또한, `광고`, `들어오는`과 같은 단어들이 여러 차례 반복되며, 제품과 관련된 유저 확보 및 유지에 대한 논의가 중심이 됨을 유추할 수 있습니다.

# 주제 변화 분석

이 영상은 Toss 리더가 주요 주제에 대해 질문을 던지고, 그에 대한 설명과 예시를 제시하며 흐름이 진행됩니다. 따라서, 주제 변화 분석을 위해 `질문`이라는 단어로 스크립트를 나누어 각 섹션별로 분석을 하려고합니다.

본격적으로 분석하기에 앞서, 어떤 단어들의 빈도수를 확인할지를 정하겠습니다. 영상을 시청한 제 입장에서 가장 자주 나왔던 단어로 느껴지는 것들은 `Carrying Capacity`, `MAU`, `유저`, `유입`, `광고`입니다. 이 단어들이 어떤 섹션에 등장하고 얼마나 많이 나오는지 확인하면서 영상의 흐름에 대해 확인하겠습니다. 다음 코드는 이전 목차에서 작성된 코드의 연장선입니다.

```python
sections = script.split("질문")

keywords = ["Carrying Capacity", "MAU", "유저", "광고"]

keyword_counts = {keyword: [] for keyword in keywords}

for section in sections:
    section_counts = {keyword: section.count(keyword) for keyword in keywords}
    for keyword in keywords:
        keyword_counts[keyword].append(section_counts[keyword])

for keyword, counts in keyword_counts.items():
    print(keyword, counts)
```

위 코드의 결과는 다음과 같습니다.

![Result 2](./assets/img/Text-Analyze/PO-Session-1-Result-2.png)

먼저 결과를 보면 `질문`이라는 단어로 인해 스크립트가 총 18개의 섹션으로 나누어졌음을 알 수 있습니다. 결과에 대한 분석은 아래에서 이어가겠습니다.

- `Carrying Capacity`
    - 초기 섹션에서는 2회 언급되었지만, 후반부에서 5회, 7회, 15회로 급격히 증가했습니다. 이 단어가 초반에는 간단히 소개되었고, 영상의 후반부에서 주요 주제로 자리 잡았음을 보여줍니다.
- `MAU`
    - 처음에는 언급되지 않다가 14번째 섹션부터 3회, 7회, 23회로 증가합니다. `Carrying Capacity`와 함께 영상의 후반부에서 중요하게 다루어진 것으로 보입니다.
- `유저`
    - 중간 섹션에 걸쳐 몇 차례 언급된 후, 후반부에서 13회, 12회, 18회로 증가합니다. 위 두 단어가 해당 단어를 사용하여 설명되기에 후반부에 많이 등장했습니다.
- `광고`
    - 9번째 섹션에서 7회 언급된 후, 15번째 섹션에서는 6회, 24회로 증가합니다. 중반부에서 `광고`와 `유저`의 관계에 대해 다루고, 후반부에서는 `Carrying Capacity`, `MAU`와 함께 제품을 위해 이를 사용하는 방식 또는 영향에 대해 설명된 것으로 보입니다.