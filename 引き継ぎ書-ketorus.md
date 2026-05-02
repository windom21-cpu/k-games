# 引き継ぎ書 — けとるす (ketorus)

最終更新: 2026-05-02 / 現行バージョン: v1.4.0

---

## 概要

テトリス風の落ちものパズル。タイトル画面にやかん(Yakan)のSVGを配置していて、ネーミング「けとるす(KETORUS)」もテトリス × ケトル(やかん)のかけ合わせ。HTML 1 ファイル(CSS / JS インライン)で完結、外部依存なし。

公開URL: https://windom21-cpu.github.io/dtt-mini/ketorus/
QRコード: https://windom21-cpu.github.io/dtt-mini/ketorus/qr.png

---

## ファイル構成

```
dtt-mini/
├─ ketorus/
│  ├─ index.html   ← ゲーム本体(これだけで動く)
│  └─ qr.png       ← 公開URLのQRコード
└─ 引き継ぎ書-ketorus.md   ← このファイル
```

ローカル作業用クローン: `/home/sk/デスクトップ/dtt-mini/`

なお、ketorus は元々 `windom21-cpu/ketorus` 単独リポジトリだったが、2026-05-02 に `git subtree add` で dtt-mini に取り込み、旧リポジトリは削除済み。履歴はマージコミット `085c276` 以前を辿れば残っている。

---

## 技術スタック

- 純粋な HTML5 + CSS + JavaScript (バニラ JS)
- 描画: `<canvas>` + 2D Context (盤面 + HOLD/NEXTプレビューの計3キャンバス)
- 永続化: なし(ハイスコアは保存していない)
- ホスティング: GitHub Pages (`main` ブランチ直下、`/ketorus/` 配下)

ビルド工程なし。`index.html` を編集して push するだけで反映 (1〜2分)。

---

## ゲーム仕様

### ルール
- 10列 × 20行の盤面、標準7種テトリミノ (I / O / T / S / Z / J / L)
- 7-bag ランダマイザ (バッグに7種を入れてシャッフル、空になったら補充)
- ライン消去でスコア加算 (1=100, 2=300, 3=500, 4=800、いずれもレベル倍率)
- 10ライン消すごとにレベル+1、落下間隔が短縮 (`max(80, 800 - (level-1) * 70)` ms)
- ピース固定時に既存ブロックと重なっていたらゲームオーバー

### 操作系
ピースは盤面上端でスポーンし、自動で1マスずつ落下。プレイヤーは:
- 左右移動
- ソフトドロップ (1マスずつ早送り、+1点)
- ハードドロップ (即着地、距離 × 2点)
- 回転 (右回転、簡易ウォールキック付き)
- ホールド (1ミノを保管、再使用可。1ピースにつき1回のみ)

### 落下速度テーブル
レベル毎の `dropInterval` (ms):
- Lv1: 800 / Lv2: 730 / Lv3: 660 / ... / Lv11+: 80 (下限)

---

## キー / タップ操作

### キーボード
| キー | 動作 |
|---|---|
| ← / → | 左右移動 |
| ↓ | ソフトドロップ |
| ↑ または X | 回転 |
| Space | ハードドロップ |
| Shift または C | ホールド |
| Esc | 一時停止 / 再開 |

### タッチ (コントローラー風レイアウト)
画面下部:
- **左クラスタ (十字キー)**: ◀(縦長)・▶(縦長)・▼ 中下
- **右クラスタ (アクション)**: HOLD(緑) / ⤓ハードドロップ(赤) / ⟳回転(青)

加えて **盤面の任意の場所をタップで回転** (v1.4.0で追加)。

長押し連射対応: ◀ / ▶ / ▼ は押しっぱなしで 110ms 間隔で連射。

タッチ判定は `(pointer: coarse), (max-width: 700px)` のメディアクエリで自動表示。

---

## レイアウト構造

```
┌──────────────────────────────────┐
│  SCORE  LINES  LV    (info-bar)  │ ← 高さ 44px の細い1行
├──────────────────────────────────┤
│ [HOLD]                  [NEXT]   │ ← 盤面コーナーに半透明オーバーレイ
│                                  │
│         BOARD (1:2 aspect)       │ ← 縦長の盤面。max-height/widthで枠内最大化
│         square cells             │
│                                  │
├──────────────────────────────────┤
│  ◀ ▼ ▶          HOLD ⤓ ⟳         │ ← タッチ時のみ表示
└──────────────────────────────────┘
```

ポイント:
- 盤面は `aspect-ratio: 1/2` で正方形マスを維持。`#board-frame` が盤面と同サイズで、HOLD/NEXTオーバーレイ・ポーズボタン・ゲームオーバーオーバーレイの位置基準になる
- HOLD/NEXTは `pointer-events: none`。盤面タップで回転が効くようにするため
- 情報バーから HOLD/NEXT を撤去して 72px → 44px に圧縮、その分盤面が伸びる (v1.3.0)

---

## バージョン管理

セマンティックバージョニング:
- パッチ(`1.0.X`): バグ修正・パラメータ調整
- マイナー(`1.X.0`): 機能追加 / レイアウト変更
- メジャー(`X.0.0`): ルールの本質的変更

`index.html` 内の **3箇所** を同期して更新する:
1. `<div id="version">v1.X.Y / build YYYY-MM-DD-NN</div>`
2. JS 冒頭 `const VERSION = 'v1.X.Y';`
3. JS 冒頭 `const BUILD   = 'YYYY-MM-DD-NN';` (NN は同日内の連番、キャッシュ判別用)

