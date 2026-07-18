# 引き継ぎ書 — K-games (サイト全体)

最終更新: 2026-07-18(新作「スターマイマイ」v1.0.0 追加)

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
├─ Lemmings/                   ← メジェドを救え!(レミングス風 誘導パズル)
│  ├─ index.html               ← ゲーム本体(単一HTML)
│  ├─ medjed_A_basic.svg       ← 基本のメジェド
│  ├─ medjed_B_pharaoh.svg     ← ファラオ
│  ├─ medjed_C_warrior.svg     ← 戦士
│  ├─ medjed_D_priest.svg      ← 司祭
│  ├─ medjed_E_mummy.svg       ← ミイラ
│  ├─ medjed_F_child.svg       ← 子供
│  └─ qr.png                   ← Lemmings 用QR
│
├─ yuto-garden/                ← ゆうとの花だん(育成タイミング)
│  ├─ index.html               ← ゲーム本体(単一HTML、Canvas 2D の procedural 描画のみ)
│  └─ qr.png                   ← yuto-garden 用QR
│
├─ star-maimai/                ← スターマイマイ(カタツムリ育成レースSLG)
│  ├─ index.html               ← ゲーム本体(単一HTML、Canvas 2D の procedural 描画のみ)
│  └─ qr.png                   ← star-maimai 用QR
│
├─ 引き継ぎ書-k-games.md       ← このファイル(サイト全体)
├─ 引き継ぎ書-DTT-mini.md      ← DTT-mini 詳細
├─ 引き継ぎ書-popins.md        ← ポピンズ 詳細
├─ 引き継ぎ書-shoot.md         ← 的当て 詳細
├─ 引き継ぎ書-ketorus.md       ← けとるす 詳細
├─ 引き継ぎ書-cafe-merge.md    ← カフェマージ 詳細
├─ 引き継ぎ書-shirokuro.md     ← しろくろ 詳細
├─ 引き継ぎ書-saikoro-wars.md  ← サイコロ大戦争 詳細
├─ 引き継ぎ書-Lemmings.md      ← メジェドを救え! 詳細
├─ 引き継ぎ書-yuto-garden.md   ← ゆうとの花だん 詳細
└─ 引き継ぎ書-star-maimai.md   ← スターマイマイ 詳細
```

⚠ `Lemmings/` だけ先頭大文字で他ディレクトリ(全て小文字)と不揃い。新作開発時にユーザー作成済みのフォルダをそのまま流用したため。改名する場合は `git mv` + QR再焼成 + 各引き継ぎ書のパス修正が必要。

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
8. メジェドを救え!
9. ゆうとの花だん
10. スターマイマイ

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

### Pages デプロイが反映されない時

push しても公開URLに反映されない場合、まずビルド/デプロイが成功しているかを確認する:

```bash
# 直近のビルド状態(成功/失敗)
gh api "repos/windom21-cpu/k-games/pages/builds" --jq '.[0:3] | .[] | {sha:.commit, status:.status, error:.error.message}'

# 直近のワークフロー実行
gh run list --limit 5

# 失敗ワークフローのログ
gh run view <run-id> --log-failed
```

過去の事例(2026-05-05): Pages の `deploy` ステップが GitHub 側の **HTTP 502 サーバエラー**で2連続失敗し、コミット `ca4681f` / `9d13b31` のデプロイが反映されずに止まったことがある(コードや Jekyll の問題ではなくインフラ起因)。この場合は時間を置いてから:

```bash
# 方法A: 失敗したワークフローを再実行
gh run rerun <run-id>

# 方法B: 空コミットで新しいビルドをトリガー(再実行が queued のまま進まない時)
git -c user.name='windom21-cpu' -c user.email='sk21.lawyer@gmail.com' \
  commit --allow-empty -m "chore: trigger Pages redeploy"
