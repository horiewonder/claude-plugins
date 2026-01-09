# Claude Code Marketplace 作成ガイド

このガイドは、Claude Code用のMarketplaceリポジトリを作成する際のノウハウをまとめたものです。

## 必須ディレクトリ構造

```
your-marketplace/
├── .claude-plugin/
│   └── marketplace.json      # マーケットプレイス定義（必須）
├── plugins/
│   └── your-plugin/
│       ├── .claude-plugin/
│       │   └── plugin.json   # プラグイン定義（必須）
│       ├── agents/           # エージェント定義（自動検出）
│       │   └── *.md
│       ├── skills/           # スキル定義（自動検出）
│       │   └── skill-name/
│       │       └── SKILL.md
│       └── README.md
└── README.md
```

## marketplace.json

マーケットプレイスのルートに配置する設定ファイル。

```json
{
  "$schema": "https://anthropic.com/claude-code/marketplace.schema.json",
  "name": "your-marketplace-name",
  "version": "1.0.0",
  "description": "マーケットプレイスの説明",
  "owner": {
    "name": "your-github-username",
    "email": "username@users.noreply.github.com"
  },
  "plugins": [
    {
      "name": "plugin-name",
      "source": "./plugins/plugin-name",
      "description": "プラグインの説明",
      "category": "Development",
      "tags": ["tag1", "tag2"]
    }
  ]
}
```

### 注意: 予約された名前

以下の名前はAnthropicが予約しているため使用不可：

- `claude-code-marketplace`
- `claude-code-plugins`
- `claude-plugins-official`
- `anthropic-marketplace`
- `anthropic-plugins`
- `agent-skills`
- `life-sciences`

また、`claude`や`anthropic`を含む「公式っぽい」名前も拒否される。

**NG例:** `claude-plugins` → **OK例:** `horiewonder-plugins`

## plugin.json

各プラグインの`.claude-plugin/`ディレクトリに配置する設定ファイル。

```json
{
  "name": "plugin-name",
  "version": "1.0.0",
  "description": "プラグインの説明",
  "author": {
    "name": "your-name"
  },
  "repository": "https://github.com/username/repo",
  "license": "MIT",
  "keywords": ["keyword1", "keyword2"]
}
```

### 重要: agents/skills フィールドは不要

`agents`や`skills`フィールドを明示的に指定すると、バリデーションエラーになる場合がある。

```json
// ❌ NG - エラーになることがある
{
  "agents": "./agents/",
  "skills": "./skills/"
}

// ✅ OK - 自動検出に任せる
{
  "name": "plugin-name",
  "version": "1.0.0",
  "description": "..."
}
```

Claude Codeは`agents/`と`skills/`ディレクトリを自動検出するため、明示的な指定は不要。

## エージェント定義 (agents/*.md)

```markdown
---
name: agent-name
model: inherit
description: エージェントの説明と使用例
---

エージェントへの指示内容
```

### frontmatter フィールド

| フィールド | 必須 | 説明 |
|-----------|------|------|
| `name` | ✅ | 64文字以内、小文字・数字・ハイフンのみ |
| `description` | ✅ | 1024文字以内、いつ使うかの説明 |
| `model` | ❌ | `inherit`, `sonnet`, `opus`, `haiku` のいずれか |

### model の推奨設定

- `inherit` - 親会話のモデルを継承（推奨）
- 省略した場合は `sonnet` がデフォルト

## スキル定義 (skills/skill-name/SKILL.md)

```markdown
---
name: skill-name
description: スキルの説明
---

スキルの内容
```

スキルは`skills/スキル名/SKILL.md`の形式で配置する。参照ファイルは`references/`サブディレクトリに置ける。

## インストールコマンド

```bash
# マーケットプレイスを追加
/plugin marketplace add username/repo-name

# プラグインをインストール
/plugin install plugin-name

# プラグインを更新
/plugin update plugin-name

# プラグインを削除
/plugin uninstall plugin-name

# マーケットプレイスを削除
/plugin marketplace remove marketplace-name
```

## トラブルシューティング

### エラー: Marketplace file not found

`.claude-plugin/marketplace.json`が存在しないか、パスが間違っている。

### エラー: Invalid schema - name reserved

マーケットプレイス名が予約語に該当している。`claude`や`anthropic`を含まない名前に変更する。

### エラー: Invalid input (agents field)

`plugin.json`から`agents`や`skills`フィールドを削除し、自動検出に任せる。

### モデル設定が反映されない

プラグインのキャッシュが古い可能性がある：

1. `/plugin uninstall plugin-name`
2. `/plugin install plugin-name`
3. Claude Codeを再起動

### プラグインの変更が反映されない

1. GitHubにpushする
2. `/plugin update plugin-name`
3. Claude Codeを再起動

## ベストプラクティス

1. **シンプルな構造を維持** - 必要最小限のファイルのみ配置
2. **自動検出を活用** - `agents/`と`skills/`は明示的に指定しない
3. **model: inherit を使用** - 親会話のモデルを継承するのが柔軟
4. **説明を充実させる** - `description`にユースケースと例を含める
5. **バージョン管理** - 変更時は`version`を更新する

## 参考リンク

- [Plugins reference - Claude Code Docs](https://code.claude.com/docs/en/plugins-reference)
- [Create and distribute a plugin marketplace](https://code.claude.com/docs/en/plugin-marketplaces)
- [Subagents - Claude Code Docs](https://code.claude.com/docs/en/sub-agents)
- [Agent Skills - Claude Code Docs](https://code.claude.com/docs/en/skills)
