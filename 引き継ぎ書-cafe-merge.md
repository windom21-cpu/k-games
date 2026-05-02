# 引き継ぎ書 — カフェマージ (cafe-merge)

最終更新: 2026-05-03 / 現行バージョン: v2.3.3

---

## 概要

グルメ・ジャーニー (Tasty Travels) の広告で有名だった「ドリンクを奥にスライドして合体させるミニゲーム」を、カフェドリンクの題材で再現したマージパズル。**スイカゲーム的な物理ベース** で、テーブル上を自由に滑るドリンク同士がぶつかり、同じものは合体、違うものはビリヤード的に押し合う。テーブルは横の壁で跳ね返り、奥の壁では止まる。手前(プレイヤー側)への跳ね返りは絶対起きない。HTML 1 ファイル(CSS / JS インライン)で完結、外部依存なし。

公開URL: https://windom21-cpu.github.io/dtt-mini/cafe-merge/
QRコード: `cafe-merge/qr.png`

---

## ファイル構成

```
dtt-mini/
├─ cafe-merge/
│  ├─ index.html   ← ゲーム本体 (これだけで動く)
│  └─ qr.png       ← 公開URLのQRコード
└─ 引き継ぎ書-cafe-merge.md   ← このファイル
```

---

## 技術スタック

- 純粋な HTML5 + CSS + JavaScript (バニラ JS)
- **描画**: Canvas 2D Context (テーブル + ドリンクの動的描画、DPR対応)
- **入力**: Pointer Events API (タッチ/マウス統一、aim + release)
- **物理**: 自前の簡易2D物理 (摩擦 + 円-円弾性衝突 + 奥向き引力)
- **ドリンク絵**: SVG 文字列 → Blob → `Image` に preload → Canvas に `drawImage`
- **HUD / レシピ図鑑 / オーバーレイ / タイトル**: 通常の DOM
- **永続化**: `localStorage` でハイスコアのみ
- **ホスティング**: GitHub Pages (`main` ブランチ直下、`/cafe-merge/` 配下)

ビルド工程なし。`index.html` を編集して push するだけで反映 (1〜2分)。

---

## ゲーム仕様

### 画面レイアウト

```
┌──────────────────────────────────┐
│ 📋テイクアウト[name][/]  💯 SCORE │ ← HUD (DOM)
├──────────────────────────────────┤
│                                  │
│  ▓▓ 木目枠   ▓▓                  │
│  ▓▓             ▓▓                │ ← bezel (左右 inset 20%)
│  ▓▓  ┃ aim ┃   ▓▓                │
│  ▓▓  ◯ 手前のドリンク  ▓▓        │
│  ▓▓ ╌╌ 危険ライン ╌╌  ▓▓         │
│  ▓▓             ▓▓                │
├──────────────────────────────────┤
│ ☕☕☕☕☕☕☕☕☕☕ レシピ図鑑       │ ← (10 cells DOM)
└──────────────────────────────────┘
```

`100dvh` を使ってモバイル/タブレットのアドレスバー伸縮で見切れないように。

### 物理ベースのコアメカニクス
- 上 (`y=0`) = テーブルの **奥**(プレイヤーから遠い)
- 下 (`y=H`) = **手前**(プレイヤー側)
- プレイヤーは `(W/2, H - 54)` に固定、ガイド常時表示
- **ガイド方向 = ポインター位置(またはドラッグ位置)**、画面に触れている間は方向追従、放した瞬間に発射
- 角度は **真上から ±60°** にクランプ(範囲外は端で止まる)
- 発射力は **固定** (`FIRE_SPEED = 900 px/sec`)
- 摩擦 `FRICTION_K = 1.0` (指数減衰)、加えて **奥への弱い引力** `BACK_PULL = 32 px/sec²` を全ドリンクに常時適用
- 衝突: 円-円、弾性係数 `COLL_E = 0.05` (ほぼ非弾性 → ぴったりくっつく)
- 質量比は drawRadius² (大きいカップは重く動かしにくい)
- **同種が触れた瞬間に合体** → 1段上のランクに、連鎖最大6回
- **手前への跳ね返りは絶対起きない**: 物理ステップの最後に `if (vy > 0) vy = 0` で吸収

### Play area (跳ね返り範囲)
- 画面は左右フル(`100vw`)
- ただし **跳ね返りの壁は左右 20% inset** で、play area = 60% of W
- max 460px キャップ(広い画面で散らかりすぎないように)
- 外側は **木目調の bezel** で「テーブルの縁」を演出
- 衝突壁: 奥(止まる) / 横(`WALL_BOUNCE_SIDE = 0.5` で跳ね返る) / 手前(画面下、止まる)

