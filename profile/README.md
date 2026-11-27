<div align="center">

<img src="https://raw.githubusercontent.com/DigiMan-Hackathon-2026/.github/main/profile/images/kuritomo-banner.png" width="100%" alt="クリとも">

**한국의 클리닉과 약을 LINE에서 확인하는 일본인 여행객용 챗봇**

DigiMan Hackathon 2026 「서울에 있는 일본인을 기쁘게 해 주세요」 · 개발 중

**한국어** · [日本語](https://github.com/DigiMan-Hackathon-2026/.github/blob/main/profile/README.ja.md)

</div>

## 이용 흐름

<img src="https://raw.githubusercontent.com/DigiMan-Hackathon-2026/.github/main/profile/images/flow-ko.png" width="100%" alt="이용 흐름">

## 구성

| Layer | 하는 일 | 저장소 | 상태 |
|---|---|---|---|
| **1 대화** | 메시지와 사진을 받아 답장하는 LINE 봇 | [line-stack](https://github.com/DigiMan-Hackathon-2026/line-stack) (검증 도구) | 봇 준비 중 |
| **2 인식·매칭** | 사진 글자 읽기 → 부스팅 모델로 후보 추리기 → LLM 확인 | 업로드 전 | 개발·시험 중 |
| **3 의약품 정보** | 성분·효능·주의사항 안내, 추가 질문 답변 | [kusuri](https://github.com/DigiMan-Hackathon-2026/kusuri) | API 구현, 연동 전 |
| **3 클리닉 정보** | 기관명, 주소, 진료 분야, 개설일, 외국인 환자 유치기관 등록 여부 | 업로드 전 | 항목 확정 |

<sub>line-stack과 kusuri는 비공개 저장소입니다. 주제 선정용 데모는 <a href="https://github.com/DigiMan-Hackathon-2026/idea">idea</a>에 있습니다.</sub>

## Tech Stack

![LINE](https://img.shields.io/badge/LINE_Messaging_API-06C755?logo=line&logoColor=white&style=flat-square) ![Node.js](https://img.shields.io/badge/Node.js-5FA04E?logo=nodedotjs&logoColor=white&style=flat-square) ![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?logo=typescript&logoColor=white&style=flat-square) ![Express](https://img.shields.io/badge/Express-000000?logo=express&logoColor=white&style=flat-square) ![VLM / OCR](https://img.shields.io/badge/VLM_%2F_OCR-555555?&style=flat-square) ![Boosting](https://img.shields.io/badge/Boosting_Model-EB5E28?&style=flat-square) ![Claude](https://img.shields.io/badge/Claude-D97757?logo=anthropic&logoColor=white&style=flat-square) ![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white&style=flat-square) ![FastAPI](https://img.shields.io/badge/FastAPI-009688?logo=fastapi&logoColor=white&style=flat-square) ![Qdrant](https://img.shields.io/badge/Qdrant-DC244C?logo=qdrant&logoColor=white&style=flat-square) ![Gemini](https://img.shields.io/badge/Gemini-8E75B2?logo=googlegemini&logoColor=white&style=flat-square)

## Team

| 최도일 | 한시온 | 이상무 | 임종훈 | 허민영 | 유예은 |
|:-:|:-:|:-:|:-:|:-:|:-:|
| [@novvvv](https://github.com/novvvv) | [@zionhann](https://github.com/zionhann) | [@sangmu1126](https://github.com/sangmu1126) | [@ImJongHoon](https://github.com/ImJongHoon) |  |  |

> 공개 정보를 안내하는 서비스입니다. 진단이나 복용 가능 여부 판단은 하지 않으니 의사·약사와 상담해 주세요.
