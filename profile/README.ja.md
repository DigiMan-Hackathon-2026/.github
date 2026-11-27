<div align="center">

<img src="https://raw.githubusercontent.com/DigiMan-Hackathon-2026/.github/main/profile/images/kuritomo-banner.png" width="100%" alt="クリとも">

**韓国のクリニックとお薬を、LINEで確認できる日本人旅行者向けチャットボット**

DigiMan Hackathon 2026「ソウルにいる日本人を喜ばせてください」・開発中

<sub>企画と構成は確定前のため、変更される場合があります。</sub>

[한국어](https://github.com/DigiMan-Hackathon-2026/.github/blob/main/profile/README.md) · **日本語**

</div>

## サービス紹介

短い日程でソウルに施術を受けに来る日本人旅行者は、2つの場面で同じ不安を抱えます。「信じて大丈夫？何を聞けばいい？」

- **クリニックに行く前**: SNS広告で見た医院が公式資料に開設届のある所か、外国人患者誘致機関として登録されているかを自分で確かめるのは困難です。
- **施術を受けた後**: 薬局で受け取った薬が何か、何に気をつけるべきかを知るのは簡単ではありません。

クリともは、メニューを選ばず**LINEに写真を送るだけ**で、公式資料を確認して日本語の結果カードでお知らせすることを目指しています。

## 大切にしていること

- おすすめ、評価、診断、違法性の判定はしません。公式資料で事実を確認します。
- 確認できたことと**確認できていないこと**を分けて表示します。
- 確認した事実には出典とデータの基準日を添えます。
- 不確かな結果を「確認済み」として表示しません。
- 最終的な判断は医師と薬剤師にお任せします。

## 利用の流れ

<img src="https://raw.githubusercontent.com/DigiMan-Hackathon-2026/.github/main/profile/images/flow-ja.png" width="100%" alt="利用の流れ">

1. **クリニック確認**: 広告や予約画面を送ると、クリニック安心情報カードが届きます。
2. **お薬を撮影**: 施術後に受け取った薬の袋や箱を撮影して送ります。
3. **お薬の案内**: 成分、効能、注意事項を日本語で受け取ります。
4. **追加の質問**: 「妊娠中でも使えますか？」のように気になることを質問します。

### 結果の表示方法

| 状態 | どんなとき | 表示 |
|---|---|---|
| **確認** | 公式資料で1件に特定できた | 結果カード |
| **候補** | どの病院・薬か曖昧 | 候補を表示して選んでもらう |
| **確認できず** | 不確か、または見つからない | 「確認できませんでした」カード |
| **聞き返し** | 文字が読み取れない | 写真の再送をお願いする |

## レイヤー構成

| Layer | 役割 | リポジトリ | 状況 |
|---|---|---|---|
| **1 対話** | 写真を受け取りカードで返信 | [line-stack](https://github.com/DigiMan-Hackathon-2026/line-stack)（検証ツール） | ボット準備中 |
| **2 認識とマッチング** | どの病院・薬かを特定 | 未公開 | 開発・試験中 |
| **3 医薬品情報** | 薬の説明 | [kusuri](https://github.com/DigiMan-Hackathon-2026/kusuri) | API実装済み、連携前 |
| **3 クリニック情報** | 病院の説明 | 未公開 | 項目確定 |

### Layer 1・対話

ユーザーとの窓口です。LINEボットが写真とメッセージを受け取って後ろのレイヤーに渡し、結果を親しみやすい日本語のカードとボタンで返します。

### Layer 2・認識とマッチング

広告の日本語・英語の病院名や薬の名前が、公式名簿のどれに当たるかを特定します。同じ名前の病院や含有量だけが違う薬が多いため、誤った答えを断定しないことが重要です。

1. 写真の文字をVLM/OCRで読み取ります。
2. ブースティングモデル（LightGBM）が候補を絞り、正しい確率を計算します。
3. LLMが候補を再確認して確定するか、ユーザーが選ぶ候補を表示します。

### Layer 3・医薬品情報

薬が特定されたら、韓国の食品医薬品安全処の医薬品許可情報をもとに説明します。成分は公式データから調べ、効能と注意事項は許可文書を翻訳します。追加の質問には、その薬の文書を検索して回答します（RAG）。

### Layer 3・クリニック情報

| カードの項目 | 出典 |
|---|---|
| 医療機関名、所在地、診療科目、開設日 | 健康保険審査評価院 |
| 外国人患者誘致機関の登録有無 | 保健福祉部 |

誘致機関の登録と日本語対応の可否は別です。施術の品質、通訳の可否、最終的な金額は**確認できていないこと**として表示します。

## リポジトリ

### [kusuri](https://github.com/DigiMan-Hackathon-2026/kusuri)・Layer 3 医薬品情報（非公開）

Python（FastAPI）で作ったお薬案内APIです。

| API | 役割 |
|---|---|
| `/v1/drug/prescription` | 薬の名前の一覧 → 成分と品目コード |
| `/v1/drug/info` | 薬1つ → 成分、効能、注意事項（日本語） |
| `/v1/drug/ask` | 薬についての質問 → 許可文書を根拠に回答 |

LINEボットですぐ使えるTypeScriptの呼び出し・メッセージ変換コード（`clients/line-bot`）も含みます。ベクトル検索にはQdrantと`embeddinggemma`、翻訳と回答にはGeminiを使います。

### [line-stack](https://github.com/DigiMan-Hackathon-2026/line-stack)・Layer 1 開発ツール（非公開）

AIエージェントがLINEボットを作るとき、実際のスマートフォンのLINE画面で返信を確認してから完了を報告するようにするClaude Codeプラグインです。練習用ボット`hello-bot`を含み、Android端末での検証に対応しています。

### [idea](https://github.com/DigiMan-Hackathon-2026/idea)・アイデア集

2026年11月22日の会議でテーマを選ぶためにメンバーが作ったデモ集です。

## Tech Stack

![LINE](https://img.shields.io/badge/LINE_Messaging_API-06C755?logo=line&logoColor=white&style=flat-square) ![Node.js](https://img.shields.io/badge/Node.js-5FA04E?logo=nodedotjs&logoColor=white&style=flat-square) ![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?logo=typescript&logoColor=white&style=flat-square) ![Express](https://img.shields.io/badge/Express-000000?logo=express&logoColor=white&style=flat-square) ![VLM / OCR](https://img.shields.io/badge/VLM_%2F_OCR-555555?&style=flat-square) ![LightGBM](https://img.shields.io/badge/LightGBM-EB5E28?&style=flat-square) ![Claude](https://img.shields.io/badge/Claude-D97757?logo=anthropic&logoColor=white&style=flat-square) ![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white&style=flat-square) ![FastAPI](https://img.shields.io/badge/FastAPI-009688?logo=fastapi&logoColor=white&style=flat-square) ![Qdrant](https://img.shields.io/badge/Qdrant-DC244C?logo=qdrant&logoColor=white&style=flat-square) ![Gemini](https://img.shields.io/badge/Gemini-8E75B2?logo=googlegemini&logoColor=white&style=flat-square)

## Team

| 최도일（チェ・ドイル） | 한시온（ハン・シオン） | 이상무（イ・サンム） | 임종훈（イム・ジョンフン） | 허민영（ホ・ミニョン） | 유예은（ユ・イェウン） |
|:-:|:-:|:-:|:-:|:-:|:-:|
| [@novvvv](https://github.com/novvvv) | [@zionhann](https://github.com/zionhann) | [@sangmu1126](https://github.com/sangmu1126) | [@ImJongHoon](https://github.com/ImJongHoon) |  |  |

> 公開情報を案内するサービスです。診断や服用してよいかの判断は行いません。医師・薬剤師にご相談ください。
