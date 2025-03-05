---
title: AI 네비게이션
description: AI Navigation
author: unm7925
date: 2025-01-22 04:00:00 +0900
categories: [ Unity_Func ]
tags: [Csharp, Unity]
pin: false # 고정
# math: false 수학기호
# mermaid: false 수학기호
image:
  path: 'AINavigation.jpg' # 이미지 이름
  # lqip:  # 저화질 넣기(인터넷)
  alt: 이거 쓰면 확실히 편하긴 해 인정이긴 해 # 부연설명명
media_subpath: '/assets/img/Github_Pages/Unity_Func/AiNavigation' # 이미지 주소 ( 끝 전)
---

# __AI Navigation__
<br>

이 글에 정리할 것 은 유니티 AI Navgation 사용 및 이어서 Agent, Obstacle 컴포넌트 사용 법 및 기능이다.

그렇다면 <br>
1. AI Navgation Setting
2. Nav Mesh Agent
3. Nav Mesh Obstacle
<br>

순서대로 덩이 덩이씩 정리하도록 하겠다.

## __AI Navigation__

이는 말 그대로 이며, 네비게이션 설치부터 시작하도록 하겠다.

우선, Unity -> Window -> `Pakage Manager` 를 들어간 후 그림과 같이 설치해준다.

![image](AINavigation_install.jpg)

설치를 하면 Unity -> Window에 AI 탭이 생기는데

- Navigation
- Navigation(obsolete)

가 있는데, 아래의 Navigation(obsolete)를 눌러주면 된다.

이유는 우리는 `Nav Mesh Agent` 또한 다룰 것 이기 때문이고, 생긴 창은 원하는 곳에 배치시켜 주면 된다.

필자는 Inspector와 같은 곳에 창을 두었다.

우선 4가지 TAP으로 구성되어 있다.
- Agents
- Areas
- Bake
- Object

### __AI Navtion(obsolete) Agents__

![image](AINavigation_Agents.jpg)

위에서부터 아래 순서대로 설명하자면

종류를 분류하는 리스트<br>
Name에 리스트를 작성, 게임으로 비유하자면 종족값을 기입 <br>
`Radius`는 해당 Agent가 지니는 둘레<br>
`Height`는 해당 Agent가 지니는 높이<br>
`Step Height`는 해당 Agent가 지나갈 수 있는 높이 ( 오를 수 있는 계단의 높이 )<br>
`Max Slope`는 해당 Agent가 지나갈 수 있는 경사로<br>
라고 분류될 수 있다.


### __AI Navtion(obsolete) Areas__

![image](AINavigation_Areas.jpg)

이제 앞으로 모두 위에서 부터 아래로 설명하도록 하겠다.

이는 지형에 대해 분류하는 것으로, 예시 작성과 같이 흔히들

걸을 수 있는 곳, 없는 곳 이런식으로 분류해서 나눠 사용한다.

### __AI Navtion(obsolete) Bake__

![image](AINavigation_Bake.jpg)

이는 Agent와 연관이 있는데 설명을 직관적으로 해보자면

여기서 Agent Radius부터 Step Height 까지를 고려해 이 구조물에서 움직일 수 있는 곳들을 먼저 연산하여 지정주는 것이다.

위의 그림과 같이, 둘레 0.5, 높이 2, 경사로 50도, 자신보다 0.4 높은 위치가 움직일 수 있는 곳들을 나타내준다.

아래는

`Drop Height` 는 Agent의 최대 점프 높이<br>
`Jump Distance` 는 Agent의 최대 점프 길이를 나타낸다.<br>
`Manual Voxel Size` 는 앞서 지정된 면적들의 Voxel 사이즈를 조절 할 수 있다.<br>
`Min Region Area`는 지정되기 위한 최소한의 면적이다.

### __AI Navtion(obsolete) Objects__

![Image](AINavigation_Object.jpg)

이는 면적으로 지정 된 메쉬 렌더러 및 터레인을 설정하는 방법이다.<br>
아까 지정한 지역을 나눠서 분류할 수 있다.

자 그럼 간단하게 AI Navigation로 특정 객체가 지나갈 수 있고, 없는 면적들을 분류하여 지정하였으면 이젠 `Nav Mesh Agent`를 알아보자.

