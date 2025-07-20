# セットアップガイド

kamuicode-workflow: AI-Powered Video Generation Workflowsのセットアップ手順

## 📋 前提条件

- GitHub リポジトリ（Actions有効）
- Anthropic API アクセス（Claude Code SDK用）
- Gemini API アクセス（オプション）
- kamuicode MCP サーバー設定

## 🔧 ステップ1: リポジトリのクローンと設定

### 1.1 リポジトリのクローン

```bash
# リポジトリをクローン
git clone https://github.com/YOUR_USERNAME/kamuicode-workflow.git
cd kamuicode-workflow
```

### 1.2 ワークフローファイルの配置

**重要**: このワークフローシステムはモジュール化されているため、**全てのワークフローファイル**が必要です。

```bash
# ワークフローディレクトリを作成
mkdir -p .github/workflows

# 全てのワークフローファイルをコピー
cp kamuicode-workflow/module-workflow/*.yml .github/workflows/

# ファイルが正しくコピーされたか確認
ls -la .github/workflows/
# 以下のファイルが必要です：
# - module-setup-branch.yml
# - module-planning-ccsdk.yml
# - module-planning-gca.yml
# - module-image-generation-kc-t2i-fal-imagen4-ultra-ccsdk.yml
# - module-image-generation-kc-t2i-fal-imagen4-fast-gca.yml
# - module-video-prompt-optimization-ccsdk.yml
# - module-video-generation-kc-r2v-fal-vidu-q1-ccsdk.yml
# - module-video-generation-kc-i2v-fal-hailuo-02-pro-gca.yml
# - module-video-analysis-gca.yml
# - module-create-summary.yml
# - module-create-pr.yml
# - module-banner-planning-ccsdk.yml
# - module-banner-text-overlay-kc-i2i-fal-flux-kontext-max-ccsdk.yml
# - orchestrator-video-generation.yml
# - orchestrator-video-generation-dual.yml
# - orchestrator-video-generation-dual-with-analysis.yml
# - orchestrator-video-generation-quad.yml
# - orchestrator-gemini-i2v-generation-analysis.yml
# - orchestrator-banner-advertisement-creation.yml
```

### 1.3 MCP設定ファイルの配置

#### Claude Code SDK用MCP設定

```bash
# Claude Code SDK設定ディレクトリを作成
mkdir -p .claude

# kamuicode MCP設定ファイルを配置
# .claude/mcp-kamuicode.json の設定が必要
```

**⚠️ 重要**: `.claude/mcp-kamuicode.json`ファイルを手動で作成する必要があります。

#### kamuicode MCP設定の作成

**⚠️ 重要**: kamuicode MCP設定は、kamuicode提供者から提供される実際の設定に従ってください。

`.claude/mcp-kamuicode.json`ファイルを作成する必要がありますが、具体的な設定内容は：

- kamuicode提供者から提供される設定情報に従って設定
- 実際のMCPサーバー情報やAPIキー設定方法を確認
- このドキュメントでは設定例を提供できません（実際の設定が必要）

#### Gemini CLI Action用MCP設定（オプション）

```bash
# Gemini設定ディレクトリを作成
mkdir -p .gemini

# .gemini/settings.json の設定が必要（一部オーケストレータで使用）
```

`.gemini/settings.json`ファイルを作成（Gemini統合版使用時のみ）：

```json
{
  "mcpServers": {
    "t2i-fal-imagen4-fast": {
      "httpUrl": "[kamuicode提供のURL]",
      "timeout": 300000
    },
    "i2v-fal-hailuo-02-pro": {
      "httpUrl": "[kamuicode提供のURL]",
      "timeout": 300000
    }
  }
}
```

**⚠️ 注意**: 
- `[kamuicode提供のURL]`部分は実際のkamuicode MCPサーバーURLに置き換えてください
- kamuicode APIキーの設定方法は、kamuicode提供者の指示に従ってください

## 🔐 ステップ2: Secrets設定

### 2.1 必要なSecrets

以下のキーの設定が必要です：