### 11段階のドリンクツリー (実質 Lv1〜10)
| Lv | 名前 | 概形 |
|---|---|---|
| 1 | エスプレッソ | 小さな白い受皿 + ダークなショット |
| 2 | ドリップコーヒー | マグ + 蒸気 |
| 3 | カフェラテ | 高めのグラスマグ + 二層 |
| 4 | カプチーノ | 大きな受皿 + ハートのラテアート |
| 5 | アイスティー | 縦長グラス + 氷 + ストロー + レモン |
| 6 | アイスカフェモカ | 縦長 + ホイップ + チョコソース + ストロー |
| 7 | フラペチーノ | ドーム蓋付き + ホイップマウンテン |
| 8 | メガパフェドリンク | サンデーグラス + 多色レイヤー + ホイップ4段 + チェリー + スパークル |
| 9 | シェイクタワー | 縦長タンブラー + ミントクリームソーダ + バニラスクープ + チェリー2 + チョコスティック + ストロー |
| 10 | ゴールデンチャリス | 金縁聖杯 + 多色グラデ + 王冠の星 + サイドチェリー + ウェハース + ゴールドスパークル多数 |

(配列インデックス0は未使用、1〜10 が表示対象)

NEXT に出るのは Lv1 が 70%、Lv2 が 23%、Lv3 が 7%。

### サイズ設計 (パフェ基点)
- `baseUnit = Math.min(playW / 4.5, H / 10.5)` で **Lv8 (パフェ) が play area の ~64% を占める**サイズに設定
- `RADIUS_RATIO[lv]` で各 Lv の **drawRadius** (SVG の描画ボックス半径) を決定
- `VISIBLE_FACTOR[lv]` で **collision radius = drawRadius × VISIBLE_FACTOR[lv]** (実際のカップ幅で衝突判定)
- これにより SVG 内のカップ余白(saucer や影)が「padding」として残らず、隣接ドリンクのカップ同士がぴったり接触する
- 質量は drawRadius² ベース(視覚と重さが一致)

`RADIUS_RATIO`: `[0, 1.0, 1.18, 1.38, 1.58, 1.78, 1.98, 2.20, 2.50, 2.85, 3.30]`
`VISIBLE_FACTOR`: `[0, 0.62, 0.46, 0.46, 0.78, 0.46, 0.46, 0.52, 0.58, 0.56, 0.62]`

### 注文 (テイクアウト) システム
本家の「テイクアウト」を踏襲:
- 「**◯◯ を N 個** 作る」のお題が常時1つ表示
- 達成すると即座に次の注文に切り替わり (フラッシュ演出 + コイン報酬)
- 16注文の固定テーブル + 17注文目以降は高難度ループ (Lv7〜Lv10 中心)

| 注文 # | お題 |
|---|---|
| 1 | Lv2 ×1 |
| 2 | Lv3 ×1 |
| 3 | Lv3 ×2 |
| ... | ... |
| 12 | Lv8 ×1 |
| 13 | Lv8 ×2 |
| 14 | Lv9 ×1 |
| 15 | Lv9 ×2 |
| 16 | Lv10 ×1 |
| 17+ | Lv7×2 / Lv8×1 / Lv8×2 / Lv9×1 / Lv9×2 / Lv10×1 をループ |

### スコア
- 配置・合体ごとの基礎点: `+合体先Lv × 12`
- 注文達成で `100 × Lv + 50 × 個数` のコインボーナス

### ハイスコア
- `localStorage['cafe-merge-high-score']` に保存
- タイトル画面に「ハイスコア: NNN」表示
- GAME OVER 画面に SCORE / HIGH を表示、新記録時は **★ NEW RECORD ★** の点滅演出

### 終了条件
**危険ライン (`y = H - 88`)** より下に、低速(`speed < 14`)のドリンクが **0.6 秒以上** 滞在したらゲームオーバー。
タイマーは安全状態に戻ると徐々に減る (誤爆しにくい設計)。

---

## 入力

### タッチ / マウス (Pointer Events 統一)
- **画面のどこでも `pointerdown` でガイド開始**(以降ポインタ位置を方向として追従)
- `pointerup` で **発射** (リリース = タップ)
- 距離は無関係(発射力固定)、方向のみ
- `pointerdown` 時にプレイヤードリンクは少し傾く演出
- ガイドは常時表示(非アクティブ時は薄め、アクティブ時は濃く扇形のコーン表示)

