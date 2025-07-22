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

# 🆕 kamuicode設定ファイルもコピー（マルチモデル機能で必須）
mkdir -p .github/workflows/kamuicode
cp kamuicode-workflow/module-workflow/kamuicode/kamuicode-usage.md .github/workflows/kamuicode/

# ファイルが正しくコピーされたか確認
ls -la .github/workflows/
# 以下のファイルが必要です：

## 🆕 基本モジュール
# - module-setup-branch.yml
# - module-planning-ccsdk.yml (🆕 モデル最適化対応)
# - module-planning-gca.yml (🆕 モデル最適化対応)
# - module-create-summary.yml
# - module-create-pr.yml

## 🆕 画像・動画生成モジュール
# - module-image-generation-kc-t2i-fal-imagen4-ultra-ccsdk.yml
# - module-image-generation-kc-t2i-fal-imagen4-fast-gca.yml
# - module-image-generation-kc-multi-model-ccsdk.yml (🆕 マルチモデル対応)
# - module-video-prompt-optimization-ccsdk.yml
# - module-video-generation-kc-r2v-fal-vidu-q1-ccsdk.yml
# - module-video-generation-kc-i2v-fal-hailuo-02-pro-gca.yml
# - module-video-generation-kc-multi-model-ccsdk.yml (🆕 マルチモデル対応)
# - module-video-analysis-gca.yml

## 🆕 バナー生成モジュール
# - module-banner-planning-ccsdk.yml
# - module-banner-text-overlay-kc-i2i-fal-flux-kontext-max-ccsdk.yml

## 🆕 ニュース動画生成モジュール（v0.3.0新機能）
# - module-news-planning-ccsdk.yml (📰 ニュース企画立案)
# - module-audio-generation-kc-multi-model-ccsdk.yml (🎵 音声生成)
# - module-lipsync-generation-kc-multi-model-ccsdk.yml (👄 リップシンク生成)
# - module-lipsync-video-analysis-gca.yml (🔍 リップシンク解析)
# - module-subtitle-overlay-ffmpeg-ccsdk.yml (📝 字幕オーバーレイ)
# - module-video-title-frame-ffmpeg-ccsdk.yml (🎬 タイトルフレーム)

## 🆕 オーケストレータ
# - orchestrator-video-generation.yml
# - orchestrator-video-generation-dual.yml
# - orchestrator-video-generation-dual-with-analysis.yml
# - orchestrator-video-generation-quad.yml
# - orchestrator-gemini-i2v-generation-analysis.yml
# - orchestrator-banner-advertisement-creation.yml
# - orchestrator-multi-model-video-test.yml (🆕 マルチモデル動画テスト版)
# - orchestrator-multi-model-image-test.yml (🆕 マルチモデル画像テスト版)
# - orchestrator-news-video-generation.yml (🆕 ニュース動画生成版)
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

### 1.4 🆕 kamuicode-usage.mdファイル

**重要**: マルチモデル対応機能で必要な設定ファイルです。

#### ファイルの配置
```bash
# kamuicode使用方法ファイルを配置
mkdir -p .github/workflows/kamuicode
cp kamuicode-workflow/module-workflow/kamuicode/kamuicode-usage.md .github/workflows/kamuicode/
```

#### ファイルの役割
- **モデル情報の管理**: 利用可能な全AIモデルの特性と仕様を記載
- **動的ツール選択**: マルチモデルモジュールがこのファイルからMCPツール名を自動特定
- **モデル最適化**: 各モデルの特性に合わせたプロンプト生成に使用

#### 必須性
- `module-image-generation-kc-multi-model-ccsdk.yml`で使用
- `module-video-generation-kc-multi-model-ccsdk.yml`で使用  
- `module-planning-ccsdk.yml`のモデル最適化機能で使用
- `module-planning-gca.yml`のモデル最適化機能で使用

**⚠️ 重要**: このファイルがないとマルチモデル機能が正常に動作しません。

## 🔐 ステップ2: Secrets設定

### 2.1 必要なSecrets

以下のキーの設定が必要です：

