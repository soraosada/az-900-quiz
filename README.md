# AZ-900 Azure Fundamentals 問題集

Microsoft Certified: Azure Fundamentals (AZ-900) 対策の問題集アプリ。
単一HTMLファイルで動作し、Googleログインで学習履歴をクラウド同期できる。
AWS CLF-C02 問題集アプリと同じエンジンを流用し、データ（問題・分野・暗記カード）をAZ-900向けに差し替えたもの。

## 収録状況

- **問題数: 75問** — 公式 Skills measured（**2026-07-20版**）に対応させて配分
- 暗記カード: 31サービス（8カテゴリ）
- ※ 公式は実際の過去問を公開していないため、本アプリは公式出題範囲に沿った**オリジナル問題**。本試験網羅（100問超）に向けて随時追加。追加は `QUESTIONS` 配列に同形式で足すだけ。

## 出題範囲と配点比率（AZ-900 公式スキル measured / 2026-07-20版）

| 分野 | 公式比率 | 本アプリの重み | 収録 | 実比率 |
|---|---|---|---|---|
| クラウドの概念 | 25–30% | 28 | 21問 | 28% |
| Azureのアーキテクチャとサービス | 35–40% | 37 | 29問 | 39% |
| Azureの管理とガバナンス | 30–35% | 35 | 25問 | 33% |

> 注: 公式では「ID・アクセス・セキュリティ」（Entra ID/MFA/RBAC/ゼロトラスト/条件付きアクセス/Defender等）は
> **分野②（アーキテクチャとサービス）**に属する。本アプリもこれに準拠（旧: 分野③に配置していたものを修正済み）。

## 機能

- 分野別演習・模擬試験（配点比率に沿って抽出・45分カウントダウン）・弱点復習・暗記カード・統計
- 学習履歴は localStorage に自動保存
- Googleログインで Firestore に自動同期（複数端末で履歴共有）／未ログインでも全機能利用可
- 暗記カードから「関連問題を解く」へジャンプ可能

## 本試験の目安

- 問題数: 40〜60問 / 制限時間: 約45分 / 合格スコア: 700 / 1000（約70%）

## ログイン / クラウド同期について

初期状態では **Googleログインは無効**（`FIREBASE_CONFIG` はプレースホルダー）。
理由: AWS版と同じFirebaseプロジェクトを使うと、Firestoreの `users/{uid}` が共通で
AWS CLFの学習履歴と混ざるため。無効時はログインUIが消え、**localStorageのみで全機能そのまま使える**。

有効化するには **AZ-900専用のFirebaseプロジェクト**を作り、`index.html` 末尾の
`FIREBASE_CONFIG` に `apiKey` / `authDomain` / `projectId` を転記する（手順は `../aws-clf-quiz-web/README.md` と同一）。
Firestore ルールは `firestore.rules` を貼り付ける。localStorage キーは `az900_state_v1`（AWS版と分離済み）。
なお **Googleログインは http(s) 配信でのみ動作**（`file://` 直開きでは不可 → GitHub Pages 等で公開後に利用可）。

## 問題の追加方法

`index.html` の `const QUESTIONS = [ ... ]` に以下の形式で追記する：

```js
{id:"D2-13",domain:2,topic:"トピック名",type:"single",difficulty:2,
question:"問題文",
choices:["選択肢A","選択肢B","選択肢C","選択肢D"],
answer:[1],                       // 正解の選択肢インデックス（0始まり・複数可）
explanation:{correct:"正解の解説",
wrong:[ "Aが誤りの理由", null, "Cが誤りの理由", "Dが誤りの理由" ],  // 正解位置は null
point:"覚え方・ひっかけポイント",
svc:["Azure App Service"]}},      // 暗記カードと紐づくサービス名（任意）
```

`type:"multi"` なら `answer` に2つ以上、`choices` は最大5つまで。
`svc` に書いたサービス名が暗記カードにあれば「関連問題」リンクが自動生成される。
