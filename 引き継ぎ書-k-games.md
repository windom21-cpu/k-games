# 引き継ぎ書 — K-games (サイト全体)

最終更新: 2026-05-04(サイコロ大戦争追加)

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
- **ファビコン**: v3 採用済み(`favicon.svg` + 16/32/48/180/512 PNG)
- **SEO/OGP**: 全ページに meta description / OGP / canonical / `og:image` 整備済み

旧 `windom21-cpu/dtt-mini` リポジトリも引き続き公開されている(QRコードや既存リンクの互換維持目的)。**今後の作業の主軸はこの `k-games` リポジトリ**で、`dtt-mini` は触らないこと。旧リポジトリの全 HTML には `<link rel="canonical">` で k-games 側の対応 URL を正規 URL として宣言済み。

---

## ファイル構成

```
k-games/
├─ index.html                  ← K-games トップページ(一覧表)
├─ 404.html                    ← 404ページ(2000年代テイスト、トップ・全ゲームへリンク)
├─ README.md                   ← GitHub上のリポジトリ説明(プロジェクト概要 + 索引)
├─ qr.png                      ← 本サイトのQRコード
├─ favicon.svg                 ← モダンブラウザ用 ベクターファビコン
├─ favicon-16.png              ← 16×16 PNG
├─ favicon-32.png              ← 32×32 PNG
├─ favicon-48.png              ← 48×48 PNG(将来用、現状未参照)
├─ apple-touch-icon.png        ← 180×180 iOS用
├─ icon-512.png                ← 512×512 PWA / og:image 兼用
│
├─ _template/                  ← 新作ゲーム追加用テンプレ(Jekyll除外でPagesに公開されない)
│  ├─ index.html               ← __GAME_NAME__ / __GAME_DIR__ プレースホルダ入り雛形
│  ├─ 引き継ぎ書-template.md   ← 引き継ぎ書テンプレ
│  └─ README.md                ← 追加手順チェックリスト
│
├─ dtt/                        ← DTT-mini(タワーディフェンス)
│  ├─ index.html               ← PC/タブレット 振り分けページ(自動判定+手動選択)
│  ├─ pc.html                  ← PC版本体(旧 dtt-mini/index.html を移したもの)
│  ├─ tablet.html              ← タブレット版本体
│  ├─ guide.html               ← Win98 風 攻略ガイド
│  ├─ 98.css                   ← guide.html 専用 (jdan/98.css)
│  └─ qr.png                   ← DTT 振り分けページのQR(k-games URL で焼成済)
│
├─ popins.html                 ← ポピンズ(ブロック崩し)単一HTML
├─ qr-popins.png               ← ポピンズ用QR(k-games URL で焼成済)
│
├─ shoot/                      ← 的当て
│  ├─ index.html
│  └─ qr.png                   ← shoot 用QR(k-games URL で焼成済)
│
├─ ketorus/                    ← けとるす(落ちものパズル)
│  ├─ index.html
│  └─ qr.png                   ← ketorus 用QR(k-games URL で焼成済)
│
├─ cafe-merge/                 ← カフェマージ(物理マージパズル)
│  ├─ index.html
│  └─ qr.png                   ← cafe-merge 用QR(k-games URL で焼成済)
│
├─ shirokuro/                  ← しろくろ(オセロ)
│  ├─ index.html
│  └─ qr.png                   ← shirokuro 用QR(k-games URL で焼成済)
│
├─ saikoro-wars/               ← サイコロ大戦争(陣地取り)
│  ├─ index.html
│  └─ qr.png                   ← saikoro-wars 用QR(k-games URL で焼成済)
│
├─ 引き継ぎ書-k-games.md       ← このファイル(サイト全体)
├─ 引き継ぎ書-DTT-mini.md      ← DTT-mini 詳細
├─ 引き継ぎ書-popins.md        ← ポピンズ 詳細
├─ 引き継ぎ書-shoot.md         ← 的当て 詳細
├─ 引き継ぎ書-ketorus.md       ← けとるす 詳細
├─ 引き継ぎ書-cafe-merge.md    ← カフェマージ 詳細
├─ 引き継ぎ書-shirokuro.md     ← しろくろ 詳細
└─ 引き継ぎ書-saikoro-wars.md  ← サイコロ大戦争 詳細
```

