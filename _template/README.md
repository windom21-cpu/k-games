# 新作ゲーム追加テンプレート

新しいミニゲームを K-games に追加するときの雛形です。

> ⚠ このディレクトリ名はアンダースコア始まり (`_template/`) なので、GitHub Pages では公開されません。意図通りです。

---

## 使い方(チェックリスト)

### 1. テンプレを新ゲーム名でコピー

```bash
cd ~/デスクトップ/k-games
cp -r _template <新ゲーム名>          # 例: cp -r _template puzzle
mv _template/引き継ぎ書-template.md 引き継ぎ書-<新ゲーム名>.md
```

### 2. プレースホルダを実際の内容に置換

`<新ゲーム名>/index.html` と `引き継ぎ書-<新ゲーム名>.md` の中の以下を一括置換:

| プレースホルダ | 置き換える内容 | 例 |
|---|---|---|
| `__GAME_NAME__` | 表示名 | `パズル` |
| `__GAME_DIR__` | URL用ディレクトリ名 | `puzzle` |
| `__GAME_DESCRIPTION__` | 一文の説明 | `ピースを揃えて消すパズル。` |

VS Code なら Ctrl+Shift+H で置換、ターミナルなら `sed -i` でも可。

### 3. ゲーム本体を実装

`<新ゲーム名>/index.html` の `<style>` と `<script>` を埋めていく。

### 4. K-games トップ `index.html` の一覧表に追記

並び順はユーザー指定(現状: DTT-mini → ポピンズ → 的当て → けとるす → カフェマージ)。新作の挿入位置はユーザーに確認すること。

### 5. QR コードを焼く

```bash
qrencode -o <新ゲーム名>/qr.png -s 10 -m 2 \
  "https://windom21-cpu.github.io/k-games/<新ゲーム名>/"
```

### 6. サイト全体引き継ぎ書の索引に追記

`引き継ぎ書-k-games.md` の「ファイル構成」「各ゲームの索引」表に新ゲームを追加。

### 7. コミット & push

```bash
git add .
git -c user.name='windom21-cpu' -c user.email='sk21.lawyer@gmail.com' \
  commit -m "add: <新ゲーム名>"
git push origin main
```

### 8. 動作確認

1〜2分後 `https://windom21-cpu.github.io/k-games/<新ゲーム名>/` で表示されることを確認。

---

## テンプレ更新時の注意

このテンプレ自体に手を入れた場合、過去に作成された各ゲームには遡及されません。共通スタイル等を変えたいときは、各ゲーム個別に反映が必要です。
