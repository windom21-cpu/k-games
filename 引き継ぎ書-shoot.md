# 引き継ぎ書 — 的当てミニゲーム (shoot)

最終更新: 2026-05-02 / 現行バージョン: v1.2.0

---

## 概要

右から左へ流れてくる的をクリック / タップで撃ち落とす1画面ミニゲーム。
HTML 1 ファイル(CSS / JS インライン)で完結し、外部依存なし。

公開URL: https://windom21-cpu.github.io/dtt-mini/shoot/
QRコード: https://windom21-cpu.github.io/dtt-mini/shoot/qr.png

---

## ファイル構成

```
dtt-mini/
├─ shoot/
│  ├─ index.html   ← ゲーム本体(これだけで動く)
│  └─ qr.png       ← 公開URLのQRコード
└─ 引き継ぎ書-shoot.md   ← このファイル
```

ローカルでの作業用クローン: `/home/sk/デスクトップ/dtt-mini/`
別途、編集テスト用に元ファイルが `/home/sk/デスクトップ/claude/index.html` にもコピーされる(同期は手動コピー)。

---

## 技術スタック

- 純粋な HTML5 + CSS + JavaScript (バニラ JS)
- 描画: `<canvas>` + 2D Context
- 永続化: `localStorage`(ハイスコアのみ)
- ホスティング: GitHub Pages(`main` ブランチ直下、`/shoot/` 配下)

ビルド工程なし。`index.html` を編集して push するだけで反映される(1〜2分)。

---

## ゲーム仕様

### ルール
- 制限時間 30 秒
- 右から的が流れてくる(時間経過で速度・出現頻度ともに上がる)
- タップで撃破。外すと −1 点(下限0)
- 残り3秒以下で背景に巨大な数字(3 / 2 / 1)が薄く表示される

### 的の種類
| 種類 | 見た目 | 動作 | 得点 |
|---|---|---|---|
| 通常 | カラフルな円(ランダム色相) | タップで即破壊 | `Math.max(1, round(50/r * 10))` |
| 爆弾 (🧨) | 黒球+赤い縁+チカチカする導火線 | タップ厳禁。触ると減点+画面振動+赤フラッシュ+スコア赤拡大演出 | `-cfg.bombPenalty`(下限0) |
| 氷 (❄️) | 水色グラデ+雪の結晶 | 1回目タップで停止(白縁取り+パルスのオーラ) → 2回目タップで破壊 | 1点(凍結時) + 通常の2倍(撃破時) |

### 難易度パラメータ(`DIFFICULTIES` オブジェクト)

すべて `index.html` の `DIFFICULTIES` 内で一元管理。

| キー | 表示名 | rMin | rRand | sBase | sRand | sRamp | intvBase | intvMin | intvRamp | tap | bombChance | bombPenalty | iceChance |
|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| easy | やさしい | 26 | 26 | 70 | 110 | 3 | 950 | 380 | 16 | 22 | 0.08 | 4 | 0.10 |
| normal | ふつう | 16 | 24 | 100 | 160 | 5 | 720 | 230 | 20 | 14 | 0.12 | 6 | 0.12 |
| hard | むつかしい | 14 | 18 | 120 | 180 | 6 | 600 | 200 | 22 | 10 | 0.18 | 8 | 0.12 |
| oni | 鬼 | 10 | 14 | 180 | 240 | 10 | 380 | 130 | 28 | 4 | 0.25 | 12 | 0.10 |

- `r` = 的半径(px)。`rMin + rand(0,1) * rRand`
- 速度 = `sBase + rand(0,1)*sRand + (30 - timeLeft) * sRamp`
- 出現間隔(ms) = `max(intvMin, intvBase - (30 - timeLeft) * intvRamp)`
- `tap` = ヒット判定の半径加算(px)。指のタップを許容するためのマージン
- `bombChance` 判定後、爆弾でなければ `iceChance` で氷判定

