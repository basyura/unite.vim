# リポジトリガイドライン

## プロジェクト構成

Unite.vim は、ソースから候補を収集し、絞り込みとアクションの実行を行う Vimscript プラグインです。

- `plugin/`：コマンドの登録と起動時の連携処理。
- `autoload/unite.vim` と `autoload/unite/`：本体の実装。拡張モジュールは `sources/`、`filters/`、`kinds/` に配置します。
- `autoload/vital/`：同梱する Vital 依存ライブラリ。目的と無関係な編集は避けてください。
- `syntax/unite.vim`：画面のハイライト定義。
- `doc/unite.txt`：詳細なヘルプ。`README.md`：基本的な使い方。
- `vest/test-*.vim`：動作テスト。

## ビルド・テスト・開発コマンド

コンパイルやパッケージのビルドは不要です。リポジトリのルートで、ユーザー設定を読み込まない Vim を起動します。

```sh
vim -Nu NONE -N --cmd 'set runtimepath^=.' -c 'runtime plugin/unite.vim'
```

`:Unite file buffer` で候補の選択、絞り込み、アクションを確認します。`:helptags doc` でヘルプタグを生成すると、`:help unite` でヘルプを参照できます。

`.travis.yml` に記載された静的解析コマンドは次のとおりです。

```sh
sh /tmp/vim-vimlint/bin/vimlint.sh -l /tmp/vim-vimlint -p /tmp/vim-vimlparser autoload
```

実行には、`syngan/vim-vimlint` と `ynkdir/vim-vimlparser` を上記のパスへ別途取得する必要があります。

## コードスタイルと命名規則

周辺の従来形式の Vimscript に合わせ、ブロックは半角空白 2 個で字下げします。継続行にはバックスラッシュ、関数定義には `function!` を使い、変数のスコープ（`s:`、`a:`、`g:`）を明示してください。自動読み込み関数の名前は配置パスに対応させます。例：`autoload/unite/sources/buffer.vim` の `unite#sources#buffer#define()`。既存の `cpoptions` の保存・復元処理と折り畳みマーカーを維持してください。名前は意味の分かる snake_case とし、フィルターには既存の接頭辞 `matcher_`、`sorter_`、`converter_` を使います。自動整形ツールの設定はありません。

## テスト方針

テストには Vest の DSL（`Context`、`It`、`ShouldEqual`、`End`、`Fin`）を使います。回帰テストは対応する `vest/test-*.vim` に追加してください。実行には別途 Vest ランナーが必要です。このリポジトリには実行用コマンドやカバレッジの基準はありません。Lua マッチャーのテストには Lua 対応の Vim が必要です。変更箇所の静的解析結果、テスト結果、手動での再現手順を報告してください。

## コミットとプルリクエスト

確認できる Git 履歴には、命令形の件名 `Remove redrawtime changes` があります。同様に、変更内容を簡潔に示す件名にしてください。変更範囲は目的に絞ります。プルリクエストには問題、変更後の動作、検証結果、関連する課題を記載してください。不具合報告は `CONTRIBUTING.md` に従い、OS、Vim のバージョン、50 行未満の最小構成の vimrc、再現手順、期待する動作、必要に応じてスクリーンショットを添えてください。公開されている動作を変更する場合はヘルプも更新します。
