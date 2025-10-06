# 『 Dancing Gueen : 펭귄 슬라이드 휭~ 』

![Project Thumbnail](Thumbnail.png)
![Project Map](DCMap.png)

---

## 🧠 프로젝트 소개

**📆 제작 기간: 2025.09.08 ~ 2025.10.**

### 💭 아이디어  
이 프로젝트의 게임 아이디어는《컴퓨터 밀리 흉내내기》와 《Hide & Chick》에서 영감을 받았다. 

두 게임이 가진 **캐주얼 파티성**과 **숨기/교란 요소**를 바탕으로, 

여기에 **서바이벌 + 히든 승리 조건**을 접목하여 새로운 형태의 멀티플레이 경험을 기획하였다.

### 🎮 Gameplay

- **장르**
    - 멀티플레이 서바이벌 파티 게임 (TPS / 캐주얼 액션)
- **전투**
    - AI가 아닌 **플레이어**를 처치해야 한다.
    - 플레이어를 처치하면 **KillCount++** 가 올라간다.
- **미션**
    - 최종 1인으로 생존하거나, 춤추기를 15회 성공하면 **히든 승리 조건**이 발동하여 즉시 승리할 수 있다.
- **핵심 목표**
    - 아이템과 전략을 활용해 다른 플레이어를 제거하고,  마지막 생존자가 되어 승리하거나 히든 조건을 달성해 조기 승리를 쟁취하라.

<br>

## 🎯 프로젝트 목표

- **최후의 1인을 가리는 서바이벌 파티 게임**
     - 플레이어 처치 → KillCount 증가 → 최종 생존자 판정 or 히든 승리 조건 발동
- **다양한 플레이 동작과 전략 요소**
     - 춤추기, 숨기, 아이템 활용, 자기장 축소 등 상황에 맞는 선택 제공
- **히든 승리 조건 시스템**
     - 춤추기 15회 성공 시 즉시 승리 가능 → 전략적 모험 요소 추가
- **멀티플레이 기반 구조**
     - 플레이어 간 전투 및 상태(HP, 애니메이션, KillCount) 서버–클라이언트 동기화
- **UI/UX 완성도 강화**
     - 체력/산소 게이지 HUD, 결과 화면, 웨이팅/엔딩 메뉴 등 직관적 UI 제공

<br>

## 🧩 주요 기능

- **전투 & 생존**
    - **게임모드 / GameMode**
        - 플레이어 공격/피격 처리 및 승리 조건 관리
        - KillCount 증가 및 AliveCount 추적
    - **UI/HUD**
        - 플레이어 처치 시 KillCount 표시
        - HP/산소 게이지 실시간 반영
- **히든 승리 조건**
    - **게임모드 / GameMode**
        - `DanceCount` 관리 (15회 이상 → 즉시 승리)
        - **`AliveCount == 1`** → 메인 승리 판정
    - **UI/HUD**
        - DanceCount 누적 표시
        - 히든 승리 달성 시 전용 이펙트/메시지 출력
- **아이템 시스템**
    - **게임모드 / GameMode**
        - 맵 특정 지점에 랜덤 스폰 관리
    - **기믹 / Gimmick**
        - 아이템 획득 시 효과 적용
    - **캐릭터 / Character**
        - 획득 시 해당 이펙트 출력
- **자기장(Zone) 시스템**
    - **게임모드 / GameMode**
        - 주기적 페이즈 축소 및 안전 구역 업데이트
        - 바깥에 있는 캐릭터 HP 감소 처리
    - **UI/HUD**
        - 자기장 밖에 노출 시 얼어붙는 효과 발생
- **산소 시스템 (Breath)**
    - **게임모드 / GameMode**
        - 물속 여부 감지 및 산소 감소/회복 규칙 등록
        - Breath 값이 0일 때 HP 감소 이벤트 트리거
        - 물속/지상 전환 시 상태 동기화
    - **캐릭터 / Character**
        - 현재 위치가 물속인지 실시간 판정
        - 물속일 경우 1초마다 Breath 감소
        - Breath가 0이면 체력(HP) 감소 시작, 물 밖으로 나오면 Breath 회복 처리
    - **UI/HUD**
        - 현재 산소 게이지 표시 (**`BreathBar`**)
        - Breath가 30% 이하일 때 색상 변화 및 경고 효과
- **Waiting & Ending Flow**
    - **게임모드 / GameMode**
        - 웨이팅 레벨에서 준비 상태 관리
        - 게임 종료 후 결과 처리 및 재시작 흐름 제어
    - **UI/HUD**
        - 준비 상태 표시 (닉네임/Ready)
        - 엔딩 메뉴에서 등수 및 Kill Count 표시
    - **채팅 / Chat**
        - 웨이팅 레벨에서 플레이어 간 채팅 가능
        - 채팅 메시지는 서버에서 브로드캐스트 후 모든 클라이언트 HUD에 출력
 