### 画面遷移

```
[Start]
 └─ 「スタート」→ [Diff]
[Diff]
 ├─ 4ボタン → ゲーム開始(本編)
 └─ 「← もどる」→ [Start]
[本編 30秒]
 └─ 時間切れ → [End]
[End]
 ├─ 「もう一度」→ [Diff]
 └─ 「やめる」 → [Start]
```

各 `.screen` を `#overlay` 内で active 切替。`showScreen('Start' | 'Diff' | 'End' | null)` 関数で制御。null は本編プレイ中(オーバーレイ全体を非表示)。

---

## データ保存

`localStorage` キー: `dtt-mini-shoot:hi:v1`
形式: `{ easy: number, normal: number, hard: number, oni: number }`(難易度ごとのハイスコア)
端末・ブラウザ単位。サーバ側保存なし。

スキーマ変更時はキーの `:v1` を上げる(古いデータは無視される)。

---

## モバイル対応の注意点

過去にハマった点:

1. **下半分が当たらない問題**: `100vh` と `window.innerHeight` のズレで Canvas の CSS サイズと内部ピクセル数が乖離していた。`window.visualViewport` の実寸で `wrap` / `canvas` を直接 px 指定して解決。
2. **的が画面下に見切れる**: アドレスバー伸縮で見える範囲が変動するため、的の出現範囲に上下マージン(`topPad: 56`、`bottomPad: 72`)を設けて安全エリアに収めている。
3. **タップ座標**: `getBoundingClientRect()` の結果と内部 W/H の比率でスケール変換(`getPos()` 関数)。

---

## バージョン管理

セマンティックバージョニング:
- パッチ(`1.0.X`): バグ修正・パラメータ調整
- マイナー(`1.X.0`): 機能追加(新モード、効果音 等)
- メジャー(`X.0.0`): ルールの本質的変更

数字は `index.html` の `<div id="version">` に直書き。変更時は必ず上げる(キャッシュ判別の指標)。

### バージョン履歴
- v1.0.0 — 4難易度・ハイスコア・カウントダウン・爆弾まで実装
- v1.1.0 — 氷の的を追加(2回タップ撃破)
- v1.2.0 — スタート / 難易度選択 / 終了の3画面構造に整理

---

## 開発フロー

```bash
# 1. 編集
$EDITOR /home/sk/デスクトップ/dtt-mini/shoot/index.html

# 2. ローカル確認
xdg-open /home/sk/デスクトップ/dtt-mini/shoot/index.html

# 3. バージョン番号を上げる(<div id="version">)

# 4. コミット & push
cd /home/sk/デスクトップ/dtt-mini
git add shoot/index.html
git commit -m "..."
git push origin main

# 5. 1〜2分後、公開URLで反映確認(右下の version で判別)
```

---

## 既知の制限・改善候補

- 効果音なし(欲しければ `Audio` API を追加)
- スコアランキング(個人内のみ。共有・サーバ保存なし)
- 一時停止ボタンなし(ゲーム中の中断はリロードのみ)
- 氷を凍結したまま放置するとずっと残る(タイムアウトで溶ける挙動を入れるかは要検討)
- iOS Safari の縦横切り替え時、稀に Canvas サイズが追従しないことがある(再起動で解消)

---

## 編集の進め方(AI協業時の助言)

- パラメータ調整(数値変更)は `DIFFICULTIES` オブジェクトを 1 か所いじるだけで全体に反映される
- 新しい的タイプを追加する場合: `spawnTarget()`、`hit()`、`drawTarget()` の3か所を同じ条件分岐パターンで増やす
- 演出の変更は `render()` 内のフラッシュ / シェイク / カウントダウン部分が局所化されているのでそこを触る
- 画面追加は `<div class="screen" id="screenXxx">` を `#overlay` 内に増やし、`showScreen('Xxx')` で呼ぶ