build 文字列は console にも出力される (ブラウザのキャッシュが古いか即判別できる)。

### バージョン履歴
- v1.0.0 — 7種ミノ・ホールド・ネクスト・ゴースト・ライン消去・キーボード/タッチ操作・タイトル画面(やかんSVG)を実装
- v1.1.0 — タッチ操作をコントローラー風(左十字キー・右アクション)に再構成、情報バーを上部に集約して盤面を縦に最大化
- v1.2.0 — 盤面をブラウザ幅いっぱいに引き伸ばす実験(横長マスになって不自然 → 即時 revert)
- v1.3.0 — HOLD/NEXTを盤面コーナーの半透明オーバーレイ化、情報バーをSCORE/LINES/LVのみのスリムバーに
- v1.4.0 — 盤面の任意の場所をタップで回転できるように

---

## 開発フロー

```bash
# 1. 編集
$EDITOR /home/sk/デスクトップ/dtt-mini/ketorus/index.html

# 2. ローカル確認
xdg-open /home/sk/デスクトップ/dtt-mini/ketorus/index.html

# 3. バージョン番号を上げる(version文字列・VERSION・BUILDの3箇所)

# 4. コミット & push
cd /home/sk/デスクトップ/dtt-mini
git add ketorus/index.html
git commit -m "..."
git push origin main

# 5. 1〜2分後、公開URLで反映確認(右下の version で判別)
```

QRコード再生成 (URLが変わった時のみ):
```bash
qrencode -o /home/sk/デスクトップ/dtt-mini/ketorus/qr.png -s 10 -m 2 \
  "https://windom21-cpu.github.io/dtt-mini/ketorus/"
```

---

## コード構造の見取り図

`index.html` 内 JS 部 (約400行):

```
定数
├─ VERSION / BUILD
├─ COLS=10 / ROWS=20 / CELL=20
├─ COLORS                ← ミノ色マップ
└─ SHAPES                ← 各ミノの4回転状態の cell offset 配列

ゲーム状態 (グローバル)
└─ board / current / next / hold / holdUsed / score / lines / level
   dropInterval / dropTimer / running / paused / bag

7-bag
├─ refillBag()
└─ nextType()

ピース操作
├─ makePiece(type)       ← 初期位置 (3, -1) で生成
├─ cellsOf(piece)        ← 絶対座標の4セル配列を返す
├─ collides(piece)       ← 壁/床/既存ブロックとの衝突判定
├─ tryMove(dx, dy)
├─ tryRotate()           ← 簡易ウォールキック(0,-1,1,-2,2)
├─ softDrop() / hardDrop()
├─ holdPiece()
└─ lockPiece() → clearLines() → spawn()

描画
├─ drawCell(ctx, x, y, color, size)  ← 1マス(立体感つき)
├─ drawBoard()           ← 盤面 + ゴースト + 現在ミノ
└─ drawPreview(ctx, type) ← HOLD/NEXTのプレビュー

ライフサイクル
├─ tick(now)             ← requestAnimationFrame ループ
├─ startGame() / gameOver()
├─ showTitle() / showGame()
└─ setPaused(p)

入力
├─ keydown ハンドラ
├─ #touch-controls の各ボタン (touchstart + click、長押し連射)
├─ 盤面 #board の touchstart/click → 回転 (v1.4.0)
└─ オーバーレイのボタン群
```

---

## 既知の制限・改善候補

- ハイスコア保存なし (`localStorage` 未使用、セッション限り)
- BGM・効果音なし
- 7-bagの先読みは1個のみ (NEXT表示が1ミノ)。3〜5ミノ表示にするとより本格的
- ウォールキックが簡易版 (`[0,-1,1,-2,2]` の水平キックのみ)。SRSを完全実装するとTスピンなどの高度なテクが可能に
- ロックディレイなし (着地直後に即固定)。SRS準拠ならロックディレイ500ms程度を入れたい
- ホールドアイコンが「HOLD」テキスト表示。ハードドロップ「⤓」と並ぶ視覚的整合性を取るならアイコン化を検討
- 盤面コーナーのHOLD/NEXTオーバーレイが上端2〜3マスを覆うので、ピースが高く積み上がると見にくくなる(透過率を下げる、隠す、などの逃げ道は要検討)
- iOS Safariの旧バージョンで `aspect-ratio` と `backdrop-filter` のサポートが弱い場合あり(モダンブラウザ前提)

---

## 編集の進め方 (AI協業時の助言)

- ミノの形状を変えたい → `SHAPES` オブジェクトに4回転状態を `[x, y]` の cell offset 配列で書く。中心位置は SRS の慣習に揃えてある
- 落下速度のカーブを変えたい → `clearLines()` 内 `dropInterval = Math.max(80, 800 - (level - 1) * 70)`
- スコア計算を変えたい → `clearLines()` 内 `[0, 100, 300, 500, 800]` テーブル + `* level`
- 盤面サイズ変更 → `COLS` / `ROWS` を変えるが、`<canvas id="board" width="200" height="400">` も `COLS*CELL` × `ROWS*CELL` に合わせる必要あり
- レイアウトの調整は `#info-bar` / `#board-frame` / `.board-info` / `#touch-controls` のCSSで局所化されている
- 新しい操作を追加 → keydown と `#touch-controls button[data-act="..."]` の両方に登録し、`fire()` の switch に分岐を足す