<br>

## 🔁 Game Flow Chart

![Game Flow Chart](GameFlowChart.jpg)

**🐧 수많은 AI 속에서 ‘진짜 플레이어’를 식별해 속이고 추적·제압하며 최후의 1인이 되는 디덕션 생존 게임**

1. **게임 시작 (Title Menu)**
 - 게임을 실행하면 메인 메뉴가 열린다
 - 플레이어는 Start 버튼을 눌러 웨이팅 레벨로 이동한다

2. **Waiting Level**
 - 모든 플레이어가 준비될 때까지 대기하는 구간이다
 - 모두 준비가 되면 `Start` 버튼을 눌러 메인 레벨로 진입한다

3. **Main Level 입장**
 - 본격적인 게임 내부에서는 AI(적)가 스폰되고, 캐릭터 생성이 이루어진다
 - GameMode: 규칙/승리조건 등록 (최종 1명 생존 + 히든 승리: 춤 15회)

4. **플레이 동작 선택**
-  플레이어는 다양한 행동을 할 수 있다:
- 🕺 **춤추기**
   - 이벤트: `DanceStart/End` → `DanceCount++`
   - 패널티: 캐릭터 주위로 불꽃 기둥 (가시성↑/추적↑)
   - 보상: 히든 승리 조건 누적
- 🫥 **숨기**
   - 상태: 은신
- 🧲 **자기장**
   - 주기적 페이즈 축소 → 바깥에 있으면 체력 감소 지속
- 🌊 **수영**
   - 물속에 들어가면 1초마다 산소량이 감소
   - 산소가 0이 되면 체력이 줄어들기 시작
   - 물 밖으로 나오면 산소가 서서히 회복
- 🎒 **아이템 획득**
   - 4개의 랜덤 스폰 아이템 활용
- ⚔️ **공격**
   - 적 처치 → 점수 획득

5. **전투 & 공격**
 - 플레이어를 처치하면 **`KillCount++`**
 - 공격당한 플레이어는 데미지를 입고 체력 감소한다
 - AI 캐릭터 공격 시 : 데미지 판정 대신 놀리는 효과음이 발생한다
 - 핵심은 AI보다 플레이어를 많이 죽여 최종 생존자가 되는 것

6. **히든 승리 조건**
 - **메인 승리**: AliveCount == 1 → 승리
 - **히든 승리**: `DanceCount >= 15`이면 즉시 승리
 - **패배**: HP 0, 자기장 데스 등

7. **승리 조건 판정**
 - 최종적으로 마지막까지 살아남은 1명이 최종 승리자가 된다
 - 히든 조건 충족 시 조기 승리 가능하다

8. **Ending Menu**
 - 승리 또는 패배가 결정되면 엔딩 메뉴로 넘어간다
 - 여기서 결과를 확인하고, 게임을 종료하거나 다시 웨이팅 레벨로 진입해 시작할 수 있다

<br>

## 🎥 시연 영상

🔗 [YouTube]()

<br>

