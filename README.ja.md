[English](README.md) | **日本語**

# cadenza

「ブログ記事を書きたい / 登壇したい / LT したい」を、規律ある段階制のパイプラインに変える Claude Code プラグインです。各フェーズ — *イシューを見つける → 分解する → 絵コンテを描く → 検証する → 仕上げる* — が**ゲート**として機能し、書き急ぎを防ぎ、答えを書く前に問いを明確にすることを著者に強制します。

`cadenza` という名前は、協奏曲の中で独奏者が技巧を披露する「カデンツァ」（構造化された自由楽章）に由来します。各フェーズは規律ある一手ですが、最終的な出力は著者自身の声であるべきだ ── という設計思想を表しています。

## インストール

このリポジトリ自体が Claude Code マーケットプレイスとして機能します。マーケットプレイスを登録し、`cadenza` プラグインをインストールしてください:

```bash
claude plugin marketplace add github.com/n-yokomachi/cadenza
claude plugin install cadenza@cadenza
```

その後 Claude Code を再起動してください。

## できること

インストールすると、以下の 7 つのスキルがスラッシュコマンドとして使えるようになります:

| フェーズ | スキル | 役割 |
|---------|--------|------|
| 1. 計画 | `/cadenza:issue-finding` | 「答える価値のある問いは何か」を特定 |
| 2. 設計 | `/cadenza:issue-decomposition` | サブイシューに分解し、ストーリーラインを設計 |
| 3. 絵コンテ | `/cadenza:storyboarding` | 各サブイシューの「見せ方」を設計 |
| 4. 検証 | `/cadenza:analysis-execution` | 絵コンテに従って実装・計測・作図 |
| 5. 仕上げ | `/cadenza:output-crafting` | 最終 Markdown 成果物を生成 |
| レビュー | `/cadenza:output-proofread` | AI 主導の徹底校正（事実検証 + 言語校正）|
| レビュー | `/cadenza:output-review` | 著者主導のレビューサイクル支援 |

各スキルは完了時に「次のフェーズに進みますか？」と確認し、承認されれば次のスキルを Skill ツール経由で呼び出してチェーンします。状態は作業ディレクトリの `./.cadenza/state.md` に共有されます。最終成果物は `./.cadenza/output.md` に書き出されます。

## `output.md` の使い道

cadenza は意図的に **単一の汎用 Markdown ファイル** のみを生成します。Zenn の frontmatter、SpeakerDeck の Marp 指示子、LT 用の極限圧縮など、プラットフォーム固有の仕上げは下流のツールに委ねます。これにより、どこに公開するかに依存せずプラグインを使えるようにしています。

代表的な下流の活用パス:

- **ブログ記事 (Qiita / Zenn / dev.to / 個人ブログ)**: `output.md` をブログリポジトリにコピーし、プラットフォーム固有の frontmatter を追加、構文を調整 (Zenn なら `:::message` など)。
- **カンファレンス用デッキ / LT スライド**: `output.md` を AI スライド生成ツール (Claude Design、Gamma など) に食わせ、構造化 Markdown を解釈してビジュアルスライドを生成させる。
- **ドキュメント**: `output.md` をそのまま docs フォルダに置く。

## なぜ「ゲート式」なのか

このようなプラグインを使わなければ、Claude Code は依頼を受けた瞬間から喜んで書き始めてしまいます。短いタスクならそれで十分ですが、ブログ記事やカンファレンストークのような技術コンテンツでは、**見た目は整って流暢に読めるけれど、問いが研がれていない**ドラフトになりがちです。*cadenza* はそのプロセスを意図的に減速させます。各フェーズには明示的なアンチパターンと「上流回帰シグナル」があり、下流で土台の弱さが露呈したときに前のフェーズに戻ることを促します。

## インスピレーションの源泉

この段階構造は、日本の知的生産論で広く知られる「イシューファースト」の伝統 ── 特に安宅和人著『イシューからはじめよ』(英治出版, 2010) ── から着想を得ています。この伝統は「アウトプットの質は、答える努力よりも答える問いの質に支配される」という今や一般的な考え方を確立しました。

ただし本プラグインは **特定の書籍・フレームワークの実装ではなく**、いかなる著者・出版社とも提携・公認関係にありません。一般原則 ── *答えを書く前に問いを絞る* ── を、技術コンテンツ制作という具体的な文脈に独自に再解釈し、Claude Code のスキルパイプラインとして表現したものです。

知的生産法のより広い文献に関心がある読者には、安宅さんの本が日本語圏での代表的な入口の一つです。英語圏では Minto の「ピラミッド原則」、マッキンゼーの「空・雨・傘」フレームなどが類似の伝統を形成しています。

## リポジトリ構造

```
cadenza/
├── .claude-plugin/
│   ├── marketplace.json     ← このリポジトリを Claude Code marketplace 化
│   └── plugin.json          ← プラグイン本体のマニフェスト
├── README.md                 ← 英語版
├── README.ja.md              ← あなたはここ
├── CHANGELOG.md
├── LICENSE                   ← MIT
└── skills/
    ├── issue-finding/SKILL.md
    ├── issue-decomposition/SKILL.md
    ├── storyboarding/SKILL.md
    ├── analysis-execution/SKILL.md
    ├── output-crafting/SKILL.md
    ├── output-proofread/SKILL.md
    └── output-review/SKILL.md
```

## ライセンス

MIT。[`LICENSE`](LICENSE) を参照してください。

## 著者

Naoki Yokomachi — [@n-yokomachi](https://github.com/n-yokomachi)
