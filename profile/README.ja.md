<div align="center">

<img src="https://raw.githubusercontent.com/DigiMan-Hackathon-2026/.github/main/profile/images/mascot.png" width="120" alt="SubStamp GO mascot">

# SubStamp GO

**ホームの案内板を撮ると「この電車に乗っていいか」を日本語で教えてくれる、ソウル地下鉄の案内サービス**

韓国語の案内板と日本語の地図のあいだで迷わないよう、乗る方向をすぐに判定し、リアルタイムの列車情報で目的地まで案内します。<br>
着いた駅では駅名の由来がつまったスタンプを集め、クエストでソウルのあちこちを巡ります。

DigiMan Hackathon 2026 · テーマ「ソウルにいる日本人を喜ばせてください」

[한국어](https://github.com/DigiMan-Hackathon-2026) · **日本語**

</div>

## Tech Stack

| 分野 | 技術 |
|---|---|
| Frontend | ![Next.js](https://img.shields.io/badge/Next.js-000000?style=flat-square&logo=nextdotjs&logoColor=white) ![PWA](https://img.shields.io/badge/PWA-5A0FC8?style=flat-square&logo=pwa&logoColor=white) |
| OCR | ![Apple Vision](https://img.shields.io/badge/Apple_Vision-000000?style=flat-square&logo=apple&logoColor=white) ![Tesseract.js](https://img.shields.io/badge/Tesseract.js-3C78D8?style=flat-square) |
| Backend · Auth | ![Supabase](https://img.shields.io/badge/Supabase-3FCF8E?style=flat-square&logo=supabase&logoColor=white) ![LINE Login](https://img.shields.io/badge/LINE_Login-06C755?style=flat-square&logo=line&logoColor=white) ![Google Login](https://img.shields.io/badge/Google_Login-4285F4?style=flat-square&logo=google&logoColor=white) |
| Data | ![Seoul Open Data](https://img.shields.io/badge/ソウル市オープンデータ_API-558BCF?style=flat-square) |
| Demo | ![Three.js](https://img.shields.io/badge/Three.js-000000?style=flat-square&logo=threedotjs&logoColor=white) ![Vercel](https://img.shields.io/badge/Vercel-000000?style=flat-square&logo=vercel&logoColor=white) |

> 技術スタックはまだ確定していません。Next.js のモノレポをベースに、OCR を端末の中で処理するか API で処理するかを検討しています。

## Screens

<table>
  <tr>
    <td align="center"><img src="https://raw.githubusercontent.com/DigiMan-Hackathon-2026/.github/main/profile/images/scan.png" width="160"><br><sub>案内板を撮る</sub></td>
    <td align="center"><img src="https://raw.githubusercontent.com/DigiMan-Hackathon-2026/.github/main/profile/images/result.png" width="160"><br><sub>方向の判定</sub></td>
    <td align="center"><img src="https://raw.githubusercontent.com/DigiMan-Hackathon-2026/.github/main/profile/images/ride.png" width="160"><br><sub>乗車中の案内</sub></td>
    <td align="center"><img src="https://raw.githubusercontent.com/DigiMan-Hackathon-2026/.github/main/profile/images/arrive.png" width="160"><br><sub>到着スタンプ</sub></td>
    <td align="center"><img src="https://raw.githubusercontent.com/DigiMan-Hackathon-2026/.github/main/profile/images/stats.png" width="160"><br><sub>きろく</sub></td>
  </tr>
</table>

<sub>テーマ選定段階のモックアップです。実際のサービス画面は変わる可能性があります。</sub>

## Features

| 機能 | 説明 |
|---|---|
| 方向の判定 | ホームの案内板を撮ると、目的地へ向かう方向かどうかを日本語ですぐに教えます。 |
| リアルタイム列車情報 | 実際の運行情報をもとに、次の電車と到着時刻を案内します。 |
| 目的地ごとの乗車案内 | 終着駅の表記や分岐する路線など、まぎらわしい区間でも目的地に合う電車を選びます。 |
| スタンプ | 訪れた駅で、駅名の由来や漢字・日本語の説明がのったスタンプを集めます。 |
| クエスト | 駅のまわりを巡るクエストで、地下鉄での移動を旅の一部にします。 |
| AR案内 | 実装できるかを検討中です。 |

## Team

| 名前 | GitHub |
|---|---|
| 최도일（チェ・ドイル） | [@novvvv](https://github.com/novvvv) |
| 한시온（ハン・シオン） | [@zionhann](https://github.com/zionhann) |
| 이상무（イ・サンム） | [@sangmu1126](https://github.com/sangmu1126) |
| 임종훈（イム・ジョンフン） | [@ImJongHoon](https://github.com/ImJongHoon) |
| 허민영（ホ・ミニョン） | |
| 유예은（ユ・イェウン） | |

## Repositories

| リポジトリ | 説明 |
|---|---|
| [idea](https://github.com/DigiMan-Hackathon-2026/idea) | テーマ選定に使ったアイデアのデモ集（2026.11.22 の会議） |
