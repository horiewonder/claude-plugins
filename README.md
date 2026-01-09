# Claude Plugins Marketplace

Claude Code用のエージェントとスキルを配布するMarketplaceリポジトリ。

## インストール

```bash
# マーケットプレイスを追加
/plugin marketplace add horiewonder/claude-plugins

# プラグインをインストール
/plugin install claude-essentials
```

## 含まれるプラグイン

### claude-essentials

開発支援に役立つ6つのエージェントと1つのスキルを含むコレクション。

**エージェント:**

| エージェント | 説明 |
|-------------|------|
| documentation-expert | ドキュメント作成・レビュー・改善 |
| bug-hunter-test-engineer | バグ発見に特化したテストケース作成 |
| code-security-reviewer | セキュリティ・パフォーマンス・保守性レビュー |
| log-investigator | ログ分析とデバッグ調査 |
| tech-docs-researcher | 最新技術ドキュメントの調査・収集 |
| ux-interface-designer | シンプルで使いやすいUI/UXデザイン |

**スキル:**

| スキル | 説明 |
|--------|------|
| twelve-factor-review | Laravel/PHPコードのTwelve-Factor App準拠レビュー |

## 使い方

エージェントはClaude Codeが自動的に適切なタイミングで呼び出します：

- 「このコードのドキュメントを作って」→ documentation-expert
- 「テストケース作って」→ bug-hunter-test-engineer
- 「セキュリティレビューして」→ code-security-reviewer
- 「ログを調べて」→ log-investigator
- 「React 19の新機能を調べて」→ tech-docs-researcher
- 「UIのレイアウトを考えて」→ ux-interface-designer

スキルはスラッシュコマンドで呼び出せます：

```bash
/twelve-factor-review
```

## ディレクトリ構成

```
claude-plugins/
├── .claude-plugin/
│   └── marketplace.json      # マーケットプレイス定義
├── plugins/
│   └── claude-essentials/    # プラグイン本体
│       ├── .claude-plugin/
│       │   └── plugin.json
│       ├── agents/           # 6つのエージェント
│       ├── skills/           # スキル
│       └── README.md
└── README.md
```

## ライセンス

MIT