## 🛠 트러블 슈팅
- [NPC 몽타주 중복 재생 문제](https://www.notion.so/kimyeoul/NPC-280cf60eefb6808caae8c51ce2deae14)
- [슬라이딩 동작 중복 입력 문제](https://www.notion.so/kimyeoul/280cf60eefb6801f8bc7ca1a446e9ef0)
- [캐릭터 수영 이동 불가 문제](https://www.notion.so/kimyeoul/280cf60eefb6807798e4e9ae9ce49868)
- [Hit 이펙트 첫 스폰 깨짐 문제](https://www.notion.so/kimyeoul/Hit-280cf60eefb68014919bc624b968a06f)
- [캐릭터 사망 시 관전자 모드 문제](https://www.notion.so/kimyeoul/280cf60eefb680029d21f336cb957067)
- [캐릭터 이동 개선 문제](https://www.notion.so/kimyeoul/280cf60eefb680bbb8a0c346ccda0368)
- [채팅창 가독성 문제](https://www.notion.so/kimyeoul/280cf60eefb68016979af0b150fad6bf)
- [히든 승리 UI 닉네임 표시 문제](https://www.notion.so/kimyeoul/UI-280cf60eefb680bfb3f8ef96f3809287)
- [게임오버 랭킹 UI 문제](https://www.notion.so/kimyeoul/UI-280cf60eefb6807bb5a4efb484bc0dab)
- [거리 비례 자기장 시스템 구현 문제](https://www.notion.so/kimyeoul/280cf60eefb680c0b63ef60467fcfe4d)
- [자기장 밖 얼어붙는 효과 문제](https://www.notion.so/kimyeoul/280cf60eefb680efbd84cadf7ccd6457)
- [NPC 춤추기 기믹 문제](https://www.notion.so/kimyeoul/NPC-280cf60eefb68071a63bf18478313d6e)
- [애니메이션 이펙트 추가 문제](https://www.notion.so/kimyeoul/280cf60eefb68042a970dae424bde50d)
- [레벨 이동 시 서버 크래시 문제](https://www.notion.so/kimyeoul/280cf60eefb68042ac70ec6cb99e3d79)
- [PlayerList Replication 문제](https://www.notion.so/kimyeoul/PlayerList-Replication-280cf60eefb680b8b5bff8a62de526b0)
- [미니맵 밝기 문제](https://www.notion.so/kimyeoul/280cf60eefb68020925aeadbc7b6c3d9)
- [베리어 아이템 효과 미적용 문제](https://www.notion.so/kimyeoul/280cf60eefb680818137e6a7c136c3c7)
- [아이템 오버랩 이펙트 과도 재생 문제](https://www.notion.so/kimyeoul/280cf60eefb680218676c5cd96a311e8)

<br>

## 👥 역할 

| 담당자 | 담당 역할 |
|-----------|--------|
| 김기탁 | 서버 / 레벨 디자인 / UI / 패키징 |
| 김상범 | 캐릭터(Code, Animation) |
| 김여울 | 캐릭터(BP) / UI 보조 / 노션 / ReadMe / 발표 |
| 성준모 | 기믹(Item) / PPT |
| 정기준| NPC(AI) / 캐릭터 보조 / VFX |
| 정성현 | 서버 / 기믹 보조 / SFX & BGM |

<br>


## ⚙️ 사용한 툴

- [Rider](https://www.jetbrains.com/ko-kr/rider/) – 개발 환경
- [Blender](https://www.blender.org/download/) – 이펙트 메쉬 제작, 3D 모델링 및 애니메이션 수정
- [Flowchart Maker & Online Diagram Software](https://app.diagrams.net/) – Game Flow Chart 제작
- [Suno](https://suno.com/home) – 노래 제작 
- [Pixabay](https://pixabay.com/) – 배경 음악
- [Convertio](https://convertio.co/kr/mp3-wav/) – 사운드 파일 확장자 변경

<br>

## 🎨 Resources
- [Low Poly Bird: Penguin](https://www.fab.com/listings/0133d312-535f-42c7-8ae7-3574ba242ad4)
- [Low Poly Winter Environment 3D Asset Pack](https://www.fab.com/listings/e4463c67-f0a3-44e4-b5ae-746020aea5ef)
- [Quirky Series FREE Pack](https://www.fab.com/ko/listings/db770e79-a950-4bb0-8086-1bc04187fe83)
- [Ultra Dynamic Sky](https://www.fab.com/ko/listings/84fda27a-c79f-49c9-8458-82401fb37cfb)
- [Plain Yellow Arrow & Hand Cursor](https://sweezy-cursors.com/cursor/plain-yellow-arrow-hand/)
- [Emojipedia](https://emojipedia.org/apple) 
- [Luckiest Guy](https://fonts.google.com/specimen/Luckiest+Guy)
- [Frozbite](https://fontmeme.com/ktype/frozbite-font/)
- [Ice &Snow Normal font](https://fonts2u.com/ice-snow-normal.font)
- [Dear뱀뱀해](https://m.blog.naver.com/byfont/223733374529)
- [keyboard typing one short 1](https://pixabay.com/sound-effects/keyboard-typing-one-short-1-292590/)
- [Spacebar Click (Keyboard)](https://pixabay.com/sound-effects/spacebar-click-keyboard-199448/)

<br>

## 🧾 노션
- [Dancing Gueen : 펭귄 슬라이드 휭~](https://www.notion.so/kimyeoul/Dancing-Gueen-265cf60eefb6800ab133e16a41f5bfd5)

<br>

## 🧑‍🤝‍🧑 1조

| 이름 | 이메일 | GitHub |
|------|--------|--------|
| 김기탁 | alaalxk@gmail.com | [@mimmita1032](https://github.com/mimmita1032) |
| 김상범 | wb0soon0@gmail.com | [@Kim0430-sa](https://github.com/Kim0430-sa?tab=repositories) |
| 김여울 | yoohozzy@gmail.com | [@yeoulkim](https://github.com/yeoulkim) |
| 성준모 | sjm7829@naver.com | [@komokomo123](https://github.com/komokomo123?tab=repositories) |
| 정기준 | jkjun1234@gmail.com | [@jkjun1234](https://github.com/jkjun1234?tab=repositories) |
| 정성현 | gussen1221@gmail.com | [@gussen33](https://github.com/gussen33) |