| Secret名 | 説明 | 取得方法 |
|---------|------|----------|
| `CLAUDE_CODE_OAUTH_TOKEN` | Claude API Key (必須) | [Anthropic Console](https://console.anthropic.com/)でAPI Keyを作成 |
| `PAT_TOKEN` | GitHub Personal Access Token (必須) | Settings → Developer settings → Personal access tokens |
| `GEMINI_API_KEY` | Gemini API Key (オプション) | [Google AI Studio](https://aistudio.google.com/)でAPI Keyを作成 |

### 2.2 CLAUDE_CODE_OAUTH_TOKENの取得方法

1. [Anthropic Console](https://console.anthropic.com/)にアクセス
2. アカウント作成・ログイン
3. 「API Keys」セクションに移動
4. 「Create Key」をクリック
5. キー名を入力（例: "kamuicode-workflow"）
6. 生成されたキーをコピー（⚠️この画面でしか表示されません）

### 2.3 PAT_TOKENの取得方法

1. GitHubにログイン
2. Settings → Developer settings → Personal access tokens → Tokens (classic)
3. 「Generate new token (classic)」をクリック
4. 以下の権限を選択：
   - `repo` (リポジトリへの完全アクセス)
   - `workflow` (GitHub Actionsワークフローの更新)
5. 「Generate token」をクリック
6. 作成されたトークンをコピー（⚠️この画面でしか表示されません）

### 2.4 GEMINI_API_KEYの取得方法（オプション）

1. [Google AI Studio](https://aistudio.google.com/)にアクセス
2. Googleアカウントでログイン
3. 左側メニューの「Get API key」をクリック
4. 「Create API key」をクリック
5. 作成されたAPIキーをコピー

### 2.5 Secrets設定手順

**2つの方法があります：**

#### 方法1: GitHub CLI（推奨・簡単）

```bash
# カレントディレクトリがリポジトリ内の場合
gh secret set CLAUDE_CODE_OAUTH_TOKEN --app actions
# ↑ 実行後、APIキーを安全に入力（画面に表示されません）

gh secret set PAT_TOKEN --app actions

# オプション
gh secret set GEMINI_API_KEY --app actions

# 設定確認
gh secret list --app actions
```

#### 方法2: GitHub Web UI（従来通り）

1. **GitHubリポジトリページ**にアクセス
2. **Settings**タブをクリック
3. 左サイドバーの**Secrets and variables** → **Actions**をクリック
4. **New repository secret**をクリック
5. 以下を順番に追加：

**CLAUDE_CODE_OAUTH_TOKENの追加：**
- **Name**: `CLAUDE_CODE_OAUTH_TOKEN`
- **Secret**: 取得したClaude APIキー
- **Add secret**をクリック

**PAT_TOKENの追加：**
- **Name**: `PAT_TOKEN`  
- **Secret**: 取得したPersonal Access Token
- **Add secret**をクリック

**GEMINI_API_KEYの追加（オプション）：**
- **Name**: `GEMINI_API_KEY`
- **Secret**: 取得したGemini APIキー
- **Add secret**をクリック

### 2.6 設定確認

設定完了後、Secretsページに以下が表示されることを確認：
- ✅ `CLAUDE_CODE_OAUTH_TOKEN` (Updated X minutes ago)
- ✅ `PAT_TOKEN` (Updated X minutes ago)
- ✅ `GEMINI_API_KEY` (Updated X minutes ago) ※設定した場合

## 📁 ステップ3: ディレクトリ構造

```
your-repo/
├── .github/
│   └── workflows/
│       ├── module-setup-branch.yml
│       ├── module-planning-ccsdk.yml
│       ├── module-planning-gca.yml
│       ├── module-image-generation-kc-t2i-fal-imagen4-ultra-ccsdk.yml
│       ├── module-image-generation-kc-t2i-fal-imagen4-fast-gca.yml
│       ├── module-video-prompt-optimization-ccsdk.yml
│       ├── module-video-generation-kc-r2v-fal-vidu-q1-ccsdk.yml
│       ├── module-video-generation-kc-i2v-fal-hailuo-02-pro-gca.yml
│       ├── module-video-analysis-gca.yml
│       ├── module-create-summary.yml
│       ├── module-create-pr.yml
│       ├── module-banner-planning-ccsdk.yml
│       ├── module-banner-text-overlay-kc-i2i-fal-flux-kontext-max-ccsdk.yml
│       ├── orchestrator-video-generation.yml
│       ├── orchestrator-video-generation-dual.yml
│       ├── orchestrator-video-generation-dual-with-analysis.yml
│       ├── orchestrator-video-generation-quad.yml
│       ├── orchestrator-gemini-i2v-generation-analysis.yml
│       └── orchestrator-banner-advertisement-creation.yml
├── .claude/
│   └── mcp-kamuicode.json
├── .gemini/
│   └── settings.json (オプション)
├── README.md
└── (他のファイル)
```

## 🎛️ ステップ4: GitHub権限設定（必要に応じて）

**ほとんどの場合、新しいリポジトリでは標準でONになっているため設定不要です。**

ワークフローが権限エラーで失敗する場合のみ、以下を確認してください：

**Settings** → **Actions** → **General** → **Workflow permissions**
- ✅ "Read and write permissions" を選択
- ✅ "Allow GitHub Actions to create and approve pull requests" をチェック

---

**サポート:**
- Issue報告: GitHub Issues
- ドキュメント: README.md