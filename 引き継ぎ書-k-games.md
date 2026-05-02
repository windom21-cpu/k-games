# 引き継ぎ書 — K-games (サイト全体)

最終更新: 2026-05-03 / 初版

---

## このファイルの位置づけ

`k-games` リポジトリのトップ引き継ぎ。サイト全体の構成・運用ルール・各ゲームへの索引をまとめている。各ゲーム個別の詳細仕様は `引き継ぎ書-<game>.md` を参照。

新しい Claude セッションを `~/デスクトップ/k-games/` から立ち上げて「引き継ぎを確認して」と言われたら、まずこのファイルを読むこと。

---

## 概要

`windom21-cpu` 個人作のミニゲームをまとめたポータルサイト。すべてブラウザだけで動き、インストール不要。トップページは2000年代の素朴な HTML テーブル風(border の二重線・背景白・テキスト中心)。

- **公開URL(本サイト)**: https://windom21-cpu.github.io/k-games/
- **GitHub リポジトリ**: https://github.com/windom21-cpu/k-games (public)
- **GitHub Pages**: 有効化済み(main ブランチ root から配信)
- **本サイトのQRコード**: ルート `qr.png`(180×180でトップに埋込)

旧 `windom21-cpu/dtt-mini` リポジトリも引き続き公開されている(QRコードや既存リンクの互換維持目的)。**今後の作業の主軸はこの `k-games` リポジトリ**で、`dtt-mini` は触らないこと。

---

## ファイル構成

```
k-games/
├─ index.html                  ← K-games トップページ(一覧表)
├─ qr.png                      ← 本サイトのQRコード
│
├─ dtt/                        ← DTT-mini(タワーディフェンス)
│  ├─ index.html               ← PC/タブレット 振り分けページ(自動判定+手動選択)
│  ├─ pc.html                  ← PC版本体(旧 dtt-mini/index.html を移したもの)
│  ├─ tablet.html              ← タブレット版本体
│  ├─ guide.html               ← Win98 風 攻略ガイド
│  └─ 98.css                   ← guide.html 専用 (jdan/98.css)
│
├─ popins.html                 ← ポピンズ(ブロック崩し)単一HTML
├─ qr-popins.png               ← ポピンズ用 旧QR(dtt-mini 用に焼かれている)
│
├─ shoot/                      ← 的当て
│  ├─ index.html
│  └─ qr.png                   ← shoot 用 旧QR(dtt-mini 用)
│
├─ ketorus/                    ← けとるす(落ちものパズル)
│  ├─ index.html
│  └─ qr.png                   ← ketorus 用 旧QR(dtt-mini 用)
│
├─ cafe-merge/                 ← カフェマージ(物理マージパズル)
│  ├─ index.html
│  └─ qr.png                   ← cafe-merge 用 旧QR(dtt-mini 用)
│
├─ 引き継ぎ書-k-games.md       ← このファイル(サイト全体)
├─ 引き継ぎ書-DTT-mini.md      ← DTT-mini 詳細
├─ 引き継ぎ書-popins.md        ← ポピンズ 詳細
├─ 引き継ぎ書-shoot.md         ← 的当て 詳細
├─ 引き継ぎ書-ketorus.md       ← けとるす 詳細
└─ 引き継ぎ書-cafe-merge.md    ← カフェマージ 詳細
```

⚠ 各ゲーム配下の `qr.png` および `qr-popins.png` は `dtt-mini` 時代に焼いた旧URL用。`k-games/` の新URL用に焼き直したい場合は `qrencode` で生成し直すこと(現状トップ `qr.png` のみ新URLで焼いてある)。

⚠ 個別引き継ぎ書(`引き継ぎ書-<game>.md`)の本文中に書かれている「公開URL」は `dtt-mini` 時代の URL のまま。**本サイト URL は `https://windom21-cpu.github.io/k-games/<game>/` に置き換えて読む**こと。両URLとも現状生きているが、新規作業の主軸は `k-games` 側。

---

## 一覧の並び順

トップページ `index.html` の表示順は以下で固定(ユーザー指定):

1. DTT-mini
2. ポピンズ
3. 的当て
4. けとるす
5. カフェマージ

並べ替えたいときはユーザーに確認してから。

---

## DTT-mini 振り分けページの仕様

`dtt/index.html` は PC版とタブレット版の選択画面。

- 表中に「PC版 / タブレット版 / 攻略ガイド」を3行で並列表示
- JS でタッチデバイス判定: `navigator.maxTouchPoints > 0 || 'ontouchstart' in window`
  - タッチ端末なら **タブレット版**側に「★おすすめ」マーク
  - 非タッチなら **PC版**側に「★おすすめ」マーク
- **自動リダイレクトはしない**(ユーザーが選べるようにする方針)
- 「← K-games 一覧へもどる」リンクで親ページへ

