<div align="center">

<img src="https://raw.githubusercontent.com/DigiMan-Hackathon-2026/.github/main/profile/images/kuritomo-banner.png" width="100%" alt="クリとも">

**韓国のクリニックとお薬を、LINEで確認できる日本人旅行者向けチャットボット**

DigiMan Hackathon 2026「ソウルにいる日本人を喜ばせてください」・開発中

[한국어](https://github.com/DigiMan-Hackathon-2026/.github/blob/main/profile/README.md) · **日本語**

</div>

## 利用の流れ

<img src="https://raw.githubusercontent.com/DigiMan-Hackathon-2026/.github/main/profile/images/flow-ja.png" width="100%" alt="利用の流れ">

## 構成

| Layer | 役割 | リポジトリ | 状況 |
|---|---|---|---|
| **1 対話** | メッセージと写真を受け取り返信するLINEボット | [line-stack](https://github.com/DigiMan-Hackathon-2026/line-stack)（検証ツール） | ボット準備中 |
| **2 認識とマッチング** | 写真の文字を読み取り → ブースティングモデルで候補抽出 → LLMで確認 | 未公開 | 開発・試験中 |
| **3 医薬品情報** | 成分・効能・注意事項の案内、追加の質問への回答 | [kusuri](https://github.com/DigiMan-Hackathon-2026/kusuri) | API実装済み、連携前 |
| **3 クリニック情報** | 医療機関名、所在地、診療科目、開設日、外国人患者誘致機関の登録有無 | 未公開 | 項目確定 |

<sub>line-stackとkusuriは非公開リポジトリです。テーマ選定用のデモは<a href="https://github.com/DigiMan-Hackathon-2026/idea">idea</a>にあります。</sub>

## Tech Stack

![LINE](https://img.shields.io/badge/LINE_Messaging_API-06C755?logo=line&logoColor=white&style=flat-square) ![Node.js](https://img.shields.io/badge/Node.js-5FA04E?logo=nodedotjs&logoColor=white&style=flat-square) ![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?logo=typescript&logoColor=white&style=flat-square) ![Express](https://img.shields.io/badge/Express-000000?logo=express&logoColor=white&style=flat-square) ![VLM / OCR](https://img.shields.io/badge/VLM_%2F_OCR-555555?&style=flat-square) ![Boosting](https://img.shields.io/badge/Boosting_Model-EB5E28?&style=flat-square) ![Claude](https://img.shields.io/badge/Claude-D97757?logo=anthropic&logoColor=white&style=flat-square) ![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white&style=flat-square) ![FastAPI](https://img.shields.io/badge/FastAPI-009688?logo=fastapi&logoColor=white&style=flat-square) ![Qdrant](https://img.shields.io/badge/Qdrant-DC244C?logo=qdrant&logoColor=white&style=flat-square) ![Gemini](https://img.shields.io/badge/Gemini-8E75B2?logo=googlegemini&logoColor=white&style=flat-square)

## Team

| 최도일（チェ・ドイル） | 한시온（ハン・シオン） | 이상무（イ・サンム） | 임종훈（イム・ジョンフン） | 허민영（ホ・ミニョン） | 유예은（ユ・イェウン） |
|:-:|:-:|:-:|:-:|:-:|:-:|
| [@novvvv](https://github.com/novvvv) | [@zionhann](https://github.com/zionhann) | [@sangmu1126](https://github.com/sangmu1126) | [@ImJongHoon](https://github.com/ImJongHoon) |  |  |

> 公開情報を案内するサービスです。診断や服用してよいかの判断は行いません。医師・薬剤師にご相談ください。