| Secret名 | 説明 | 必要性 | 取得方法 |
|---------|------|--------|----------|
| `ANTHROPIC_API_KEY` | Claude API Key | **必須** | [Anthropic Console](https://console.anthropic.com/)でAPI Keyを作成 |
| `PAT_TOKEN` | GitHub Personal Access Token | **必須** | Settings → Developer settings → Personal access tokens |
| `GEMINI_API_KEY` | Gemini API Key | **必須** | [Google AI Studio](https://aistudio.google.com/)でAPI Keyを作成 |

**🆕 v0.3.0での必要性の変更:**
- `GEMINI_API_KEY`が**必須**に変更：従来のGCAモジュール＋ニュース動画生成システムでリップシンク解析に使用
- GCA機能（動画分析等）とニュース動画機能の両方で必要

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

gh secret set MCP_CONFIG_SECRET --app actions
# ↑ .claude/mcp-kamuicode.jsonの内容をJSON文字列として入力

gh secret set PAT_TOKEN --app actions

# オプション（Geminiワークフロー使用時のみ）
gh secret set GEMINI_API_KEY --app actions
gh secret set GEMINI_SETTINGS_SECRET --app actions
# ↑ .gemini/settings.jsonの内容をJSON文字列として入力

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

**MCP_CONFIG_SECRETの追加：**
- **Name**: `MCP_CONFIG_SECRET`
- **Secret**: `.claude/mcp-kamuicode.json`の内容をJSON文字列として入力
- **Add secret**をクリック

**PAT_TOKENの追加：**
- **Name**: `PAT_TOKEN`  
- **Secret**: 取得したPersonal Access Token
- **Add secret**をクリック

**GEMINI_API_KEYの追加（オプション）：**
- **Name**: `GEMINI_API_KEY`
- **Secret**: 取得したGemini APIキー
- **Add secret**をクリック

**GEMINI_SETTINGS_SECRETの追加（オプション）：**
- **Name**: `GEMINI_SETTINGS_SECRET`
- **Secret**: `.gemini/settings.json`の内容をJSON文字列として入力
- **Add secret**をクリック

### 2.6 設定確認

設定完了後、Secretsページに以下が表示されることを確認：
- ✅ `CLAUDE_CODE_OAUTH_TOKEN` (Updated X minutes ago)
- ✅ `MCP_CONFIG_SECRET` (Updated X minutes ago)
- ✅ `PAT_TOKEN` (Updated X minutes ago)
- ✅ `GEMINI_API_KEY` (Updated X minutes ago) ※設定した場合
- ✅ `GEMINI_SETTINGS_SECRET` (Updated X minutes ago) ※設定した場合

## 📁 ステップ3: ディレクトリ構造

```
your-repo/
├── .github/
│   └── workflows/
│       ├── module-setup-branch.yml
│       ├── module-planning-ccsdk.yml (🆕 モデル最適化対応)
│       ├── module-planning-gca.yml (🆕 モデル最適化対応)
│       ├── module-image-generation-kc-t2i-fal-imagen4-ultra-ccsdk.yml
│       ├── module-image-generation-kc-t2i-fal-imagen4-fast-gca.yml
│       ├── module-image-generation-kc-multi-model-ccsdk.yml (🆕 マルチモデル対応)
│       ├── module-video-prompt-optimization-ccsdk.yml
│       ├── module-video-generation-kc-r2v-fal-vidu-q1-ccsdk.yml
│       ├── module-video-generation-kc-i2v-fal-hailuo-02-pro-gca.yml
│       ├── module-video-generation-kc-multi-model-ccsdk.yml (🆕 マルチモデル対応)
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
│       ├── orchestrator-banner-advertisement-creation.yml
│       ├── orchestrator-multi-model-video-test.yml (🆕 マルチモデル動画テスト版)
│       └── orchestrator-multi-model-image-test.yml (🆕 マルチモデル画像テスト版)
├── .github/
│   └── workflows/
│       └── kamuicode/
│           └── kamuicode-usage.md (🆕 マルチモデル機能で必須)
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

## 🆕 ステップ5: 新機能の使用方法

### 5.1 🆕 ニュース動画生成システム（v0.3.0）

#### プロフェッショナルニュース番組の完全自動生成

**`orchestrator-news-video-generation.yml`** を使用すると、概念から完成品まで約20-30分でプロフェッショナルなニュース動画を生成できます。

#### 📰 使用手順
1. GitHub Actionsの **orchestrator-news-video-generation** を選択
2. パラメータ入力：
   ```
   concept: "最新技術ニュース"
   news-content: "AIの最新動向について..."
   target-language: "japanese"
   image-model: "t2i-fal-imagen4-fast" (選択式)
   video-model: "i2v-fal-hailuo-02-pro" (選択式)
   audio-model: "t2s-fal-minimax-speech-02-turbo"
   ```

#### 🎯 生成される成果物
- 📺 高品質アンカー画像（AIアナウンサー）
- 🎵 プロ品質ナレーション音声
- 👄 リップシンク同期動画
- 📝 多言語字幕（タイミング最適化）
- 🎬 カスタムタイトルフレーム
- 📰 最終ニュース動画（完全統合）

#### 🔧 技術的特徴
- **ffmpeg統合**: プロレベルの動画編集
- **Gemini Vision**: 高精度リップシンク解析
- **マルチモデル対応**: 全工程でモデル選択可能
- **多言語対応**: 翻訳機能付き

### 5.2 マルチモデル対応オーケストレータ

#### 動画生成: `orchestrator-multi-model-video-test.yml`

#### 画像生成モデル選択
- `t2i-google-imagen3`: 高品質・写実的
- `t2i-fal-imagen4-ultra`: 最高品質（商用利用）
- `t2i-fal-imagen4-fast`: バランス型・高速
- `t2i-fal-flux-schnell`: 超高速生成
- `t2i-fal-rundiffusion-photo-flux`: フォトリアリスティック特化

#### 動画生成モデル選択
- `t2v-fal-veo3-fast`: テキスト→動画（画像生成をスキップ）
- `i2v-fal-hailuo-02-pro`: 画像→動画（高品質）
- `r2v-fal-vidu-q1`: 参照動画生成

#### 画像生成: `orchestrator-multi-model-image-test.yml`

画像生成に特化したテストワークフローで、動画生成を行わず高速で結果を確認できます：

- **用途**: モデル比較、プロンプトテスト、高速確認
- **処理時間**: 約3-5分（動画生成なし）
- **対応モデル**: 全5種類の画像生成モデル
- **成果物**: 高品質画像とプロンプト最適化結果

### 5.3 モデル最適化機能

計画モジュール（`module-planning-ccsdk.yml`, `module-planning-gca.yml`）にモデル指定機能が追加されました：

- **デフォルト動作**: モデル未指定時は汎用プロンプトを生成
- **最適化動作**: モデル指定時は`kamuicode-usage.md`から特性を読み取り最適化

この機能により、各AIモデルの特性に合わせた最適なプロンプトが自動生成されます。

---

**サポート:**
- Issue報告: GitHub Issues
- ドキュメント: README.md