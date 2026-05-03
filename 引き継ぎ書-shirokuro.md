# 引き継ぎ書 — しろくろ

最終更新: 2026-05-03 / 現行バージョン: v1.0.0

---

## 概要

8×8 の盤で黒と白の石を打って挟み、ひっくり返していく定番のリバーシ(オセロ)。CPU対戦(3段階の強さ)と2人プレイの両モードに対応。PC・スマートフォン両対応。

公開URL: https://windom21-cpu.github.io/k-games/shirokuro/
QRコード: `shirokuro/qr.png`

---

## ファイル構成

```
k-games/shirokuro/
├─ index.html   ← ゲーム本体(これだけで動く)
└─ qr.png       ← 公開URLのQRコード
```

---

## 技術スタック

- 純粋な HTML5 + CSS + JavaScript (バニラ JS)
- 描画: DOM(8×8 の `<div class="cell">`、各セルに `<div class="disc">`)
- レイアウト: CSS Grid(`grid-template-columns: repeat(8, 1fr)` + `aspect-ratio: 1/1`)
- 入力: クリック / タップ(`click` イベント)
- アニメ: CSS keyframe(配置時のスケールイン) + transition(石の色反転)
- 永続化: なし(リロードでリセット)

ビルド工程なし。`index.html` を編集して push するだけで反映 (1〜2分)。

---

## ゲーム仕様

### 画面レイアウト

```
[← K-games 一覧へもどる]            v1.0.0
                ★ しろくろ ★
[●黒: 02]                  [○白: 02]
        手番: ● 黒 (あなた)
┌──────────────────────┐
│                      │
│    8×8 オセロ盤       │
│                      │
└──────────────────────┘
[モード▼] [あなた▼] [強さ▼] [最初から]
```

- 手番側のスコアパネルは黄色の枠でハイライト
- 候補マスは盤面に薄い白丸で表示(2人プレイ時、CPU戦時は自分の手番のみ)
- 石を置くと配置アニメ(0.2s scale-in)、ひっくり返る石は色 transition(0.3s)

### コアメカニクス

- 標準オセロ(リバーシ)ルール:8×8 盤、初期配置は中央2×2 に W-B / B-W
- 黒先手。クリック/タップで合法手の位置に石を置く
- 自分と相手の石で挟まれた相手の石を全方向(8方向)で反転
- 合法手がない手番は自動的にパス。両者パスでゲーム終了
- 終局時は石の多い方の勝ち。同数なら引き分け

### モード

| モード | 説明 |
|---|---|
| CPUと対戦 | 自分の色(黒=先手 / 白=後手)を選択。CPU の強さも選択 |
| 2人プレイ | 1台で交互に。候補マスは常に表示 |

### CPU 強さ

| 段階 | 内部実装 |
|---|---|
| よわい | 合法手から完全ランダム |
| ふつう | `flips数 + WEIGHTS[r][c]` の最大手をランダム選択(同点時) |
| つよい | depth-3 の minimax(negamax + α-β pruning)。評価は WEIGHTS の合計差 + 合法手数差(モビリティ) |

### 終了条件

両者ともに合法手がなくなった時点で終了(盤が埋まる場合と、片方0個になる場合あり)。

---

## バージョン管理

セマンティックバージョニング(`X.Y.Z`):
- パッチ(Z): バグ修正・パラメータ調整(難易度の重み微調整など)
- マイナー(Y): 機能追加(初手記録、勝敗ログ、難易度追加など)
- メジャー(X): ルール本質変更(盤サイズ可変、ルール変種など)

`index.html` 冒頭の `VERSION` 定数(右上に表示)とドキュメント末尾の履歴を同期して更新。

### バージョン履歴

- v1.0.0 — 初版。CPU(3段階)/ 2人プレイ、候補ヒント、パス自動処理、終局表示、モバイル対応

---

## 開発フロー

```bash
# 1. 編集
$EDITOR /home/sk/デスクトップ/k-games/shirokuro/index.html

# 2. ローカル確認(http越しでないとファビコン等が効かない)
cd /home/sk/デスクトップ/k-games && python3 -m http.server 8000
# → http://localhost:8000/k-games/shirokuro/ (絶対パス参照のため /k-games/ プレフィックス前提)
# 単体表示だけならファイルを直接開いてもOK:
xdg-open /home/sk/デスクトップ/k-games/shirokuro/index.html

# 3. コミット & push
cd /home/sk/デスクトップ/k-games
git add shirokuro/index.html
git -c user.name='windom21-cpu' -c user.email='sk21.lawyer@gmail.com' \
  commit -m "..."
git push origin main
```

QRコード再生成(URL が変わった時のみ):

```bash
qrencode -o /home/sk/デスクトップ/k-games/shirokuro/qr.png -s 10 -m 2 \
  "https://windom21-cpu.github.io/k-games/shirokuro/"
```

---

## チューニング箇所早見表

| 項目 | 場所 | 現在値 | 意味 |
|---|---|---|---|
| AI 位置評価重み | `WEIGHTS` (CSS下の `<script>` 冒頭) | 角=100, X-square=-50, ... | minimax / greedy 共通の盤位置価値 |
| ハード探索深度 | `aiMove()` の `negamax(..., 2, ...)` | 2 (= 合計3-ply 先読み) | 大きくすると強くなるがモバイルで重い |
| モビリティ係数 | `evalForPlayer()` 末尾 `* 3` | 3 | 合法手数差の重み |
| CPU 思考前ディレイ | `scheduleAI()` の `delay` | 通常450ms / パス時900ms | 早すぎると瞬殺感、遅すぎるとイラつく |
| 盤面サイズ上限 | CSS `.board { width: min(95vw, 540px) }` | 540px | 大画面での最大盤面 |
| 配置アニメ | CSS `@keyframes place` | 0.20s scale 0.2→1.0 | 石を打った時のポップ感 |
| 反転 transition | CSS `.disc { transition: background-color 0.30s }` | 0.30s | ひっくり返るスピード |
| 盤面色 | CSS `--board-bg: #2f7a4f` | 緑 | フェルト風の緑色 |