### キーボード (PC 簡易)
| キー | 動作 |
|---|---|
| Space / Enter / ↑ / W | 現在の角度で発射 |
| ← / A | ガイドを左に 0.08 rad 動かす |
| → / D | ガイドを右に 0.08 rad 動かす |

---

## バージョン管理

セマンティックバージョニング:
- パッチ(`X.Y.Z` の Z): バグ修正・パラメータ調整
- マイナー(`X.Y.0`): 機能追加 / レイアウト変更
- メジャー(`X.0.0`): ルールの本質的変更

`index.html` 内の **3箇所** を同期して更新:
1. `<div id="version">vX.Y.Z / build YYYY-MM-DD-NN</div>`
2. JS 冒頭 `const VERSION = 'vX.Y.Z';`
3. JS 冒頭 `const BUILD   = 'YYYY-MM-DD-NN';`

build 文字列は console にも出力 (DevTools のキャッシュ確認用)。

### バージョン履歴
- v1.0.0 — 初版。横レーン7マス・ボタンスライド・HOLD・9ステージミッション・PC/タッチ対応
- v1.1.0 — テーブル奥行き視点、上ドラッグ発射、注文連続処理、レシピ図鑑、HOLD撤去
- v2.0.0 — **物理エンジン化**。Canvas + 自前 2D 物理 (摩擦・弾性衝突・連鎖合体)。aim+release 入力。マスを撤廃して自由位置
- v2.1.0 — UIを明るいカフェ調に刷新。フル画面化(`100vw/100vh`)、ガイド常時表示、角度クランプ ±60°、初速 900 / 摩擦 1.0 で奥まで届く調整
- v2.1.1 — 跳ね返り範囲を内側 10% inset、外側を木目調 bezel で表現
- v2.2.0 — カップサイズ拡大(`playW/14→/11`)、ハイスコア localStorage、`100dvh` でタブレット見切れ解消
- v2.2.1 — 跳ね返り範囲をさらに狭く (`PLAY_INSET_PCT` 0.10→0.16, `MAX_PLAY_WIDTH` 600→520)
- v2.2.2 — 跳ね返り範囲縮小 (0.16→0.20)、**手前への跳ね返りを禁止** (`vy > 0` を吸収)
- v2.2.3 — 衝突弾性 0.65→0.05 でほぼ非弾性、`BACK_PULL = 32 px/sec²` の奥向き引力で着地後も詰める
- v2.2.4 — `VISIBLE_FACTOR` 導入(衝突半径 = 描画半径 × 各Lvのカップ実体率)で見えない padding を撤廃
- v2.2.5 — `baseUnit` 約2倍 (`playW/4.5, H/10.5`) で Lv8 が play area の ~64% を占める迫力に
- v2.3.0 — Lv9 シェイクタワー / Lv10 ゴールデンチャリスを追加 (MAX_LEVEL 8 → 10)、注文難度に新ターゲットを追加、レシピ図鑑を10セル対応
- v2.3.1 — タイトル画面の中身を一時的に削減 (失敗・すぐ revert): サブタイトル/ルールを撤去し flex column 化、max-width 480→360 に
- v2.3.2 — **タイトル/ゲーム画面の同時表示バグ修正**。`#game-screen { display: flex }` の specificity が `[hidden]` 属性に勝っていて、タイトル画面の右側にゲーム画面の HUD が表示されていた。グローバル `[hidden] { display: none !important; }` で解決。サブタイトル/ルールも復活
- v2.3.3 — タイトル文字列「カフェマージ」が幅広端末で折り返していたのを修正。font-size 上限 60→52px、`white-space: nowrap`、max-width 360→420 に

---

## 開発フロー

```bash
# 1. 編集
$EDITOR /home/sk/デスクトップ/dtt-mini/cafe-merge/index.html

# 2. ローカル確認
xdg-open /home/sk/デスクトップ/dtt-mini/cafe-merge/index.html

# 3. バージョン番号を上げる(version文字列・VERSION・BUILDの3箇所)

# 4. コミット & push
cd /home/sk/デスクトップ/dtt-mini
git add cafe-merge/index.html
git commit -m "..."  # git identity 未設定なら -c user.name/email で1回限り渡す
git push origin main

# 5. 1〜2分後、公開URLで反映確認(右下の version で判別)
```

