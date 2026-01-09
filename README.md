# Claude Code Skills Marketplace

Claude Code用の自作スキルを集めたMarketplaceです。

## ⚡ クイックスタート

### 1. Marketplaceスキルをインストール

```bash
# marketplaceスキルをダウンロード
mkdir -p ~/.claude/commands
curl -sL "https://raw.githubusercontent.com/horiewonder/claude-plugins/main/skills/utilities/marketplace.md" \
  -o ~/.claude/commands/marketplace.md
```

### 2. スキルを追加

Claude Code内で以下のコマンドを実行：

```
/marketplace add horiewonder/claude-plugins
```

これで、このリポジトリ内のスキルをインストールできます！

### 使用例

```
/marketplace add horiewonder/claude-plugins   # スキルをインストール
/marketplace list                              # インストール済みスキルを一覧
/marketplace remove quick-commit               # スキルを削除
/marketplace search test                       # スキルを検索
```

---

## 📁 ディレクトリ構成

```
claude-plugins/
├── README.md              # このファイル
├── CONTRIBUTING.md        # スキル作成・投稿ガイド
├── skills/                # スキル本体
│   ├── development/       # 開発系（コード生成、テスト、デバッグなど）
│   ├── documentation/     # ドキュメント系（README作成、API仕様書など）
│   ├── productivity/      # 生産性向上系（Git操作、タスク管理など）
│   ├── creative/          # クリエイティブ系（デザイン、アート生成など）
│   └── utilities/         # ユーティリティ系（ファイル操作、変換など）
└── examples/              # サンプル・テンプレート
```

## 🚀 スキルのインストール方法

### 方法1: シンボリックリンク（推奨）

特定のスキルだけをインストールする場合：

```bash
# グローバルにインストール
ln -s /path/to/claude-plugins/skills/development/my-skill.md ~/.claude/commands/my-skill.md

# プロジェクト単位でインストール
ln -s /path/to/claude-plugins/skills/development/my-skill.md .claude/commands/my-skill.md
```

### 方法2: ファイルコピー

```bash
# グローバルにインストール
cp /path/to/claude-plugins/skills/development/my-skill.md ~/.claude/commands/

# プロジェクト単位でインストール
mkdir -p .claude/commands
cp /path/to/claude-plugins/skills/development/my-skill.md .claude/commands/
```

### 方法3: 一括インストール

全スキルをまとめてインストールする場合：

```bash
# グローバルに全スキルをシンボリックリンク
for file in /path/to/claude-plugins/skills/**/*.md; do
  ln -sf "$file" ~/.claude/commands/
done
```

## 📖 スキルの使い方

インストール後、Claude Code内で `/スキル名` で呼び出せます。

```
# 例
/commit          # commitスキルを実行
/review-code     # コードレビュースキルを実行
```

## 📂 カテゴリ一覧

| カテゴリ | 説明 | 例 |
|---------|------|-----|
| `development` | 開発・コーディング支援 | テスト生成、リファクタリング |
| `documentation` | ドキュメント作成 | README生成、API仕様書 |
| `productivity` | 生産性向上 | Git操作、PR作成 |
| `creative` | クリエイティブ作業 | デザイン、アート生成 |
| `utilities` | 汎用ユーティリティ | ファイル変換、検索 |

## 🔍 スキルを探す

```bash
# カテゴリ内のスキル一覧
ls skills/development/

# キーワードで検索
grep -r "キーワード" skills/
```

## 🤝 コントリビュート

新しいスキルを追加したい場合は [CONTRIBUTING.md](./CONTRIBUTING.md) を参照してください。

## 📝 スキルファイルの形式

スキルは Markdown ファイル（`.md`）で記述します。基本的な構造：

```markdown
---
description: スキルの簡単な説明（オプション）
---

# スキル名

## 概要
このスキルが何をするか

## 使い方
$ARGUMENTS - ユーザーからの引数を受け取る変数

## 手順
1. ステップ1
2. ステップ2
...
```

## ⚠️ 注意事項

- スキルはClaude Codeのコンテキストで実行されます
- 機密情報を含むスキルは作成しないでください
- 破壊的な操作を行うスキルには適切な確認ステップを含めてください

## 📜 ライセンス

MIT License
