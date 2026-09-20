# config

Org-mode による literate configuration（文芸的設定）のリポジトリです。
各 `.org` ファイルが設定の「原本」で、`org-babel-tangle` によって
`~/.emacs.d/init.el` や `~/.bashrc` などの実ファイルへ書き出されます。

主な環境は macOS + Emacs。Linux (xmonad) / Windows 時代の設定も記録として残しています。

## 使い方

Emacs でいずれかの `.org` を開き、`C-c C-v t`（`org-babel-tangle`）を実行すると
各ブロックの `:tangle` 先へ展開されます。
`emacs_init.org` には保存時に自動 tangle する `after-save-hook` が仕込んであるため、
編集して保存するだけで `~/.emacs.d/init.el` が更新されます。

tangle 先はすべて `:tangle` ヘッダに書かれています。取り込む前に必ず確認してください。
既存の設定ファイルを上書きします。

## ファイル一覧

### Emacs

| ファイル | tangle 先 | 内容 |
| --- | --- | --- |
| `emacs_init.org` | `~/.emacs.d/init.el` | メインの Emacs 設定。パッケージ管理は [leaf.el](https://github.com/conao3/leaf.el) |
| `early-init.org` | `~/.emacs.d/early-init.el` | 起動前の初期化 |
| `new_org_roam.org` | `~/.emacs.d/lisp/my-org-roam.el` | org-roam まわりのカスタマイズ |
| `org-roam.org` | — | org-roam に関するメモ |

`emacs_init.org` の第一レベル見出し: Bootstrap / General / Appearance / Platform /
Navigation & Editing / Completion & Search / Development Tools / Programming Languages /
Org Mode / Applications / Post Process

### シェル / ターミナル

| ファイル | tangle 先 |
| --- | --- |
| `bashrc.org` | `~/.bashrc` |
| `starship.org` | `~/.config/starship.toml` |

### macOS

| ファイル | tangle 先 |
| --- | --- |
| `karabiner.org` | `~/.config/karabiner/karabiner.json` |
| `aquaskk.org` | `~/Library/Application Support/AquaSKK/…` |
| `xbar.org` | `~/Library/Application Support/xbar/…` |
| `autumn.org` | `~/.autumn.js` |

### Linux（アーカイブ）

現在は使用していませんが、記録として残しています。

`xmonad.org` / `dunst.org` / `desktop.org` / `linux.org` / `linux_keyboard_config.org`

`desktop.org` の `.desktop` ファイルだけは、仕様上 `~` や `$HOME` が展開されないため
`Exec=` が絶対パスです。`/home/YOUR_USER` を自分のホームディレクトリに置き換えてください。

### Windows（アーカイブ）

`windows.org` — Visual Studio 2019 の `vcvarsall` 連携など。

## 環境ごとの設定

`:tangle` 先はすべて `~/` からの相対パスなので、ユーザー名には依存しません。

org 関連ファイルの置き場所だけはマシンごとに違うため、`my/org-root` 一箇所に
まとめてあります。既定値は `~/org/` です。別の場所を使う場合は、このリポジトリの
ファイルではなく **`~/.emacs.d/local-init.el`**（git 管理外）に書いてください。

```elisp
;; ~/.emacs.d/local-init.el
(setq my/org-root "~/Sync/Org/")
```

このファイルは `emacs_init.org` の Bootstrap で `:noerror` 付きで読み込まれるので、
無い環境では既定値のまま起動します。

`my/org-root` からは以下が派生します。

| 変数 | 既定値 | 用途 |
| --- | --- | --- |
| `my/org-roam-dir` | `<root>/roam/` | org-roam のノート |
| `my/org-gtd-dir` | `<root>/roam/gtd/` | GTD 用ファイル |
| `my/org-daily-dir` | `<root>/roam/daily/` | デイリーノート |
| `my/org-daily-templates-dir` | `<root>/roam/daily/templates/` | デイリーノートのテンプレート |
| `my/org-task-dir` | `<root>/roam/task/` | `org-agenda-dir` |
| `my/org-config-dir` | `<root>/roam/config/` | このリポジトリ。保存時の自動 tangle の対象判定に使う |
| `my/org-agenda-calendar` | `<root>/agenda/calendar` | カレンダー連携 |
| `my/org-snippets-dir` | `<root>/snippets` | YASnippet |
| `my/org-images-dir` | `<root>/images` | org-download の保存先 |

GTD 配下の個別ファイルは `(my/org-gtd-file "inbox.org")` で取得します。

なお leaf の `:custom` は値に関数呼び出しをそのまま書けない（平のリストとして
分解されてしまう）ため、派生パスは変数として持たせ、リスト値には backquote を
使っています。

Windows 用の `windows.org` はユーザーディレクトリを `%USERPROFILE%` から取得します。

## 認証情報について

このリポジトリに API キーやトークンは含めていません。
認証情報が必要な設定は Emacs の `auth-source`（`~/.authinfo.gpg`）から
読む前提にしてあります。

## ライセンス

MIT License. 詳細は [LICENSE](LICENSE) を参照してください。
