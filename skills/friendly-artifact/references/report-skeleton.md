# HTMLレポートの骨格とスニペット

Artifact として公開する検証レポート向け。`artifact-design` スキルの色トークン・ダークモード規約はそのまま従い、ここでは **節構成と、指摘を受けやすい部品の作り方** だけ定める。

## 節構成(見出しはメッセージにする)

```html
<title>付着物検出モデル 実データ検証</title>   <!-- 2〜4語の名前。説明は description へ -->

<header>
  <h1>…検証レポート</h1>
  <p class="meta">日付 / 対象データ / 重みファイル / 誰向けか</p>
  <div class="verdict">結論。1〜3文。数値付き。採用/不採用/継続が読める</div>
  <nav>ホーム / 案1 / 案2 / 現行モデル評価 …(ハブ&スポークのとき)</nav>
</header>

<section id="how">     1. この仕組みは何をするか(ゼロから読む人向け) + 用語表
<section id="data">    2. データと条件(件数、学習/held-out、正解の出所)
<section id="setup">   3. 構成(モデル、学習条件。仕様書があれば§番号で参照)
<section id="results"> 4〜6. 結果(現物 + 数値 + 各現物の考察)
<section id="limits">  7. 限界と未確認事項(確実/推定/未確認)
<section id="next">    8. 次のアクション
<section id="appendix">付録: スコープ外の観察、再現手順、決定の経緯
```

## 凡例ブロック(文中の一文にしない)

画像グリッドごとに直上へ置く。`position: sticky` にすると長いグリッドでも見失わない。

```html
<div class="legend" role="note" aria-label="凡例">
  <span><i style="background:var(--c-ch1)"></i>ch1 部品A(付着物込み)</span>
  <span><i style="background:var(--c-ch2)"></i>ch2 構造物上の付着物</span>
  <span><i class="line" style="border-color:#fff"></i>基準状態の固定マスク</span>
  <span><i class="line" style="border-color:var(--c-gt)"></i>準正解の輪郭</span>
  <span><i class="dot"></i>点プロンプト(正例)</span>
</div>
```

```css
.legend{display:flex;flex-wrap:wrap;gap:12px 18px;padding:10px 14px;border:1px solid var(--border);
        border-radius:8px;background:var(--surface-2);font-size:13px;position:sticky;top:env(safe-area-inset-top,0px);z-index:2}
.legend i{display:inline-block;width:14px;height:14px;border-radius:3px;vertical-align:-2px;margin-right:6px}
.legend i.line{width:18px;height:0;border-top:2px solid;border-radius:0;vertical-align:3px}
.legend i.dot{width:10px;height:10px;border-radius:50%;background:var(--c-prompt)}
```

## 原画 + 重畳の並置(画像1件の単位)

```html
<figure class="pair">
  <div><img src="…/orig.jpg" alt="原画 地点A 2021-12-05 12:00"><figcaption>原画</figcaption></div>
  <div><img src="…/overlay.png" alt="ch1+ch2 合成"><figcaption>推論結果(ch1+ch2 合成)</figcaption></div>
  <div class="note">
    <b>地点A 2021-12-05 12:00 · held-out</b>
    <span class="tag">IoU 0.71</span>
    <p>対象の輪郭まで一致。右下の似た色の部材を対象として拾っており、これが誤検出の主因。</p>
  </div>
</figure>
```

```css
.pair{display:grid;grid-template-columns:1fr 1fr minmax(200px,.8fr);gap:10px;align-items:start;margin:14px 0}
.pair img{width:100%;border-radius:6px;background:#000}
.pair figcaption{font-size:12px;color:var(--muted);margin-top:4px}
.pair .note p{margin:6px 0 0;font-size:13px}
@media (max-width:720px){.pair{grid-template-columns:1fr 1fr}.pair .note{grid-column:1/-1}}
```

- 考察(`.note p`)の分量は画像ごとに違ってよい。良い例は一言、問題例は厚く。
- 「学習に使用 / held-out / 別地点」のタグを必ず入れる。

## 対になるものの2カラム(ch1/ch2、案A/案B、Before/After)

```html
<div class="cols2">
  <article><h3>ch1 部品A(付着物込み)</h3><p>作り方・枚数・品質管理</p><img …></article>
  <article><h3>ch2 構造物上の付着物</h3><p>…</p><img …></article>
</div>
```

```css
.cols2{display:grid;grid-template-columns:1fr 1fr;gap:20px}
@media (max-width:720px){.cols2{grid-template-columns:1fr}}
```

## 比較表(ホーム用)

| 方式 | 何をするか(1行) | 主要指標 | 対象なし時の誤検出 | 判定 | 詳細 |
|---|---|---|---|---|---|
| 現行 + 追加学習 | 1ステップのセグを同条件で12ep延長 | IoU 0.56 | 0.2% | **採用** | [ページ] |
| 案1 ROI制約 | 構造物マスク外の検出を捨てる | 効果ゼロ | 変化なし | 不採用 | [ページ] |

- 1行1方式、折り返さない。判定列を太字。
- 「何をするか」列がないと読者は指標を解釈できない。

## 不確実性ラベル

```html
<p><span class="lab sure">確実</span> 重み <code>…_best.pt</code> の val IoU は 0.53(出典: 評価スクリプト出力)。</p>
<p><span class="lab est">推定</span> 薄暮の見逃しは露出不足が主因(状況証拠: 16→18時で検出ゼロ率が急増)。</p>
<p><span class="lab unk">未確認</span> 夜間画像は0枚のため夜間性能は評価不能。確認には夜間の収集が必要。</p>
```

## 最終メッセージ(チャット側)の型

```
結論: …(1〜2文、数値つき)
レポート: https://claude.ai/artifact/…
未確認: …(あれば)
```

作業ログ(何を試して何が失敗したか)は付録に入れ、メッセージには書かない。