⚠ 個別引き継ぎ書(`引き継ぎ書-<game>.md`)の本文中に書かれている「公開URL」は `dtt-mini` 時代の URL のまま。**本サイト URL は `https://windom21-cpu.github.io/k-games/<game>/` に置き換えて読む**こと。両URLとも現状生きているが、新規作業の主軸は `k-games` 側。

---

## 一覧の並び順

トップページ `index.html` の表示順は以下で固定(ユーザー指定):

1. DTT-mini
2. ポピンズ
3. 的当て
4. けとるす
5. カフェマージ
6. しろくろ
7. サイコロ大戦争

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

`index.html` / `dtt/index.html` / `404.html` 共通スタイル:

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

## SEO / メタタグ / ファビコン規約

すべての公開HTMLページの `<head>` に以下のブロックを揃える(404.html と _template も同様。新ゲーム追加時の `_template/index.html` には既に雛形入り)。

```html
<title>ページタイトル</title>
<meta name="description" content="ページごとに固有の1文の説明">
<link rel="canonical" href="https://windom21-cpu.github.io/k-games/<相対パス>">
<meta property="og:type" content="website">
<meta property="og:title" content="タイトル | K-games">
<meta property="og:description" content="OGP用の説明">
<meta property="og:url" content="https://windom21-cpu.github.io/k-games/<相対パス>">
<meta property="og:site_name" content="K-games">
<meta name="twitter:card" content="summary">
<meta property="og:image" content="https://windom21-cpu.github.io/k-games/icon-512.png">
<meta property="og:image:width" content="512">
<meta property="og:image:height" content="512">
<link rel="icon" type="image/svg+xml" href="/k-games/favicon.svg">
<link rel="icon" type="image/png" sizes="32x32" href="/k-games/favicon-32.png">
<link rel="icon" type="image/png" sizes="16x16" href="/k-games/favicon-16.png">
<link rel="apple-touch-icon" href="/k-games/apple-touch-icon.png">
<meta name="apple-mobile-web-app-title" content="K-games">
```

注意点:
- ファビコンは絶対パス `/k-games/...` で参照する(GitHub Pages のサブパス公開のため、サブディレクトリのページからも同じパスで効く)
- og:image はSNSに渡すので**必ず絶対URL** (`https://...`) で書く
- 各ページの canonical / og:url は **そのページ自身**の正規 URL を指す
- 404.html は `<meta name="robots" content="noindex">` を入れているため、og:image系は省略してファビコンのみ

ファビコン画像の選定:
- 採用版は v3(2026-05-03 採用)
- 元の作業ファイル(`k-games-v1〜v4-*.png/svg`)は削除済

---

## 開発フロー

```bash
# 1. 編集
$EDITOR /home/sk/デスクトップ/k-games/<対象>

# 2. ローカル確認(必要なら)
xdg-open /home/sk/デスクトップ/k-games/index.html
# ファビコンや絶対パス参照を含む確認は、http サーバ越しでないと効かない:
# python3 -m http.server 8000  →  http://localhost:8000/k-games/  で確認

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
# サイト全体QR
qrencode -o /home/sk/デスクトップ/k-games/qr.png -s 10 -m 2 \
  "https://windom21-cpu.github.io/k-games/"

# 個別ゲームQR(例: cafe-merge)
qrencode -o /home/sk/デスクトップ/k-games/cafe-merge/qr.png -s 10 -m 2 \
  "https://windom21-cpu.github.io/k-games/cafe-merge/"
```

URL を変えたときだけ焼き直し。現状すべて k-games URL で焼成済み。

---

## 新作ゲーム追加フロー

`_template/` ディレクトリに雛形と手順チェックリストがある。詳細は `_template/README.md` を参照。

ざっくり:

