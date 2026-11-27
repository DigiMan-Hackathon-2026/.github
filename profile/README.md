<div align="center">

<img src="https://raw.githubusercontent.com/DigiMan-Hackathon-2026/.github/main/profile/images/kuritomo-banner.png" width="100%" alt="クリとも">

**한국의 클리닉과 약을 LINE에서 확인하는 일본인 여행객용 챗봇**

DigiMan Hackathon 2026 「서울에 있는 일본인을 기쁘게 해 주세요」 · 개발 중

<sub>기획과 구조는 확정 전이며 바뀔 수 있습니다.</sub>

**한국어** · [日本語](https://github.com/DigiMan-Hackathon-2026/.github/blob/main/profile/README.ja.md)

</div>

## 서비스 소개

짧은 일정으로 서울에서 시술을 받는 일본인 여행객은 두 순간에 같은 불안을 겪습니다. "믿어도 될까, 무엇을 물어야 할까?"

- **클리닉에 가기 전**: SNS 광고에서 본 곳이 공식 자료에 개설 신고된 곳인지, 외국인환자 유치기관으로 등록되어 있는지 직접 확인하기 어렵습니다.
- **시술을 받은 뒤**: 약국에서 받은 약이 무엇인지, 어떤 점을 조심해야 하는지 알기 어렵습니다.

クリとも는 메뉴를 고르지 않고 **LINE에 사진을 보내면** 공식 자료로 확인해 일본어 결과 카드로 알려 주는 것을 목표로 합니다.

## 지키는 원칙

- 추천, 평가, 진단, 위법 판정을 하지 않습니다. 공식 자료로 사실을 확인합니다.
- 확인된 것과 **확인되지 않은 것**을 나눠서 보여 줍니다.
- 확인된 사실에는 출처와 데이터 기준일을 함께 표시합니다.
- 확실하지 않은 결과를 "확인"으로 보여 주지 않습니다.
- 최종 판단은 의사와 약사에게 맡깁니다.

## 이용 흐름

<img src="https://raw.githubusercontent.com/DigiMan-Hackathon-2026/.github/main/profile/images/flow-ko.png" width="100%" alt="이용 흐름">

1. **클리닉 확인**: 광고나 예약 화면을 보내면 클리닉 안심 정보 카드를 받습니다.
2. **약 촬영**: 시술 후 받은 약봉투나 약상자를 찍어 보냅니다.
3. **약 안내**: 약의 성분, 효능, 주의사항을 일본어로 받습니다.
4. **추가 질문**: "임신 중에도 쓸 수 있나요?"처럼 궁금한 것을 물어봅니다.

### 결과를 보여 주는 방식

| 상태 | 언제 | 보이는 것 |
|---|---|---|
| **확인** | 공식 자료에서 하나로 확인됨 | 결과 카드 |
| **후보** | 어느 병원·약인지 애매함 | 후보를 보여 주고 고르게 함 |
| **확인하지 못함** | 확실하지 않거나 찾지 못함 | "확인하지 못했습니다" 카드 |
| **되묻기** | 글자를 읽을 수 없음 | 사진을 다시 요청 |

## 레이어 구성

| Layer | 역할 | 저장소 | 상태 |
|---|---|---|---|
| **1 대화** | 사진을 받고 카드로 답장 | [line-stack](https://github.com/DigiMan-Hackathon-2026/line-stack) (검증 도구) | 봇 준비 중 |
| **2 인식·매칭** | 어느 병원·약인지 찾기 | 업로드 전 | 개발·시험 중 |
| **3 의약품 정보** | 약 설명 | [kusuri](https://github.com/DigiMan-Hackathon-2026/kusuri) | API 구현, 연동 전 |
| **3 클리닉 정보** | 병원 설명 | 업로드 전 | 항목 확정 |

### Layer 1 · 대화

사용자가 만나는 창구입니다. LINE 봇이 사진과 메시지를 받아 뒤의 레이어에 전달하고, 결과를 친근한 일본어 카드와 버튼으로 돌려줍니다.

### Layer 2 · 인식·매칭

광고의 일본어·영문 병원명이나 약 이름이 공식 명단의 어느 항목인지 찾습니다. 같은 이름의 병원이나 함량만 다른 약이 많아, 틀린 답을 확신 있게 내지 않는 것이 중요합니다.

1. 사진에서 VLM/OCR로 글자를 읽습니다.
2. 부스팅 모델(LightGBM)이 후보를 좁히고, 맞을 확률을 계산합니다.
3. LLM이 후보를 다시 확인해 확정하거나, 사용자가 고를 후보를 보여 줍니다.

### Layer 3 · 의약품 정보

약이 정해지면 식품의약품안전처의 의약품 허가정보를 바탕으로 설명합니다. 성분은 공식 데이터에서 찾고, 효능과 주의사항은 허가문서를 번역합니다. 추가 질문에는 그 약의 문서를 검색해 답합니다(RAG).

### Layer 3 · 클리닉 정보

| 카드 항목 | 출처 |
|---|---|
| 의료기관명, 소재지 주소, 진료 분야, 개설일 | 건강보험심사평가원 |
| 외국인환자 유치기관 등록 여부 | 보건복지부 |

유치기관 등록과 일본어 응대 가능 여부는 별개입니다. 시술 품질, 통역 가능 여부, 최종 금액은 **확인되지 않은 것**으로 표시합니다.

## 저장소

### [kusuri](https://github.com/DigiMan-Hackathon-2026/kusuri) · Layer 3 의약품 정보 (비공개)

Python(FastAPI)으로 만든 약 안내 API입니다.

| API | 하는 일 |
|---|---|
| `/v1/drug/prescription` | 약 이름 목록 → 성분과 품목코드 |
| `/v1/drug/info` | 약 하나 → 성분, 효능, 주의사항 (일본어) |
| `/v1/drug/ask` | 약에 대한 질문 → 허가문서를 근거로 답변 |

LINE 봇용 TypeScript 호출·메시지 코드(`clients/line-bot`)가 함께 있습니다. 벡터 검색은 Qdrant와 `embeddinggemma`, 번역과 답변은 Gemini를 씁니다.

### [line-stack](https://github.com/DigiMan-Hackathon-2026/line-stack) · Layer 1 개발 도구 (비공개)

AI 에이전트가 LINE 봇을 만들 때, 실제 휴대폰의 LINE 화면에서 답장을 확인한 뒤에 완료를 보고하도록 만든 Claude Code 플러그인입니다. 연습용 봇 `hello-bot`이 들어 있으며, Android 기기 검증을 지원합니다.

### [idea](https://github.com/DigiMan-Hackathon-2026/idea) · 아이디어 모음

2026년 11월 22일 회의에서 주제를 고르기 위해 팀원들이 만든 데모 모음입니다.

## Tech Stack

![LINE](https://img.shields.io/badge/LINE_Messaging_API-06C755?logo=line&logoColor=white&style=flat-square) ![Node.js](https://img.shields.io/badge/Node.js-5FA04E?logo=nodedotjs&logoColor=white&style=flat-square) ![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?logo=typescript&logoColor=white&style=flat-square) ![Express](https://img.shields.io/badge/Express-000000?logo=express&logoColor=white&style=flat-square) ![VLM / OCR](https://img.shields.io/badge/VLM_%2F_OCR-555555?&style=flat-square) ![LightGBM](https://img.shields.io/badge/LightGBM-EB5E28?&style=flat-square) ![Claude](https://img.shields.io/badge/Claude-D97757?logo=anthropic&logoColor=white&style=flat-square) ![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white&style=flat-square) ![FastAPI](https://img.shields.io/badge/FastAPI-009688?logo=fastapi&logoColor=white&style=flat-square) ![Qdrant](https://img.shields.io/badge/Qdrant-DC244C?logo=qdrant&logoColor=white&style=flat-square) ![Gemini](https://img.shields.io/badge/Gemini-8E75B2?logo=googlegemini&logoColor=white&style=flat-square)

## Team

| 최도일 | 한시온 | 이상무 | 임종훈 | 허민영 | 유예은 |
|:-:|:-:|:-:|:-:|:-:|:-:|
| [@novvvv](https://github.com/novvvv) | [@zionhann](https://github.com/zionhann) | [@sangmu1126](https://github.com/sangmu1126) | [@ImJongHoon](https://github.com/ImJongHoon) |  |  |

> 공개 정보를 안내하는 서비스입니다. 진단이나 복용 가능 여부 판단은 하지 않으니 의사·약사와 상담해 주세요.
