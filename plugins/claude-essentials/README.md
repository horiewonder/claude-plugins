# Claude Essentials Plugin

Claude Code用の便利なエージェントとスキルのコレクション。

## 含まれるエージェント

| エージェント | 説明 |
|-------------|------|
| **documentation-expert** | ドキュメント作成・レビュー・改善を行うスペシャリスト |
| **bug-hunter-test-engineer** | バグ発見に特化したテストケース作成のエキスパート |
| **code-security-reviewer** | セキュリティ・パフォーマンス・保守性に焦点を当てたコードレビュー |
| **log-investigator** | ログ分析とデバッグ調査のスペシャリスト |
| **tech-docs-researcher** | 最新技術ドキュメントの調査・収集 |
| **ux-interface-designer** | シンプルで使いやすいUI/UXデザインのエキスパート |

## 含まれるスキル

| スキル | 説明 |
|--------|------|
| **twelve-factor-review** | Laravel/PHPコードのTwelve-Factor App準拠レビュー |

## インストール方法

```bash
# マーケットプレイスを追加
/plugin marketplace add horiewonder/claude-plugins

# プラグインをインストール
/plugin install claude-essentials
```

## 使い方

エージェントはClaude Codeが自動的に適切なタイミングで呼び出します。以下のようなリクエストで発動します：

- 「このコードのドキュメントを作って」→ documentation-expert
- 「テストケース作って」→ bug-hunter-test-engineer
- 「セキュリティレビューして」→ code-security-reviewer
- 「ログを調べて」→ log-investigator
- 「Next.js 15の新機能を調べて」→ tech-docs-researcher
- 「UIのレイアウトを考えて」→ ux-interface-designer

スキルはスラッシュコマンドで呼び出せます：

```bash
/twelve-factor-review
```

## ライセンス

MIT
