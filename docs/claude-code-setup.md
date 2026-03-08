# Claude Code 環境構築手順

Anthropic のコーディング支援ツール「Claude Code」を macOS に導入する手順と、よくあるトラブル対処をまとめます。

---

## 1. 概要

- **Claude Code**: ターミナルから使える AI コーディングツール（Anthropic 提供）
- プロジェクトディレクトリで `claude` と実行すると対話的にコード支援が受けられる

---

## 2. システム要件

| 項目 | 要件 |
|------|------|
| OS | macOS 13.0 以上 |
| メモリ | 4GB 以上 |
| Node.js | 18 以上 |
| Git | 2.23 以上（任意） |

---

## 3. インストール方法

### 方法A: ネイティブインストール（推奨）

ターミナルで以下を実行。自動更新に対応。

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

### 方法B: Homebrew

```bash
brew install --cask claude-code
```

Homebrew の場合は **PATH の追加は不要**（Homebrew が管理するため）。

---

## 4. 重要: PATH の設定（ネイティブインストール時）

ネイティブインストールでは、実行ファイルは **`~/.local/bin`** に置かれます。  
多くの環境ではこのディレクトリが初期状態で PATH に含まれていないため、そのままでは `claude` が「コマンドが見つからない」になります。

### 症状

```bash
$ claude
zsh: command not found: claude
```

### 対処: `.zshrc` に PATH を追加

1. ホームディレクトリの `~/.zshrc` を編集（なければ新規作成）。

2. 次の1行を追加する。  
   **ポイント**: `~` ではなく `$HOME` を使う（macOS の一部環境では `~` が期待通りに展開されないため）。

```bash
export PATH="$HOME/.local/bin:$PATH"
```

3. 設定を反映する。

```bash
source ~/.zshrc
```

4. 動作確認。

```bash
claude --version
# 例: 2.1.71 (Claude Code)
```

### インストール先の確認

バイナリの有無を確認したい場合:

```bash
ls -la ~/.local/bin/claude
# シンボリックリンクで、~/.local/share/claude/versions/ 以下を指している
```

---

## 5. 起動と認証

1. 作業したいプロジェクトのディレクトリに移動する。

```bash
cd /path/to/your-project
```

2. Claude Code を起動する。

```bash
claude
```

3. 初回起動時は認証が必要。  
   - Anthropic Console の OAuth  
   - または Claude Pro / Max などのアカウント  
   画面の指示に従ってログインする。

---

## 6. トラブルシューティング

| 現象 | 対処 |
|------|------|
| `command not found: claude` | 上記「4. PATH の設定」のとおり `~/.zshrc` に `$HOME/.local/bin` を追加し、`source ~/.zshrc` を実行 |
| 新しいターミナルでだけ動かない | そのターミナルで `source ~/.zshrc` を実行するか、ターミナルを開き直す |
| Homebrew で入れたのに見つからない | `brew list --cask claude-code` でインストール確認。アプリとして入っている場合は「アプリケーション」から起動する運用もある |

---

## 7. 参考リンク

- [Claude Code 公式ドキュメント（セットアップ）](https://code.claude.com/docs/ja/setup)
- [Troubleshooting - Claude Code Docs](https://docs.claude.com/en/docs/claude-code/troubleshooting)

---

*このドキュメントは、実際の環境構築で得たノウハウをまとめたものです。（最終更新: 2025年3月）*