---

## トップページのデザイン規約(2000年代テイスト)

`index.html` および `dtt/index.html` 共通スタイル:

```css
body { background: #ffffff; color: #222222;
       font-family: "MS PGothic", "Hiragino Kaku Gothic ProN", sans-serif; }
a:link    { color: #0033cc; }
a:visited { color: #660099; }

table.frame {
  border-collapse: collapse;
  border: 4px double #000000;   /* 外枠は二重線 */
  background: #ffffff;
  margin: 0 auto;
}
table.frame th,
table.frame td {
  border: 1px solid #000000;    /* 内部セルは細い実線 */
  padding: 8px 14px;
  vertical-align: middle;
}
table.frame th { font-weight: bold; text-align: center; }
```

- 外枠は **`4px double` の二重線**
- 内部セルは **`1px solid`**、`border-collapse: collapse` で隙間ゼロ
- 全体背景は **白(`#ffffff`)** ・ 文字は **黒系(`#222`)**
- ヘッダ行や偶数行に色を付けない(2000年代テイスト + ユーザー希望)
- タイトルは `<h1>★ ◯◯ ★</h1>`、`<center>` で中央寄せ、`<hr width="80%">` で区切り

新しいページを追加する場合もこの規約に揃えること。

---

## 開発フロー

```bash
# 1. 編集
$EDITOR /home/sk/デスクトップ/k-games/<対象>

# 2. ローカル確認(必要なら)
xdg-open /home/sk/デスクトップ/k-games/index.html

# 3. コミット & push
cd /home/sk/デスクトップ/k-games
git add <files>
git commit -m "..."
git push origin main

# 4. 1〜2分後、公開URLで反映確認
```

### git identity

リポジトリのローカル `.git/config` に user.name / user.email を**設定していない**(意図的)。コミット時に `-c` で1回限り渡す:

```bash
git -c user.name='windom21-cpu' -c user.email='sk21.lawyer@gmail.com' commit -m "..."
```

永続的な `git config` 変更はしないこと。

### gh CLI

`gh` は導入済み・`windom21-cpu` でログイン済み(scope: `repo`, `workflow`, `delete_repo`, `gist`, `read:org`)。GitHub Pages の有効化や API 操作は `gh api` を使う。

### QR コード生成

```bash
qrencode -o /home/sk/デスクトップ/k-games/qr.png -s 10 -m 2 \
  "https://windom21-cpu.github.io/k-games/"
```

URL を変えた場合のみ焼き直し。

---

## 運用ルール(重要)

### 引き継ぎ書は明示指示時のみ更新

`引き継ぎ書-*.md` は、ユーザーが明示的に「引き継ぎを更新して」「引き継ぎ書を書いて」と指示したときだけ更新する。バージョンアップやコード変更のたびに自動で書き直さない。

**Why:** バージョン更新のたびに書き直すと、まだ仕様が固まっていない段階では無駄な作業になる。ユーザーは確定したタイミングで自分から指示する。

**How to apply:** v1.0.0 → v1.1.0 のような細かい更新作業中は、引き継ぎ書には触れない。ゲーム本体とコミット/push のみ作業対象とする。「引き継ぎを書いて/更新して」と言われたら更新する。

### 既存ゲームの改修は k-games 側で

今後ゲーム本体(`cafe-merge/`, `ketorus/`, `shoot/`, `popins.html`, `dtt/`)を改修する場合は **`k-games/` 側で編集して push** する。`dtt-mini/` リポジトリは触らない(古いURL/QRの維持用にアーカイブする扱い)。

新作ゲームも `k-games/` 配下に追加する。

---

## 各ゲームの索引

| 表示名 | パス | 引き継ぎ書 | 公開URL |
|---|---|---|---|
| DTT-mini | `dtt/` | `引き継ぎ書-DTT-mini.md` | https://windom21-cpu.github.io/k-games/dtt/ |
| ポピンズ | `popins.html` | `引き継ぎ書-popins.md` | https://windom21-cpu.github.io/k-games/popins.html |
| 的当て | `shoot/` | `引き継ぎ書-shoot.md` | https://windom21-cpu.github.io/k-games/shoot/ |
| けとるす | `ketorus/` | `引き継ぎ書-ketorus.md` | https://windom21-cpu.github.io/k-games/ketorus/ |
| カフェマージ | `cafe-merge/` | `引き継ぎ書-cafe-merge.md` | https://windom21-cpu.github.io/k-games/cafe-merge/ |

---

## 来歴メモ

- 2026-05-03: `k-games` リポジトリを新規作成。`dtt-mini` から5ゲームをコピーし、トップページ(2000年代テイスト)+ DTT 振り分けページ + サイトQRを整備。GitHub Pages 配信開始。旧 `dtt-mini` リポジトリは保存(URL互換のため)。
