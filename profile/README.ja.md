<div align="center">

<img src="https://raw.githubusercontent.com/DigiMan-Hackathon-2026/.github/main/profile/images/kuritomo-banner.png" width="100%" alt="クリとも">

**写真1枚で韓国のクリニックとお薬を確認できる、日本人旅行者向けLINEボット**

DigiMan Hackathon 2026「ソウルにいる日本人を喜ばせてください」出品作

<sub>ハッカソンで開発中のため、企画と構成は実装の過程で変わる場合があります。</sub>

[한국어](https://github.com/DigiMan-Hackathon-2026/.github/blob/main/profile/README.md) · **日本語**

</div>

## プロジェクトの背景

短い日程でソウルを訪れて肌・美容の施術を受ける日本人旅行者は、来院の前と後の2回、情報の空白に直面します。

- **施術前**: SNS広告や予約ページで見たクリニックが、正式に開設届のある医療機関なのか、保健福祉部に登録された外国人患者誘致機関なのかを、日本語で確かめるのは困難です。
- **施術後**: 薬局で受け取る薬の袋や箱は韓国語だけで、どんな成分で何に気をつけるべきかが分かりにくいです。

クリともは、アプリのインストールやメニュー探しをせずに、いつものLINEトークに広告のスクリーンショットや薬の写真を送るだけで、この問題を解決することを目指します。写真から要点を読み取り、公共データと照合して日本語の結果カードで返します。

## データの原則

医療情報は誤った案内が健康上のリスクにつながるため、推測や拡大解釈を排除するように設計します。

- **おすすめ・評価・判定はしません。** 診断や違法性の判断もせず、健康保険審査評価院・保健福祉部・食品医薬品安全処の公共データにある事実を伝えます。
- **確認できたことと確認できていないことを分けます。** 誘致機関の登録有無は確認しますが、日本語通訳の常駐、施術の品質、最終的な支払額は未確認の項目として表示します。
- **出典と基準日を明示することを原則とします。**
- **曖昧な場合は断定しません。** 同じ名前の病院や含有量だけが違う製品は、候補を示して選んでもらいます。
- **服用と施術の判断は医師と薬剤師に任せます。**

## 利用の流れ

<img src="https://raw.githubusercontent.com/DigiMan-Hackathon-2026/.github/main/profile/images/flow-ja.png" width="100%" alt="利用の流れ">

- **施術前**: クリニックの広告や予約画面を送ると、医療機関名、所在地、診療科目、開設日、外国人患者誘致機関の登録有無をまとめた安心情報カードが届きます。
- **施術後**: 薬の袋や箱を撮影して送ると、成分、効能、注意事項が日本語で届きます。
- **追加の質問**: 「妊娠中でも服用できますか？」のように質問すると、その薬の許可文書を調べて根拠とともに回答します。

応答は確認の度合いによって4つに分かれます。

| 状態 | どんなとき | 応答 |
|---|---|---|
| **確認** | 公式データの1件と一致 | 結果カード |
| **候補** | 似た名前や含有量の違いで複数 | 選択肢を提示 |
| **確認不可** | 名簿にない、または信頼度が低い | 推測せず未確認と案内 |
| **再撮影** | 反射や低解像度で読めない | 写真の再送を依頼 |

## アーキテクチャとリポジトリ

| Layer | 役割 | リポジトリ | 状況 |
|---|---|---|---|
| **1 対話** | ユーザーとの接点 | [line-stack](https://github.com/DigiMan-Hackathon-2026/line-stack)（開発・検証ツール、非公開） | サービス用ボット準備中 |
| **2 認識とマッチング** | どの病院・薬かを特定 | 未公開 | 開発・試験中 |
| **3 医薬品情報** | 薬の説明 | [kusuri](https://github.com/DigiMan-Hackathon-2026/kusuri)（非公開） | API実装済み、連携前 |
| **3 クリニック情報** | 病院の説明 | 未公開 | 実装準備 |

### Layer 1・対話インターフェース

LINEボットがWebhookで写真とメッセージを受け取ってバックエンドに渡し、結果を日本語のカードとボタンで表示します。`line-stack`には、Claude Codeエージェントが実際のAndroid端末のLINE画面でボットの返信を確認してから作業を終えるためのプラグインと、練習用ボット（`hello-bot`）があります。

### Layer 2・認識とマッチング

写真の日本語・英語の病院名や韓国語の薬の名前を、公式名簿の名称に結び付けます。

1. VLM/OCRで写真の文字を読み取ります。
2. LightGBMモデルが候補を絞り、一致する確率を計算します。
3. LLMがスコアと文脈を再確認し、確定・候補提示・確認不可を判定します。

### Layer 3・医薬品情報（`kusuri`）

Python（FastAPI）のAPIサーバーと、LINEボット用TypeScriptクライアント（`clients/line-bot`）で構成されます。

| API | 役割 |
|---|---|
| `/v1/drug/prescription` | 薬の名前の一覧 → 成分と品目コード |
| `/v1/drug/info` | 薬1つ → 効能と注意事項を日本語に翻訳 |
| `/v1/drug/ask` | 追加の質問 → 許可文書を根拠に回答（RAG: Qdrant、`embeddinggemma`、Gemini） |

### Layer 3・クリニック情報

健康保険審査評価院の医療機関情報と保健福祉部の誘致機関登録データを組み合わせて、安心情報カードを作る予定です。

<sub>2026年11月22日のテーマ選定で作ったプロトタイプは<a href="https://github.com/DigiMan-Hackathon-2026/idea">idea</a>リポジトリにあります。</sub>

## 技術スタック

![LINE](https://img.shields.io/badge/LINE_Messaging_API-06C755?logo=line&logoColor=white&style=flat-square) ![Node.js](https://img.shields.io/badge/Node.js-5FA04E?logo=nodedotjs&logoColor=white&style=flat-square) ![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?logo=typescript&logoColor=white&style=flat-square) ![Express](https://img.shields.io/badge/Express-000000?logo=express&logoColor=white&style=flat-square) ![VLM / OCR](https://img.shields.io/badge/VLM_%2F_OCR-555555?&style=flat-square) ![LightGBM](https://img.shields.io/badge/LightGBM-EB5E28?&style=flat-square) ![Claude](https://img.shields.io/badge/Claude-D97757?logo=anthropic&logoColor=white&style=flat-square) ![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white&style=flat-square) ![FastAPI](https://img.shields.io/badge/FastAPI-009688?logo=fastapi&logoColor=white&style=flat-square) ![Qdrant](https://img.shields.io/badge/Qdrant-DC244C?logo=qdrant&logoColor=white&style=flat-square) ![Gemini](https://img.shields.io/badge/Gemini-8E75B2?logo=googlegemini&logoColor=white&style=flat-square)

## チーム

| 최도일（チェ・ドイル） | 한시온（ハン・シオン） | 이상무（イ・サンム） | 임종훈（イム・ジョンフン） | 허민영（ホ・ミニョン） | 유예은（ユ・イェウン） |
|:-:|:-:|:-:|:-:|:-:|:-:|
| [@novvvv](https://github.com/novvvv) | [@zionhann](https://github.com/zionhann) | [@sangmu1126](https://github.com/sangmu1126) | [@ImJongHoon](https://github.com/ImJongHoon) |  |  |
