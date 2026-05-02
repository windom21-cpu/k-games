# K-games

ブラウザでサクッと遊べるミニゲーム集。インストール不要、すべて単一HTMLで自己完結。
2000年代の素朴なHTMLテーブル風のシンプルなトップページからアクセスできます。

**公開URL: <https://windom21-cpu.github.io/k-games/>**

![K-games QR](./qr.png)

---

## 収録ゲーム

| タイトル | ジャンル | 公開URL |
|---|---|---|
| **DTT-mini** | タワーディフェンス | <https://windom21-cpu.github.io/k-games/dtt/> |
| **ポピンズ** | ブロック崩し | <https://windom21-cpu.github.io/k-games/popins.html> |
| **的当て** | シューティング | <https://windom21-cpu.github.io/k-games/shoot/> |
| **けとるす** | 落ちものパズル | <https://windom21-cpu.github.io/k-games/ketorus/> |
| **カフェマージ** | 物理マージパズル | <https://windom21-cpu.github.io/k-games/cafe-merge/> |

DTT-mini は PC版/タブレット版の選択ページが入口になっています(自動判定で「★おすすめ」マーク付き)。

---

## 構成

```
k-games/
├─ index.html        ← トップページ
├─ qr.png            ← サイト全体のQRコード
├─ dtt/              ← DTT-mini(PC版・タブレット版・ガイド)
├─ popins.html       ← ポピンズ
├─ shoot/            ← 的当て
├─ ketorus/          ← けとるす
├─ cafe-merge/       ← カフェマージ
└─ 引き継ぎ書-*.md   ← サイト全体・各ゲームの仕様メモ
```

すべて静的HTML/CSS/JSのみで動作。ビルド工程なし。
編集 → `git push` で GitHub Pages に1〜2分で反映されます。

---

## 開発

詳細な仕様・運用ルールは [`引き継ぎ書-k-games.md`](./引き継ぎ書-k-games.md) を参照。
各ゲームの個別仕様は `引き継ぎ書-<game>.md` に分かれています。

---

## ライセンス・クレジット

個人趣味のミニゲーム集です。
DTT-mini は 2007 年の Flash ゲーム *Desktop Tower Defense* を参考にした復刻版、
ポピンズはセガのメダリンクシリーズ「マジカルポピンズ」を参考にしたオマージュです。
