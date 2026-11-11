# Research talk template

`RLC-LinTS/RLC-slides/` を参考にした、英語の研究発表用Slidevテンプレートです。白背景、黒い本文、四角い黒枠、薄黄色の観察ボックス、薄青色の定理ボックス、赤い強調、右下のページ番号を共通化しています。

## 起動

Node.js 22.12以降を使用します。

```bash
npm ci
npm run dev
```

http://localhost:3030 を開きます。依存パッケージをインストール済みなら `npm run dev` だけで起動できます。

## ファイル

- `slides.md`: 表紙と各章の読み込み。タイトル・著者・所属を編集します。
- `slides/examples.md`: 設定、動機、観察、定理・補題、証明、図付き手法、まとめのサンプル。
- `style.css`: 色、余白、文字組み、共通ボックス。
- `slide-top.vue`: 右上のタグと右下のページ番号（現在のページ / 総枚数）。タグは未指定なら非表示、ページ番号は表紙では非表示。
- `public/figures/`: 図の保存先。`/figures/ファイル名` で参照できます。

サンプルの数式は有限方策クラスの標準的な学習保証を使った説明例です。実際の研究内容に置き換えてください。参考スライドの研究成果・著者名・大学ロゴはテンプレートの内容に含めていません。

## 共通の書き方

通常の見出しは参考スライドと同じ `##` を使います。

```markdown
## Main Result

Explain how this result follows from the previous slide.

<div class="block block-theorem">

**Theorem.** State the assumptions and conclusion here.

$$
L(\widehat\pi)-L(\pi^\star)\leq\varepsilon.
$$

</div>
```

| 用途 | クラス | 背景 |
|---|---|---|
| 定義・仮定・目的 | `block` | 白 |
| 重要な観察・アイデア・貢献 | `block block-observation` | `#ffffe0` |
| 定理・補題・保証 | `block block-theorem` | `rgb(220, 241, 255)` |

文章の強調は `<span class="emphasis">key idea</span>`、数式の強調は `\textcolor{red}{...}` を使います。HTMLタグの前後と内部に空行を入れると、Markdownや数式が正しく処理されます。

段階表示は `<v-click>`、左右配置は `<div class="columns">` を使います。具体例は `slides/examples.md` にあります。章を増やすときは、`slides.md` に次の読み込みブロックを追加します。

```markdown
---
src: ./slides/next-section.md
---
```

## 右上のセクションタグ

各スライドのfrontmatterに `tag` を指定すると、右上に所属セクションとグループ内の進捗バーを表示します。
同じセクションのスライドには同じタグを指定します。`tag` が未指定または空なら非表示です。

例えば `Regret proof` が10枚ある場合、その3枚目ではタグ名の隣の細いバーが30%まで塗られます。
進捗は同じタグのスライドから自動計算されるので、手動設定は不要です。
同じタグが離れた位置にあっても1つのグループとして数え、デッキ内の順序で進捗を計算します。
数字のページ番号は右下の「現在のページ / 総枚数」にまとめています。

```markdown
---
tag: Regret proof
---

## The true environment survives

Explain this step of the regret proof.

---
tag: Regret proof
---

## Bounding the comparison policy's regret

Continue the proof.
```

既存のfrontmatterがある場合は、`class` や `layout` と同じ場所に `tag` を追加します。
読み込み先のMarkdownでも各スライドに指定できます。表示は `slide-top.vue` にまとめてあり、
スライド一覧・発表者画面・PDFでも各スライド自身のタグを使います。
色や位置、大きさは同ファイルの `.research-slide-tag` で調整できます。

## PDFとビルド

```bash
npm run build
npx playwright install chromium
npm run export
```

出力は `dist/` と `slides-export.pdf`。参考プロジェクトに合わせ、PDF出力は `--per-slide` を指定済みです。タグとページ番号は各スライドの前面レイヤーで描画し、通常表示・一覧・PDFで各ページ自身の情報を表示します。

既存のChromiumを使う場合：

```bash
npm run export -- --executable-path /absolute/path/to/chromium
```

段階表示の途中もPDFに含める場合は `npm run export -- --with-clicks` を使います。通常のPDFは各ページの最終表示を出力します。

## 見た目の調整

色と文字組みは `style.css` にまとめています。本文には参考テーマの標準フォントであるNunito Sansをローカル依存として同梱し、発表時の外部フォント配信を不要にしています。表紙のロゴは `slides.md` のコメントを外して自身のファイルを指定してください。

既存デッキへ適用する場合は `style.css`、`slide-top.vue`、フォント設定・依存を移し、本文のボックスを上のクラスへ置き換えます。このフォルダの作成だけでは、ほかのデッキは変更されません。

Slidevの構成については [Directory Structure](https://sli.dev/custom/directory-structure) を参照してください。
