# AI Video Generation Workflow

Claude Code SDK と kamuicode MCP を使用したAI動画生成ワークフロー

## 概要
このGitHub Actionsワークフローは以下を自動実行します：
1. **Imagen4 Ultra** で高品質画像生成
2. **Vidu Q1** で参照動画生成
3. 結果をプルリクエストとして提出

## ワークフロー

### 1. `create-video-from-prompt.yml`
手動実行型の動画生成ワークフロー。GitHub Actionsから直接実行。

### 2. `issue-video-trigger.yml`
Issueからのトリガー型ワークフロー。以下の形式でIssueまたはコメントに記載：

```
@create-video-i2v
コンセプト：[動画のコンセプトや内容を記述]
```

## 必要な設定

### Secrets
- `CLAUDE_CODE_OAUTH_TOKEN`: Claude APIキー
- `PAT_TOKEN`: (オプション) GitHub Personal Access Token

### MCP設定
`.claude/mcp-kamuicode.json` ファイルが必要です

### リポジトリ権限
Settings → Actions → General で以下を設定：
- **Workflow permissions**: "Read and write permissions"（ブランチ作成・ファイル保存のため）
- **Allow GitHub Actions to create and approve pull requests**: ✓（PR自動作成のため）

## セットアップ

1. `.github/workflows/` にワークフローファイルをコピー
2. `.github/ISSUE_TEMPLATE/` にIssueテンプレートをコピー（オプション）
3. 必要なSecretsを設定

## 使用方法

### 方法1: 手動実行
1. GitHub Actions → `Create Video from Prompt` を選択
2. `Run workflow` をクリック
3. プロンプトを入力して動画生成

### 方法2: Issue経由
1. 新しいIssueを作成
2. テンプレート「AI動画生成リクエスト (I2V)」を選択
3. コンセプトを記入して送信
4. 自動的に動画生成が開始

### 方法3: コメントでトリガー
既存のIssueやPRのコメントで：
```
@create-video-i2v
コンセプト：[動画のコンセプト]
```

## ライセンス
MIT License

---

🎬 **AI Video Generation Workflow** - Powered by [Claude Code SDK](https://github.com/anthropics/claude-code) & [kamuicode MCP](https://www.kamui.ai/ja)