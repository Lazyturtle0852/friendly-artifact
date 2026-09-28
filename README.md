# friendly-artifact

「第三者に見せる成果物」を一発で通る品質で出すための Claude Code スキル。

検証レポート、HTML artifact、PR 説明、仕様書、顧客や上司への回答、ロードマップなど、初見の読者が読んで判断できる形にするための手順・分野別ルール・セルフレビュー表をまとめています。3つの実案件(時系列異常検知、定点カメラ画像のセグメンテーション、植物画像の生育分類)で実際に受けた約55件の手戻り指摘を分析し、根本原因6つに整理したものが土台です。

A Claude Code skill for shipping deliverables (reports, HTML artifacts, PR descriptions, specs, replies to clients) that survive first review. Distilled from ~55 real review comments across three ML/software projects. Instructions are written in Japanese.

## 根本原因6つ

| # | 手戻りの根本原因 |
|---|---|
| 1 | 仕組みの説明を飛ばして結果を出した |
| 2 | 数値だけで現物(画像・グラフ・実出力)を見せなかった |
| 3 | AIが勝手に決めた・省いたことに根拠がない |
| 4 | 結論が埋もれる・重要度順になっていない |
| 5 | 凡例・用語・文書間の整合が崩れている |
| 6 | 指標を信じて現物を見ていない・考察がない |

## インストール

[skills CLI](https://github.com/vercel-labs/skills) を使う場合:

```bash
npx skills add Lazyturtle0852/friendly-artifact --skill friendly-artifact
```

手動で入れる場合は `skills/friendly-artifact/` を丸ごと `~/.claude/skills/` (ユーザー共通) か、リポジトリの `.claude/skills/` (プロジェクト単位) にコピーしてください。

## 使い方

`/friendly-artifact <依頼内容>` で明示的に呼ぶか、「レポートにまとめて」「artifact にして」「PR 出して」「返信を作って」といった依頼で自動的に参照されます。

## 構成

```
skills/friendly-artifact/
├── SKILL.md                      # 手順・書き方ルール・セルフレビュー12項目
└── references/
    ├── domain-rules.md           # 分野別ルール(画像認識、時系列/API、PR、顧客回答、仕様書、計画、実装報告)
    ├── report-skeleton.md        # HTMLレポートの節構成と部品(凡例ブロック、原画+重畳の並置、2カラム、比較表)
    └── feedback-log.md           # 根拠となった指摘の一覧(一般化済み)。新しい指摘はここに追記
```

## 画像認識系レポートの必須ルール(抜粋)

- 原画と推論結果(重畳)を必ず横に並べる。複数チャンネルは1枚に合成した重畳も出す
- 評価画像は全件載せる。良い例だけ抜かない
- 点プロンプト・参照画像など入力側も表示する
- 各画像に「学習に使用 / held-out」のタグと定性考察を付ける
- 凡例は色チップ付きブロックとして画像群の直上に固定する(本文中の一文にしない)
- 指標は「何に対する何か」を書く(参照マスクのコピーとの一致は安定性であって精度ではない)

## ライセンス

MIT