git push origin main
```

A→B の順で試すと早い。今回は B が即通った。

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
| メジェドを救え! | `Lemmings/` | `引き継ぎ書-Lemmings.md` | https://windom21-cpu.github.io/k-games/Lemmings/ |
| ゆうとの花だん | `yuto-garden/` | `引き継ぎ書-yuto-garden.md` | https://windom21-cpu.github.io/k-games/yuto-garden/ |
| スターマイマイ | `star-maimai/` | `引き継ぎ書-star-maimai.md` | https://windom21-cpu.github.io/k-games/star-maimai/ |

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
- 2026-05-05 (サイコロ大戦争 v1.1.0): 残り2人になった時点で消化試合化を防ぐ分岐を追加。領地差 ≤ 6 なら **決戦モード(サイコロPK 5本3勝先取・両者3個ずつ・同点はやり直し)** を自動発火、それ以外なら **服従選択モーダル**(優勢時:「服従させますか?」/劣勢時:「服従しますか?」 / いいえで通常戦継続)。詳細は `引き継ぎ書-saikoro-wars.md`。
- 2026-05-05 (サイコロ大戦争 v1.2.0): 演出・視認性を強化。Web Audio API ベースの **効果音**(サイコロ振り/勝敗/選択/ターン/PK/ファンファーレ)+ topbar に 🔊/🔇 ミュートトグル(localStorage 永続化)。通常戦に **サイコロ目バッジ**(`⚀⚄⚂=合計` 形式、攻撃側=黄枠/守備側=赤枠)と **攻撃元→守備マスの黄色矢印** を追加。各マス下部に **8段ピップバー** でサイコロ数を視覚化。プレイヤー名表示を 「あなた」 → 「あなた赤」 に(CPU青/緑/黄 と並列)。
- 2026-05-05 (運用メモ): Pages のデプロイステップが HTTP 502 サーバエラーで2連続失敗し、`ca4681f` / `9d13b31` の反映が止まる事象が発生。空コミット `d7a5daf` で再ビルドをトリガーして復旧。再発時の対処を「Pages デプロイが反映されない時」セクションに追記済み。
- 2026-05-06 (新作追加): **メジェドを救え!** v1.0.0 公開。レミングス風の誘導パズル(全9ステージ・8通常+1隠し)。エジプト神話のメジェド神6種(基本/ファラオ/戦士/司祭/ミイラ/子供)を題材に、右へ歩くメジェドにアイテム7種(ブロック/ランプ/橋/はしご/右矢印/左矢印/落下安全)を置いてピラミッドへ誘導する。制限時間120〜180秒、評価S〜C、stage 8 を S/A クリアで隠しstage 9 解放(ファラオ全員救出必須)。詳細は `引き継ぎ書-Lemmings.md`。
- 2026-05-06 (UI改善): メジェドを救え! の **操作説明画面のみ HTML4風レトロスタイル** を採用。`k-books/引き継ぎ書_HTML4風スタイル.md` のレシピに準拠(白背景/MS UI Gothic/★見出し/`<table border="1">`/`<hr width="80%">`)。タイトル・ゲーム画面など他の画面は従来の砂漠テイストのままで、画面ごとにスタイル切替している。
- 2026-05-06 (メジェドを救え! v1.1.0): UX/演出を全面強化。**バグ修正**: ベストグレード保持で再プレイによる隠し解放再ロックを防止 / 結果モーダル遅延タイマーを ID 追跡。**演出**: 死亡時の ✕マーク+砂埃、ゴール時のフェードアウト+浮遊、隠し解放のキラ演出、ファラオ専用ファンファーレ、タイトル画面の歩行アニメ。**操作**: ⏯ スポーン一時停止 / ⏩ 2倍速、stage 1 初回チュートリアル、ステージ選択画面に地形サムネイル。詳細は `引き継ぎ書-Lemmings.md`。
- 2026-05-06 (メジェドを救え! v1.1.1): **スマホ誤タップ対策**。タッチでの即配置をやめ、ドラッグでセル追従 → 離した時点で確定する方式に変更。タッチ中はタイルより一回り大きな枠+画面四方向に伸びるクロスヘアでセル位置を表示し、指で隠れた状態でも配置先が確認できる。スマホ実機での誤タップ頻発がきっかけ。
- 2026-05-06 (メジェドを救え! v1.2.0): **ズーム機能** を追加。HUD の 🔍 トグルで Canvas を 1x / 2x / 3x に拡大、`overflow: auto` でスクロール可能に。タイルが物理的に大きくなりタップ精度が大幅改善。アイテム未選択時はネイティブスクロールが効くため、撤去とスクロールが両立。余白を活用したいというユーザー要望から実装。
- 2026-05-06 (メジェドを救え! v1.3.0): ズームを **1x ↔ 4x のみに簡素化**(タッチ環境はデフォルト 4x)。各ステージ開始時に **マッププレビュー段階**(全体表示+▶スタートボタン)を導入し、計画してから実行できるように。**Canvas 内部解像度を DPR=2 倍化**(1920×1088)+ ctx スケール、メジェドスプライトも 2倍解像度でラスタライズ。これにより 4x 時のアップスケール由来のざらつきを解消し、4x表示でも downscale 1.33倍 = 鮮明なまま。
- 2026-05-06 (メジェドを救え! v1.3.1): プレビューパネルを **Canvas 右上の小さな浮きボタン**に縮小。中央配置で説明枠が大きくマップ全体が見えなかった問題に対応。HUD と重複していたステージ情報行(出現/必要/許容死亡/制限時間)を削除し、`▶ スタート` + 小さなヒントだけに簡素化。
- 2026-05-06 (メジェドを救え! v1.4.0): 縦方向の新ステージ **Stage 9「神々の階段」** 追加。スポーン上(col 1, row 1) → ゴール下(col 28, row 13)のジグザグ降下マップ。5段の足場で各落差3タイル安全圏、底の棘を ARROW_R で右に折り返して回避。隠しを Stage 10 にスライド(解放条件は不変)。result-modal の「次へ」ボタンを `isStageUnlocked(nextIdx)` 判定に簡素化。
- 2026-05-06 (メジェドを救え! v1.5.0): 縦方向の難ステージ **Stage 10「時の試練」** 追加。子供(F ×1.4)と神官(D ×0.7)のみ出現、制限時間 90 秒の時間圧。plat 2 上の棘群を BLOCK + BRIDGE で持ち上げて越え、plat 4 上の棘群を ARROW_R で左→右に転換して回避する2段構成。隠しを Stage 11 にスライド(解放条件は不変)。
- 2026-05-06 (メジェドを救え! v1.5.1): 操作説明画面の3つのテーブル(メジェドの仲間/アイテム一覧/評価)を **HTML4風スタイル レシピ §3.2 に厳密準拠**。`.help-page table { border-collapse: collapse }` を撤去し、HTML 属性 `border="1" cellpadding="8" align="center" width="90%"` のブラウザ デフォルト描画(セル分離+3D風枠線)に戻す。
- 2026-05-11 (新作追加): **ゆうとの花だん** v1.0.0 公開。ちびっこのゆうとが畑に種を植えて花を育て、ママに花束をプレゼントする絵本風育成ゲーム。タイミングタップ式お世話 + 花の摘み取り。難易度4段階(やさしい4 / ふつう8 / むつかしい16 / おに32マス、指数関数増加)、6種の花(タンポポ/あさがお/チューリップ/コスモス/ひまわり/バラ)、ゲージのスイートスポット2段階評価(黄=good / 赤=perfect で花が増殖)、おにモードのみスポットがランダム移動。**種は0.5秒長押しで配置・タップで撤去**、1ますからスタート可。準備画面で「練習モード」(3ラウンド)を選択可能。エンディング: ゆうとが歩いてきてママに花束を渡す Canvas 2D 演出 + ハートぽよん。摘んだ花は花束として動的合成。難易度別ハイスコア TOP5 を localStorage 保存。すべて Canvas 2D の procedural 描画(画像アセット 0)。
- 2026-07-18 (新作追加): **スターマイマイ** v1.0.0 公開。カタツムリ育成レースシミュレーション(スターホース風の翻案)。サイコロ10個で誕生した1頭を半月×24ターンの1年間で育成し、年間12レース(G3×4/G2×4/G1×2+12月後半のマイマイカップ)を戦う。能力は ぬめり/体力/根性/カラ の4ステ+**隠しステ「調教効果」**(ステ別0.6〜1.7、弱い初期値でも大化けする余地)。カラは調教不可でカルシウムのエサのみで成長、雨のぬれぬれ地面は全員ブースト・晴れのカラカラ地面はカラの強さ勝負。**セーブ3スロット+version付きマイグレーション**(`MIGRATIONS` チェーン)。引退個体は殿堂に `canBreed: true` で永久保存し、v1.1 の繁殖・世代交代に備えた構造。バランスは実コード500シーズン×3方針の自動プレイで調整(雑プレイでMC制覇1.4%/上級19%)。調整値は `TUNE` に全集約。詳細は `引き継ぎ書-star-maimai.md`。
- 2026-05-11 (ゆうとの花だん v1.0.1): **横向きで説明画面が見切れる問題を修正**。`.screen` と `.modal-back` の `justify-content` を `center` → **`safe center`** に変更し `overflow-y: auto` を追加。コンテンツが画面に収まる時は中央寄せ、はみ出す時は自動的に上寄せ + ネイティブスクロールに切り替わる。タブレット横向きで操作説明・ハイスコア・やりかたモーダルがスクロールできなかった事象に対応。
