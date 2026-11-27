<div align="center">

<img src="https://raw.githubusercontent.com/DigiMan-Hackathon-2026/.github/main/profile/images/kuritomo-banner.png" width="100%" alt="クリとも">

**사진 한 장으로 한국의 클리닉과 처방약을 확인하는 일본인 여행객용 LINE 봇**

DigiMan Hackathon 2026 「서울에 있는 일본인을 기쁘게 해 주세요」 출품작

<sub>현재 해커톤 개발이 진행 중이며, 기획 및 아키텍처는 구현 과정에서 일부 변경될 수 있습니다.</sub>

**한국어** · [日本語](https://github.com/DigiMan-Hackathon-2026/.github/blob/main/profile/README.ja.md)

</div>

## 기술 스택

봇 인터페이스는 LINE Messaging API, Node.js, TypeScript, Express를 기반으로 동작하며, 백엔드와 AI 파이프라인은 Python, FastAPI, VLM/OCR, LightGBM, Qdrant, Gemini를 중심으로 구성했습니다.

![LINE](https://img.shields.io/badge/LINE_Messaging_API-06C755?logo=line&logoColor=white&style=flat-square) ![Node.js](https://img.shields.io/badge/Node.js-5FA04E?logo=nodedotjs&logoColor=white&style=flat-square) ![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?logo=typescript&logoColor=white&style=flat-square) ![Express](https://img.shields.io/badge/Express-000000?logo=express&logoColor=white&style=flat-square) ![VLM / OCR](https://img.shields.io/badge/VLM_%2F_OCR-555555?&style=flat-square) ![LightGBM](https://img.shields.io/badge/LightGBM-EB5E28?&style=flat-square) ![Claude](https://img.shields.io/badge/Claude-D97757?logo=anthropic&logoColor=white&style=flat-square) ![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white&style=flat-square) ![FastAPI](https://img.shields.io/badge/FastAPI-009688?logo=fastapi&logoColor=white&style=flat-square) ![Qdrant](https://img.shields.io/badge/Qdrant-DC244C?logo=qdrant&logoColor=white&style=flat-square) ![Gemini](https://img.shields.io/badge/Gemini-8E75B2?logo=googlegemini&logoColor=white&style=flat-square)

## 프로젝트 배경

짧은 일정으로 서울을 찾아 피부·미용 시술을 받는 일본인 여행객은 방문 전과 후, 두 번의 정보 공백을 마주합니다. 시술 전에는 SNS 광고나 예약 페이지에서 접한 클리닉이 실제로 정식 개설 신고된 의료기관인지, 보건복지부에 등록된 외국인환자 유치기관인지 일본어 환경에서 직접 확인할 방법이 마땅치 않습니다. 시술을 마친 뒤에도 마찬가지입니다. 약국에서 건네받은 약봉투와 상자에는 한국어만 적혀 있어, 내가 먹는 약이 어떤 성분이고 무엇을 조심해야 하는지 파악하기 어렵습니다.

クリとも는 별도의 앱을 설치하거나 복잡한 메뉴를 찾아 들어갈 필요 없이, 평소 쓰는 LINE 채팅방에 광고 캡처나 약 사진을 보내는 것만으로 이 문제를 해결하고자 합니다. 전송된 이미지에서 핵심 정보를 읽어 국내 공공 데이터와 대조한 뒤, 여행객이 바로 이해할 수 있는 일본어 결과 카드로 정리해 돌려줍니다.

## 우리가 지키는 데이터 원칙

의료와 의약품 정보는 잘못된 안내가 곧바로 건강상의 위험으로 이어질 수 있기 때문에, 모델의 추측이나 과잉 해석을 배제하는 방향으로 시스템을 설계했습니다.

クリとも는 특정 병원이나 의약품을 추천하거나 평가하지 않으며, 의료적 진단이나 위법 여부도 판정하지 않습니다. 건강보험심사평가원, 보건복지부, 식품의약품안전처 등 공식 기관의 공공 데이터에 등재된 사실을 전달합니다. 이때 공식 데이터로 확인된 사실과 확인할 수 없는 영역을 명확히 나눠 보여 줍니다. 예를 들어 외국인환자 유치기관 등록 여부는 공식 데이터로 확인해 주지만, 현장의 일본어 통역 상주 여부나 시술 품질, 최종 결제 금액은 확인되지 않은 항목으로 선을 긋습니다.

안내 카드에는 데이터의 출처 기관과 기준일을 함께 밝히는 것을 원칙으로 합니다. 같은 이름의 병원이나 함량만 다른 동일 제품군처럼 매칭 결과가 애매할 때는 임의로 하나를 단정 짓지 않고, 후보를 제시해 사용자가 직접 고르도록 합니다. 판독할 수 없는 사진이나 명단에 없는 항목은 '확인 불가'로 분명하게 안내합니다. 최종적인 복용과 시술 판단은 현장의 의사와 약사에게 맡긴다는 원칙을 모든 응답의 전제로 둡니다.

## 이용 흐름과 작동 방식

<img src="https://raw.githubusercontent.com/DigiMan-Hackathon-2026/.github/main/profile/images/flow-ko.png" width="100%" alt="이용 흐름">

사용자의 여정은 크게 시술 전 클리닉 확인과 시술 후 의약품 확인으로 이어집니다. 클리닉 광고나 예약 확인 화면을 채팅방에 보내면 의료기관명, 소재지 주소, 진료 분야, 개설일, 외국인환자 유치기관 등록 여부가 담긴 안심 정보 카드를 받아볼 수 있습니다. 시술 후 약국에서 받은 약봉투나 약 상자를 촬영해 보내면, 각 약의 주성분과 효능·효과, 복용 시 주의사항이 일본어로 전달됩니다. 안내를 받은 뒤 "임신 중에도 복용할 수 있나요?"처럼 궁금한 점을 물어보면, 해당 의약품의 공식 허가문서를 검색해 근거를 바탕으로 답변합니다.

이 과정에서 시스템은 인식 정확도에 따라 네 가지 상태로 응답을 나눕니다. 공식 데이터의 한 항목과 정확히 매칭되면 바로 결과 카드를 보내고, 유사한 이름이나 함량 차이로 여러 후보가 잡히면 선택지를 띄워 확인을 요청합니다. 공식 명단에서 찾을 수 없거나 신뢰도 기준에 미치지 못하면 추측하지 않고 미확인 카드를 보내며, 빛 번짐이나 낮은 해상도로 글자를 읽을 수 없으면 재촬영을 요청합니다.

## 시스템 아키텍처와 저장소 구성

전체 서비스는 사용자 접점, 엔티티 매칭, 도메인 데이터 처리라는 세 개의 레이어로 역할을 나눠 구축하고 있습니다.

| Layer | 역할 | 저장소 | 상태 |
|---|---|---|---|
| 1 | 대화 인터페이스 | [line-stack](https://github.com/DigiMan-Hackathon-2026/line-stack) (개발·검증 도구, 비공개) | 서비스 봇 준비 중 |
| 2 | 인식 및 엔티티 매칭 | 업로드 전 | 개발·시험 중 |
| 3 | 의약품 정보 | [kusuri](https://github.com/DigiMan-Hackathon-2026/kusuri) (비공개) | API 구현, 연동 전 |
| 3 | 클리닉 정보 | 업로드 전 | 구현 준비 |

### Layer 1 · 대화 인터페이스

사용자가 직접 만나는 LINE 봇입니다. Webhook으로 이미지와 텍스트 메시지를 받아 백엔드로 넘기고, 처리 결과를 직관적인 일본어 Flex Message 카드와 액션 버튼으로 보여 줍니다. 이 레이어의 개발 효율을 높이기 위해 만든 `line-stack` 저장소에는, Claude Code 에이전트가 실제 Android 기기의 LINE 화면에서 봇의 답장을 확인한 뒤에 작업을 완료하도록 하는 개발 플러그인과 테스트용 봇(`hello-bot`)이 담겨 있습니다.

### Layer 2 · 인식 및 엔티티 매칭

이미지 속 일본어·영문 병원명이나 한국어 약품명을 국내 공식 명단의 표준 명칭과 연결하는 핵심 파이프라인입니다. 먼저 VLM과 OCR로 사진에서 텍스트를 추출한 뒤, LightGBM 기반 분류·랭킹 모델로 1차 후보군을 압축하고 매칭 확률을 계산합니다. 마지막으로 LLM이 점수와 문맥을 다시 검토해 단일 확정, 후보 제시, 미확인 여부를 판정합니다. 현재 모델을 조정하고 시험하는 중이며, 안정화 후 별도 저장소로 공개할 예정입니다.

### Layer 3 · 도메인 데이터 처리

확정된 병원과 약의 상세 정보를 만듭니다.

의약품 영역을 담당하는 `kusuri` 저장소는 Python(FastAPI) API 서버와 LINE 봇 연동용 TypeScript 클라이언트(`clients/line-bot`)로 구성되어 있습니다. 식약처 의약품 허가정보를 바탕으로 약품명 목록을 표준 성분과 품목코드로 연결하고(`/v1/drug/prescription`), 약의 효능과 주의사항을 일본어로 번역해 제공합니다(`/v1/drug/info`). 후속 질문(`/v1/drug/ask`)에는 Qdrant 벡터 DB(`embeddinggemma`)와 Gemini를 결합한 RAG 파이프라인으로 허가문서 원문을 근거 삼아 답변합니다.

클리닉 영역은 건강보험심사평가원의 의료기관 기본정보와 보건복지부의 유치기관 등록 데이터를 결합해 응답 카드를 구성합니다. 현재 데이터 구조를 정하고 구현을 준비하고 있습니다.

2026년 11월 22일 초기 기획 회의 때 아이디어를 검증하려고 만든 프로토타입은 [idea](https://github.com/DigiMan-Hackathon-2026/idea) 저장소에 보관되어 있습니다.

## 팀

이 프로젝트는 총 6명의 팀원이 함께 만들어가고 있습니다.

| 최도일 | 한시온 | 이상무 | 임종훈 | 허민영 | 유예은 |
|:-:|:-:|:-:|:-:|:-:|:-:|
| [@novvvv](https://github.com/novvvv) | [@zionhann](https://github.com/zionhann) | [@sangmu1126](https://github.com/sangmu1126) | [@ImJongHoon](https://github.com/ImJongHoon) |  |  |
