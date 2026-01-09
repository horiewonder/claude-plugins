# Marketplace

## 概要

GitHubからClaude Codeスキルをインストール・管理するためのスキルです。

## 使い方

```
/marketplace <command> [args]
```

### コマンド一覧

| コマンド | 説明 | 例 |
|---------|------|-----|
| `add` | GitHubリポジトリからスキルをインストール | `/marketplace add username/repo` |
| `remove` | インストール済みスキルを削除 | `/marketplace remove skill-name` |
| `list` | インストール済みスキルを一覧表示 | `/marketplace list` |
| `search` | リポジトリ内のスキルを検索 | `/marketplace search test` |
| `update` | インストール済みスキルを更新 | `/marketplace update username/repo` |

## 実行手順

$ARGUMENTS を解析して、対応するコマンドを実行する。

### `add username/repo` の場合

1. GitHubリポジトリ `https://github.com/username/repo` の存在を確認
2. リポジトリの `skills/` ディレクトリ内のスキル一覧を取得
3. ユーザーにインストールするスキルを選択させる（全部 or 個別選択）
4. 選択されたスキルを `~/.claude/commands/` にダウンロード
5. インストール元を記録するため `~/.claude/marketplace-registry.json` を更新

```bash
# 実行例
gh api repos/username/repo/contents/skills --jq '.[].name'
# スキルをダウンロード
curl -sL "https://raw.githubusercontent.com/username/repo/main/skills/category/skill.md" \
  -o ~/.claude/commands/skill.md
```

### `remove skill-name` の場合

1. `~/.claude/commands/skill-name.md` の存在を確認
2. ユーザーに削除確認を求める
3. 承認されたらファイルを削除
4. `~/.claude/marketplace-registry.json` から該当エントリを削除

```bash
rm ~/.claude/commands/skill-name.md
```

### `list` の場合

1. `~/.claude/commands/` 内の `.md` ファイルを一覧表示
2. `~/.claude/marketplace-registry.json` から各スキルのインストール元を表示

```bash
ls -la ~/.claude/commands/*.md 2>/dev/null
```

### `search keyword` の場合

1. 指定されたリポジトリ（デフォルト: horiewonder/claude-plugins）を検索
2. スキル名と説明にキーワードが含まれるものを表示

```bash
gh api repos/horiewonder/claude-plugins/contents/skills -q '.. | .name? // empty' | grep -i keyword
```

### `update username/repo` の場合

1. `~/.claude/marketplace-registry.json` から該当リポジトリのスキルを取得
2. 各スキルの最新版をダウンロードして上書き

## レジストリファイル形式

`~/.claude/marketplace-registry.json`:
```json
{
  "skills": {
    "quick-commit": {
      "repo": "horiewonder/claude-plugins",
      "path": "skills/productivity/quick-commit.md",
      "installed_at": "2025-01-09T12:00:00Z"
    }
  }
}
```

## 注意事項

- GitHubリポジトリはパブリックである必要がある（プライベートリポジトリは gh auth が必要）
- 既存のスキルを上書きする場合は確認を求める
- `gh` CLIがインストールされている必要がある