## __Nav Mesh Agent__

Nav Mesh Agent 컴포넌트는 목표를 향해 움직일 때 서로 피해가는 캐릭터 생성에 유용하다.<br>
Agent는 Nav Mesh를 이용하여 게임 월드에 대해 움직이거나, 멈춰있는 장애물들을 피할 방법을 연산한다.

두 가지 경우로 분류해서 정리하도록 한다.

- 컴포넌트 인스펙터
- 스크립트 파라미터

### __Nav Mesh Agent Inspector__

![image](NavMeshAgent_Inspector.jpg);

__Agent Size__


|  프로퍼티   | 기능                                                                  |
| :---------: | --------------------------------------------------------------------- |
|   Radius    | 에이전트의 반경은 장애물과 다른 에이전트 간의 충돌 계산하기 위해 사용 |
|   Height    | 에이전트가 장애물 밑으로 지나갈 수 있는 높이 간격                     |
| Base Offset | 트랜스폼 피봇 포인트와 관련한 충돌 실린더의 오프셋                    |

__Streering__

|      프로퍼티      | 기능                                                                                                                                                       |
| :----------------: | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
|       Speed        | 최대 이동 속도( 초당 월드 단위 )                                                                                                                           |
|   Angular Speed    | 최대 회전 속도 ( 초당 각도 )                                                                                                                               |
|    Acceleration    | 최대 가속 ( 제곱 초당 월드 단위로 )                                                                                                                        |
| Stopping Distance  | 에이전트는 목표 위치에 가까워졌을 시 정지                                                                                                                  |
| Auto Braking  <br> | 활성화 시 에이전트는 목적지에 다다를 때 속도를 줄임. <br>멀티플 포인트 사이에서 부드럽게 움직여야 하는 순찰과 같은 동작을 할 때에는 반드시 비활성화 해야함 |

__Obstacle Avoidance__

|   프로퍼티    | 기능                                                                                                                                                                                                 |
| :-----------: | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Quality <br>  | 장애물 회피 품질 - 에이전트의 수가 많다면 장애물 회피 품질을 줄임으로써 CPU 시간을 절약,<br> 회피를 없음으로 설정할 경우 충돌만 해결할 수 있을 뿐 다른 에이전트와 장애물에 대한 적극적인 회피는 안함 |
| Priority <br> | 낮은 우선 순위의 에이전트는 이 에이전트의 회피 대상에서 제외<br> 값은 0에서 99사이에서 설정되어야 하며 낮은 숫자가 높은 우선 순위임을 의미                                                           |

__Path Finding__

|            프로퍼티             | 기능                                                                                                                                                                                                                                   |
| :-----------------------------: | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Auto Traverse <br> OffMesh Link | 자동적으로 오프 메시 링크를 횡단하려면 트루로 설정.<br> 애니메이션을 사용하거나 오프메시 링크를 횡단하는 특정한 방법을 사용하고 싶다면 끄기.                                                                                           |
|       Auto Repath    <br>       | 활성화 시 에이전트가 경로 일부분의 끝에 도달하면 경로를 재탐색 <br> 목적지까지 경로가 없다면 목적지에서 제일 가깝게 도달할 수 있는 위치까지 부분적인 경로가 생성                                                                       |
|       Area Mask <br><br>        | 에이전트가 경로 탐색에 어떠한 영역 타입을 고려할 것인지를 설명 <br> 내비메시 베이킹를 위해 메시를 준비할 때 각각의 메시 영역 타입을 설정 <br> 예를 들어 계단을 특별한 영역 타입으로 표시하고 몇 몇 캐릭터 타입의 계단 이용을 금지 가능 |

### __Nav Mesh Agent Scripts__