1. `cp -r _template <ゲーム名>` でコピー
2. `<ゲーム名>/index.html` 内の `__GAME_NAME__` / `__GAME_DIR__` / `__GAME_DESCRIPTION__` を一括置換
3. ゲーム本体を実装
4. トップ `index.html` の一覧表に追記(並び順はユーザーに確認)
5. `qrencode` で `<ゲーム名>/qr.png` を焼く
6. 引き継ぎ書 `引き継ぎ書-<ゲーム名>.md` を `_template/引き継ぎ書-template.md` から作成、k-games 直下へ移動
7. このファイル(`引き継ぎ書-k-games.md`)の「ファイル構成」「各ゲームの索引」に追記
8. `git commit && git push`

新ゲームの head には `_template` のメタタグ群が既に入っているので、SEO/OGP/ファビコンは追加作業不要。

---

## 旧 dtt-mini リポジトリとの関係

| 項目 | 状態 |
|---|---|
| 公開URL | https://windom21-cpu.github.io/dtt-mini/ も生きたまま(QR・既存リンク互換のため) |
| canonical | 旧側全 HTML に `<link rel="canonical">` で k-games 側を指すよう設定済み |
| README.md | 旧側にも置いた:「保守モード・最新は k-games」案内 + 対応表 |
| 修正方針 | **旧側は触らない**。バグ修正・機能追加はすべて k-games 側で実施 |

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
| しろくろ | `shirokuro/` | `引き継ぎ書-shirokuro.md` | https://windom21-cpu.github.io/k-games/shirokuro/ |
| サイコロ大戦争 | `saikoro-wars/` | `引き継ぎ書-saikoro-wars.md` | https://windom21-cpu.github.io/k-games/saikoro-wars/ |

---

## 来歴メモ

- 2026-05-03 (初版): `k-games` リポジトリを新規作成。`dtt-mini` から5ゲームをコピーし、トップページ(2000年代テイスト)+ DTT 振り分けページ + サイトQRを整備。GitHub Pages 配信開始。旧 `dtt-mini` リポジトリは保存(URL互換のため)。
- 2026-05-03 (同日改善): テーブル枠を「外枠 4px double / 内部 1px solid / 背景白」に統一。
- 2026-05-03 (同日改善): 全ページに meta description / OGP / canonical を追加。404.html・README.md・新作テンプレ `_template/` を追加。各ゲーム配下のQRをすべて k-games URL で焼き直し。旧 `dtt-mini` リポジトリにも canonical と移転 README.md を追加(ゲーム本体DOMには手を入れず安全策)。
- 2026-05-03 (同日改善): ファビコン v3 を採用(`favicon.svg`, `favicon-16/32/48.png`, `apple-touch-icon.png` 180×180, `icon-512.png`)。全ページに `<link rel="icon">` 群と `og:image` を追加(`_template` も同期)。試作版 v1/v2/v4 は削除。
- 2026-05-03 (同日改善): トップ `index.html` のテーブル枠を CSS の `4px double` から HTML 属性 `border="1"`(デフォルト cellspacing=2 で二重線風になる)に変更。`table.frame` 関連 CSS は削除。
- 2026-05-03 (同日改善): 全公開HTML(`dtt/guide.html` を除く)の `font-family` を `"MS UI Gothic", "ＭＳ ＵＩ Ｇｏｔｈｉｃ", "MSUIGothic", "Hiragino Kaku Gothic ProN", sans-serif` に統一。Windows ではUIゴシック、それ以外は端末既定のヒラギノ/Noto等にフォールバック。
- 2026-05-03 (新作追加): **しろくろ**(オセロ / リバーシ)を追加。CPU(よわい/ふつう/つよい の3段階)+ 2人プレイ。AI は negamax(α-β、depth=3 hard 時)+ 位置重みテーブル + モビリティ評価。トップ一覧の末尾に挿入。
- 2026-05-03 (しろくろ v1.0.1): 候補マスのヒントを中央の丸ドット → 角丸四角ハイライトに変更。クリックできる場所が視覚的に分かりやすくなった(CSS のみ)。
- 2026-05-04 (新作追加): **サイコロ大戦争**(Dice Wars 風の陣地取り)を追加。関東地方の7都県を六角タイル46マスで表現、4人プレイ(あなた + CPU3体)、CPU 3段階(よわい/ふつう/つよい)。AI は 期待勝率テーブル + 連結領域成長 + 反撃リスク評価。トップ一覧と 404.html(あわせて しろくろ も追加)に登録。
