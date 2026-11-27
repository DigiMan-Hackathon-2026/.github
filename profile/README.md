<div align="center">

<img src="https://raw.githubusercontent.com/DigiMan-Hackathon-2026/.github/main/profile/images/kuritomo-banner.png" width="100%" alt="クリとも">

**한국의 클리닉과 약을 LINE에서 확인하는 일본인 여행객용 챗봇**

DigiMan Hackathon 2026 「서울에 있는 일본인을 기쁘게 해 주세요」 · 개발 중

**한국어** · [日本語](https://github.com/DigiMan-Hackathon-2026/.github/blob/main/profile/README.ja.md)

</div>

## 서비스 소개

한국의 클리닉을 찾는 일본인 여행객은 병원 정보를 한국어로 확인해야 하고, 받아 온 약이 무엇인지 알기 어렵습니다.
クリとも는 방문 전의 클리닉 확인부터 방문 후의 약 안내까지를 LINE 대화 하나로 잇는 것을 목표로 합니다.

## 이용 흐름

<img src="https://raw.githubusercontent.com/DigiMan-Hackathon-2026/.github/main/profile/images/flow-ko.png" width="100%" alt="이용 흐름">

1. **클리닉 확인**: 가려는 의료기관의 정보를 안심 정보 카드로 확인합니다.
2. **약 촬영**: 한국에서 받은 약봉투나 약상자를 찍어 보냅니다.
3. **약 안내**: 약의 성분, 효능, 주의사항을 일본어로 받습니다.
4. **추가 질문**: "임신 중에도 쓸 수 있나요?"처럼 궁금한 것을 물어봅니다.

## 레이어 구성

역할에 따라 세 층으로 나눕니다. LINE 봇이 뒤의 파이썬 서버를 호출하는 구조입니다.

| Layer | 역할 | 저장소 | 상태 |
|---|---|---|---|
| **1 대화** | 사용자와 주고받기 | [line-stack](https://github.com/DigiMan-Hackathon-2026/line-stack) (검증 도구) | 봇 준비 중 |
| **2 인식·매칭** | 어떤 약인지 찾기 | 업로드 전 | 개발·시험 중 |
| **3 의약품 정보** | 약 설명 | [kusuri](https://github.com/DigiMan-Hackathon-2026/kusuri) | API 구현, 연동 전 |
| **3 클리닉 정보** | 병원 설명 | 업로드 전 | 항목 확정 |

### Layer 1 · 대화

사용자가 만나는 창구입니다. LINE 봇 서버(Node.js)가 메시지와 사진을 받아 뒤의 레이어에 전달하고, 결과를 친근한 일본어 메시지와 버튼으로 돌려줍니다.

### Layer 2 · 인식·매칭

사진을 보고 어떤 약인지 정하는 단계이며, 아직 저장소에 올라오지 않았습니다.

1. VLM/OCR로 사진의 글자와 특징을 읽습니다.
2. 부스팅 모델이 후보 약 20개를 고릅니다.
3. LLM이 후보를 확인해 하나를 정하거나, 사용자가 고를 후보를 보여 줍니다.

AI가 없는 약 이름을 지어내지 않도록, 후보를 먼저 좁힌 뒤에 확인합니다.

### Layer 3 · 의약품 정보

약이 정해지면 식품의약품안전처의 의약품 허가정보를 바탕으로 설명합니다. 성분은 공식 데이터에서 찾고, 효능과 주의사항은 허가문서를 번역합니다. 추가 질문에는 그 약의 문서를 검색해 답합니다(RAG).

### Layer 3 · 클리닉 정보

방문할 곳을 판단할 때 필요한 사실을 카드로 보여 줍니다.

- 의료기관명, 소재지 주소, 진료 분야(표시 과목)
- 개설일, 외국인 환자 유치기관 등록 여부

## 저장소

### [kusuri](https://github.com/DigiMan-Hackathon-2026/kusuri) · Layer 3 의약품 정보 (비공개)

Python(FastAPI)으로 만든 약 안내 API입니다.

| API | 하는 일 |
|---|---|
| `/v1/drug/prescription` | 약 이름 목록 → 성분과 품목코드 |
| `/v1/drug/info` | 약 하나 → 성분, 효능, 주의사항 (일본어) |
| `/v1/drug/ask` | 약에 대한 질문 → 허가문서를 근거로 답변 |

LINE 봇에서 바로 쓸 수 있는 TypeScript 호출·메시지 코드(`clients/line-bot`)가 함께 있습니다. 벡터 검색은 Qdrant와 `embeddinggemma`, 번역과 답변은 Gemini를 씁니다.

### [line-stack](https://github.com/DigiMan-Hackathon-2026/line-stack) · Layer 1 개발 도구 (비공개)

AI 에이전트가 LINE 봇을 만들 때, 실제 휴대폰의 LINE 화면에서 답장을 확인한 뒤에 완료를 보고하도록 만든 Claude Code 플러그인입니다. 연습용 봇 `hello-bot`이 들어 있으며, Android 기기 검증을 지원합니다.

### [idea](https://github.com/DigiMan-Hackathon-2026/idea) · 아이디어 모음

2026년 11월 22일 회의에서 주제를 고르기 위해 팀원들이 만든 데모 모음입니다.

## Tech Stack

![LINE](https://img.shields.io/badge/LINE_Messaging_API-06C755?logo=line&logoColor=white&style=flat-square) ![Node.js](https://img.shields.io/badge/Node.js-5FA04E?logo=nodedotjs&logoColor=white&style=flat-square) ![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?logo=typescript&logoColor=white&style=flat-square) ![Express](https://img.shields.io/badge/Express-000000?logo=express&logoColor=white&style=flat-square) ![VLM / OCR](https://img.shields.io/badge/VLM_%2F_OCR-555555?&style=flat-square) ![Boosting](https://img.shields.io/badge/Boosting_Model-EB5E28?&style=flat-square) ![Claude](https://img.shields.io/badge/Claude-D97757?logo=anthropic&logoColor=white&style=flat-square) ![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white&style=flat-square) ![FastAPI](https://img.shields.io/badge/FastAPI-009688?logo=fastapi&logoColor=white&style=flat-square) ![Qdrant](https://img.shields.io/badge/Qdrant-DC244C?logo=qdrant&logoColor=white&style=flat-square) ![Gemini](https://img.shields.io/badge/Gemini-8E75B2?logo=googlegemini&logoColor=white&style=flat-square)

## Team

| 최도일 | 한시온 | 이상무 | 임종훈 | 허민영 | 유예은 |
|:-:|:-:|:-:|:-:|:-:|:-:|
| [@novvvv](https://github.com/novvvv) | [@zionhann](https://github.com/zionhann) | [@sangmu1126](https://github.com/sangmu1126) | [@ImJongHoon](https://github.com/ImJongHoon) |  |  |

> 공개 정보를 안내하는 서비스입니다. 진단이나 복용 가능 여부 판단은 하지 않으니 의사·약사와 상담해 주세요.