ローカルの git config に user.name/email が設定されていない場合、コミット時に
`git -c user.name='windom21-cpu' -c user.email='sk21.lawyer@gmail.com' commit ...` の形で
1回限り渡す(永続的な config 変更はしない)。

QRコード再生成 (URL が変わった時のみ):
```bash
qrencode -o /home/sk/デスクトップ/dtt-mini/cafe-merge/qr.png -s 10 -m 2 \
  "https://windom21-cpu.github.io/dtt-mini/cafe-merge/"
```

---

## コード構造の見取り図

`index.html` 内 JS 部 (約 700 行):

```
定数
├─ VERSION / BUILD
├─ MAX_LEVEL = 10 / ANGLE_LIMIT = π/3
├─ DRINKS                ← 各Lvの表示名
├─ RADIUS / RADIUS_RATIO ← 描画半径(動的計算)
├─ VISIBLE_FACTOR        ← 衝突半径 = 描画半径 × ここの値
├─ FIRE_SPEED / FRICTION_K / MIN_SPEED / WALL_BOUNCE_SIDE / COLL_E / BACK_PULL
├─ DANGER_PAD / PLAYER_Y_PAD
└─ PLAY_INSET_PCT / MAX_PLAY_WIDTH

SVG
└─ svgFor(level)         ← Lv1〜10 の SVG 文字列を返す

Image preload
└─ preloadDrinks()       ← SVG → Blob → Image を MAX_LEVEL ぶん

注文ジェネレータ
└─ generateOrder(orderNum)  ← 16注文の固定 tier + 17以降のループ pool

ハイスコア (localStorage)
├─ HS_KEY
├─ getHighScore()
├─ trySaveHighScore(s)
└─ refreshHsDisplay()

クラス
└─ Drink                 ← {level, x, y, vx, vy, drawRadius, radius, mass, id}

ゲーム状態 (グローバル)
├─ drinks[]              ← 物理上のドリンクすべて
├─ player                ← {x, y, level, cooldown}
├─ nextLevel             ← 次の手元ドリンクLv
├─ aim / aiming / aimPointerId
├─ score / orderNum / order / unlocked / dangerTimer / running / lastTime
├─ wallLeft / wallRight  ← play area の壁位置
└─ mergeFlashes[]        ← 合体時のスパークル {x, y, t}

ライフサイクル
├─ startGame()           ← 全リセット & タイトル→ゲーム
├─ nextOrder()
├─ gameOver()            ← ハイスコア保存 + オーバーレイ
└─ resizeCanvas()        ← DPR対応 + recomputeBounds + recomputeRadii

寸法計算
├─ recomputeBounds()     ← wallLeft/wallRight を PLAY_INSET_PCT + MAX_PLAY_WIDTH から
└─ recomputeRadii()      ← baseUnit = min(playW/4.5, H/10.5) から RADIUS[lv] を生成

物理
├─ physicsStep(dt)       ← 速度→位置反映、摩擦、奥向き引力、壁、衝突、合体、手前バウンス吸収
├─ resolveCollisions()   ← 全ペアの円衝突を2パス
└─ doOneMerge()          ← 1組の合体を実行 (true=合体あり)

描画
├─ render()              ← bezel、play area、パース、危険ライン、ドリンク、スパークル、ガイド、プレイヤー、NEXT
├─ drawDrinkAt(level,x,y,scale=1)  ← Image を draw radius で描画
├─ drawAimGuide()        ← 扇形コーン + 矢印
└─ drawNextPreview()     ← 右下に小さく次のドリンク

入力
├─ Canvas pointerdown/move/up/cancel ← aim → release で発射
├─ keydown: Space/Enter/↑/W = fire、←/→ = aim 微調整
└─ start-btn / オーバーレイのボタン

Loop
└─ loop(t)               ← requestAnimationFrame で 60fps 物理 + 描画
```

### 重要関数の挙動

**`fire(vx, vy)`**
- プレイヤー位置の少し上に新 Drink を生成、`drinks[]` に追加
- `player.level` を `nextLevel` に、`nextLevel` を新ランダムに
- cooldown 0.18s 開始

**`physicsStep(dt)`**
1. 速度→位置を `dt` で積分
2. 摩擦は指数減衰: `v *= exp(-FRICTION_K * dt)`
3. `MIN_SPEED` 以下なら 0 にゼロイング
4. 全ドリンクに **奥向き引力** `vy -= BACK_PULL * dt` を加算
5. 壁判定で位置クリップ + 反発係数を適用 (奥/手前は止める、横は跳ね返す)
6. `resolveCollisions()` で全ペアの円-円衝突を解消
7. `doOneMerge` を最大 6 反復で連鎖
8. **手前バウンス吸収**: `if (vy > 0) vy = 0`