|   분류    | 기능                                  | 설명                                               |
| :-------: | ------------------------------------- | -------------------------------------------------- |
| 이동 제어 | `destiantion`                         | 이동하려는 목적지 설정                             |
|     -     | `speed`                               | 이동 속도 설정                                     |
|     -     | `angularSpeed`                        | 방향 전환 속도 설정                                |
|     -     | `acceleration`                        | 속도 변화 비율 설정                                |
|     -     | `isStopped`                           | 이동 중지 및 재개                                  |
|     -     | `Warp(Vector3 pos)`                   | 특정 위치로 순간 이동                              |
|     -     | `Move(Vector3 offset)`                | NavMesh 제약 없이 특정 방향으로 이동               |
| 경로 탐색 | `SetDestination(Vector3 target)`      | 목표 위치 설정 및 이동 시작                        |
|     -     | `ResetPath()`                         | 현재 경로 초기화                                   |
|     -     | `CalculatePath(Vector3, NavMeshPath)` | 목표 위치로 가는 경로 계산                         |
|     -     | `path`                                | 에이전트가 따르는 현재 경로 ( `NavMeshPath` 객체 ) |
|     -     | `areaMask`                            | 에이전트가 이동 가능한 NavMesh 영역 설정           |
| 상태 확인 | `remainingDistacne`                   | 현재 위치에서 목적지까지의 남은 거리               |
|     -     | `hasPath`                             | 에이전트가 유효한 경로를 갖은지 확인               |
|     -     | `velocity`                            | 에이전트의 현재 이동 속도 벡터                     |
|     -     | `isOnOffMeshLink`                     | 에이전트가 현재 Off-Mesh Link에 있는지 확인        |
|     -     | `CompleteOffMeshLink()`               | Off-Mesh Link 를 건너는 작업 완료                  |
|    etc    | `autoBraking`                         | 목적지 도달 시 자동 감속 여부 설정                 |
|     -     | `stoppingDistance`                    | 목표에 도달했따고 간주할 최소 거리 설정            |
|     -     | `autoRepath`                          | 경로가 막혔을 때 자동 재탐색 여부 설정             |

## __Nav Mesh Obstacle__

Nav Mesh Agent가 월드를 탬색하는 동안 피해야 하는 움직이는 장애물이다.<Br>
장애물이 정지상태 일 때는 Nav Mesh에 구멍을 냅니다.<br>
Agent는 돌아가거나, 경로가 완전히 차단 될 경우 다른 길을 찾습니다.<br>

![image](NavMeshObstacle_Inspector.jpg)

### __Nav Mesh Obstacle Inspector__

|       프로퍼티        | 기능                                                            |
| :-------------------: | --------------------------------------------------------------- |
|         Shape         | 장애물 지오메트리의 모양, 오브젝트 모양에 가장 적합한 것을 선택 |
|          Box          | ---                                                             |
|        Center         | 변환 포지션에 대한 박스의 상대적 중심                           |
|         Size          | 상자의 크기                                                     |
|        Capsule        | -                                                               |
|        Center         | 변환 포지션에 대한 캡슐의 상대적 중심                           |
|        Radius         | 캡슐의 반지름                                                   |
|        Height         | 캡슐의 높이                                                     |
|         Func          | -                                                               |
|         Carve         | Carve 체크 시 Navi Mesh Obstacle이 Navi Mesh에 구멍을 생성      |
|    Move Threshold     | 움직이는 파인 구멍을 업데이트 하는 임계 거리 설정               |
|  Time To Stationary   | 장애물이 정지되었다고 간주할 때 까지 기다리는 시간              |
| Carve Only Stationary | 활성화 시 장애물이 정지되어 있을 경우만 구멍을 생성             |

### __Nav Mesh Obstacle Script__


|         프로퍼티          | 설명                                                                |
| :-----------------------: | ------------------------------------------------------------------- |
|          `size`           | 장애물의 크기 설정 (`Vector3`)                                      |
|         `center`          | 장애물의 중심 위치 설정 (`Vecotr3`)                                 |
|         `carving`         | 장애물이 경로에 영향을 주기 위해  NavMesh를 동적 업데이트 설정      |
|  `carvingMoveThreshold`   | 장애물이 이동할 때 NavMesh를 다시 계산하기 위한 최소 거리 (`float`) |
| `carvingTimeToStationary` | 장애물이 정지 상태로 간주되기 까지 걸리는 시간 (`float`)            |
|          `shape`          | 장애물의 모양 설정 (`NavMeshObstacleShape`)                         |
|   `velocity(읽기전용)`    | 장애물의 이동 속도 반환 (`Vector 3`)                                |
