# 引き継ぎ書 — __GAME_NAME__

最終更新: YYYY-MM-DD / 現行バージョン: v1.0.0

---

## 概要

(ゲームの説明をここに書く。元ネタや題材があれば触れる。)

公開URL: https://windom21-cpu.github.io/k-games/__GAME_DIR__/
QRコード: `__GAME_DIR__/qr.png`

---

## ファイル構成

```
k-games/__GAME_DIR__/
├─ index.html   ← ゲーム本体(これだけで動く)
└─ qr.png       ← 公開URLのQRコード
```

---

## 技術スタック

- 純粋な HTML5 + CSS + JavaScript (バニラ JS)
- 描画: (Canvas 2D Context / DOM / SVG など、実装に応じて)
- 入力: (Pointer Events / Keyboard / その他)
- 永続化: (localStorage を使う場合は記載)

ビルド工程なし。`index.html` を編集して push するだけで反映 (1〜2分)。

---

## ゲーム仕様

### 画面レイアウト
(必要に応じて図示)

### コアメカニクス
(ルール・操作)

### 終了条件
(ゲームオーバー条件、クリア条件)

---

## バージョン管理

セマンティックバージョニング(`X.Y.Z`):
- パッチ(Z): バグ修正・パラメータ調整
- マイナー(Y): 機能追加 / レイアウト変更
- メジャー(X): ルールの本質的変更

`index.html` 冒頭の `VERSION` 定数とドキュメント末尾の履歴を同期して更新。

### バージョン履歴

- v1.0.0 — 初版

---

## 開発フロー

```bash
# 1. 編集
$EDITOR /home/sk/デスクトップ/k-games/__GAME_DIR__/index.html

# 2. ローカル確認
xdg-open /home/sk/デスクトップ/k-games/__GAME_DIR__/index.html

# 3. コミット & push
cd /home/sk/デスクトップ/k-games
git add __GAME_DIR__/index.html
git -c user.name='windom21-cpu' -c user.email='sk21.lawyer@gmail.com' \
  commit -m "..."
git push origin main
```

QRコード再生成 (URL が変わった時のみ):

```bash
qrencode -o /home/sk/デスクトップ/k-games/__GAME_DIR__/qr.png -s 10 -m 2 \
  "https://windom21-cpu.github.io/k-games/__GAME_DIR__/"
```

---

## チューニング箇所早見表

(主要パラメータと現在値・意味を一覧化)