**`Drink` のサイズ系**
- `this.drawRadius = RADIUS[level]` — SVG の描画ボックス半径
- `this.radius = RADIUS[level] * VISIBLE_FACTOR[level]` — 物理衝突半径 (見えてるカップ幅)
- `this.mass = this.drawRadius²` — 視覚サイズ ≒ 重さ

**`checkGameOver(dt)`**
- `y + radius * 0.4 > DANGER_Y` かつ 速度 14 未満なら `bad = true`
- bad なら dangerTimer 加算、0.6s 超えで gameOver
- bad でなければ dangerTimer を徐々に減らす

---

## 既知の制限・改善候補

- ハイスコアは1件のみ保存(直近のベスト)
- BGM・効果音なし
- 物理ステップは固定 FPS 想定。極端に重い端末で `dt > 0.033` になるとクランプして物理が遅くなる
- 衝突処理は O(N²) — ドリンクが ~30 個以上溜まると CPU 負荷増 (普通そこまで増える前にゲームオーバー)
- Lv4 (カプチーノ) は SVG の saucer が大きいため、見た目の visible 幅が Lv5 (アイスティー) より広い場面がある (Lv4 → Lv5 マージで「縮んだ?」と感じる可能性)
- Lv1 (エスプレッソ) と Lv2 (マグ) も VISIBLE_FACTOR の差で Lv2 がわずかに小さく見える瞬間がある
- ドリンクの drawRadius は physics radius より大きいので、壁ぎりぎりだと SVG が bezel に少し被る (clip はしていない)
- `BACK_PULL` は常時かかっているので、奥壁に押し付けられたドリンクは 1 フレームごとに微小 vy がリセットされる (実害なし、性能影響もごく軽微)
- 画面回転で `resize` イベントは発火するが、既存ドリンクの位置は再配置しない (それなりに自然に追従はする)
- レシピ図鑑(下部バー)はタップで詳細を見るなどの機能はなし

## 実装の落とし穴 (踏み抜いたメモ)

- **`[hidden]` 属性は ID セレクタの `display: ...` に負ける**。`#title-screen { display: flex }` のような ID 付き display 指定をしている要素には、HTML の `hidden` 属性をつけても `display: none` が適用されない。これに気付かず title-screen と game-screen が同時に body の flex 内で並んで表示され、「右側に HUD が見える」状態になっていた。グローバルに `[hidden] { display: none !important; }` を入れて吸収済み。今後 ID + display を持つ要素を hide したい場合はこの仕組みに依存していい
- **タイトル h1 のフォントが clamp() で大きくなる**ぶん、コンテナ max-width とのバランスを取らないと折り返す。`white-space: nowrap` を入れている

---

## チューニング箇所早見表

- **発射スピード** → `FIRE_SPEED` (現 900)
- **滑り具合 (摩擦)** → `FRICTION_K` (現 1.0、大きいほど早く止まる)
- **奥への引力** → `BACK_PULL` (現 32px/sec²、密着の強さ)
- **跳ね返り具合 (横)** → `WALL_BOUNCE_SIDE` (現 0.5)
- **衝突弾性** → `COLL_E` (現 0.05、0=完全非弾性)
- **危険ラインの位置** → `DANGER_PAD` (現 88、画面下からの距離)
- **危険判定の猶予** → `checkGameOver()` 内 `dangerTimer > 0.6`
- **ドリンク全体のサイズ** → `recomputeRadii()` 内 `Math.min(playW/4.5, H/10.5)`
- **レベル別比率** → `RADIUS_RATIO[]` 配列
- **見えるカップ幅 (パッキング)** → `VISIBLE_FACTOR[]` 配列
- **NEXT 出現確率** → `randomDrink()` の閾値
- **注文の難度カーブ** → `generateOrder()` の `tiers` / `pool`
- **角度の制限** → `ANGLE_LIMIT` (現 `π/3` = ±60°)
- **連射クールダウン** → `fire()` 内 `player.cooldown = 0.18`
- **連鎖合体回数上限** → `physicsStep()` 内 `for (let iter = 0; iter < 6; iter++)`
- **play area の幅** → `PLAY_INSET_PCT` (現 0.20) と `MAX_PLAY_WIDTH` (現 460)
