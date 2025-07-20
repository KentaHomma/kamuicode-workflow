# セットアップガイド

AI Video Background Removal Workflowのセットアップ手順

## 📋 前提条件

- GitHub リポジトリ（Actions有効）
- Claude API アクセス
- kamuicode MCP サーバー設定

## 🔧 ステップ1: リポジトリのクローンと設定

### 1.1 リポジトリのクローン

```bash
# リポジトリをクローン
git clone https://github.com/KentaHomma/kamuicode-workflow.git
```

### 1.2 ワークフローファイルの配置

```bash
# ワークフローディレクトリを作成
mkdir -p .github/workflows

# ワークフローファイルをコピー
cp kamuicode-workflow/video-background-removal-workflow/create-video-with-bg-removal.yml .github/workflows/
cp kamuicode-workflow/video-background-removal-workflow/issue-bg-removal-trigger.yml .github/workflows/

# ファイルが正しくコピーされたか確認
ls -la .github/workflows/
```

### 1.3 Issueテンプレートの配置

```bash
# Issueテンプレートディレクトリを作成
mkdir -p .github/ISSUE_TEMPLATE

# Issueテンプレートをコピー
cp kamuicode-workflow/video-background-removal-workflow/video-bg-removal-request.md .github/ISSUE_TEMPLATE/

# ファイルが正しくコピーされたか確認
ls -la .github/ISSUE_TEMPLATE/
```

### 1.4 Claude Code設定

#### MCP設定ファイルの配置

```bash
# Claude設定ディレクトリを作成
mkdir -p .claude

# kamuicode MCP設定ファイルを配置
# .claude/mcp-kamuicode.json の設定が必要
cp kamuicode-workflow/video-background-removal-workflow/.claude/mcp-kamuicode.json .claude/

# 設定ファイルが正しく配置されたか確認
ls -la .claude/
```

#### Claude Code権限設定（重要）

**権限設定の分析方法:**
この設定は、Claude Codeが実際に実行する全ての処理を詳細に分析して作成されています。これが重要です。

**正しい分析手順:**
1. **--allowedToolsパラメータの分析** - 基本的な許可ツールを把握
2. **Claude Codeへの指示内容の分析** - PROMPTで実際に何を実行させているかを確認
3. **追加のBashコマンドの特定** - ダウンロード、動画編集、システムコマンド等の特定
4. **包括的な権限設定** - すべての必要な権限を網羅

`--allowedTools`だけでは不十分で、実際にClaude Codeが実行する処理（ダウンロード、ffmpeg、Python等）を分析する必要があります。

`.claude/settings.json` ファイルを作成し、ワークフローで使用するツールの権限を設定する必要があります：

```bash
# settings.jsonファイルを作成
cat > .claude/settings.json << 'EOF'
{
  "defaultMode": "acceptEdits",
  "permissions": {
    "allow": [
      "Bash(curl:*)",
      "Bash(wget:*)",
      "Bash(sleep:*)",
      "Bash(stat:*)",
      "Bash(find:*)",
      "Bash(ls:*)",
      "Bash(cat:*)",
      "Bash(head:*)",
      "Bash(mkdir:*)",
      "Bash(git:checkout:*)",
      "Bash(git:config:*)",
      "Bash(git:push:*)",
      "Bash(git:add:*)",
      "Bash(git:diff:*)",
      "Bash(git:commit:*)",
      "Bash(git:pull:*)",
      "Bash(date:*)",
      "Bash(jq:*)",
      "Bash(tr:*)",
      "Bash(wc:*)",
      "Bash(echo:*)",
      "Bash(npx:*)",
      "Bash(open:*)",
      "Bash(ffmpeg:*)",
      "Bash(python:*)",
      "Bash(pip:*)",
      "mcp__t2v-fal-veo3-fast__veo3_fast_submit",
      "mcp__t2v-fal-veo3-fast__veo3_fast_status",
      "mcp__t2v-fal-veo3-fast__veo3_fast_result"
    ]
  }
}
EOF

# 設定ファイルが作成されたか確認
cat .claude/settings.json
```

**⚠️ 重要**: この設定がないと、Claude CodeがMCPツールを使用できずワークフローが失敗します。

**ワークフロー分析結果 - 動画背景除去に必要な権限一覧:**

**基本的なBashコマンド:**
- `Bash(curl:*)` - ファイルダウンロード
- `Bash(wget:*)` - 代替ダウンロード方法
- `Bash(sleep:*)` - 待機処理
- `Bash(stat:*)` - ファイル情報確認
- `Bash(find:*)` - ファイル検索
- `Bash(ls:*)` - ディレクトリ一覧
- `Bash(cat:*)` - ファイル内容表示
- `Bash(head:*)` - ファイル先頭表示
- `Bash(mkdir:*)` - ディレクトリ作成
- `Bash(date:*)` - 日時取得
- `Bash(jq:*)` - JSON処理
- `Bash(tr:*)` - 文字列変換
- `Bash(wc:*)` - 文字・行数カウント
- `Bash(echo:*)` - 出力
- `Bash(npx:*)` - Node.js実行
- `Bash(open:*)` - ファイルオープン

**動画処理コマンド:**
- `Bash(ffmpeg:*)` - 動画・音声処理
- `Bash(python:*)` - Python実行（rembg）
- `Bash(pip:*)` - Python パッケージ管理

**安全なGitコマンド（必要最小限）:**
- `Bash(git:checkout:*)` - ブランチ作成・切り替え
- `Bash(git:config:*)` - ユーザー設定
- `Bash(git:push:*)` - リモートへプッシュ
- `Bash(git:add:*)` - ステージング
- `Bash(git:diff:*)` - 変更確認
- `Bash(git:commit:*)` - コミット
- `Bash(git:pull:*)` - リモートから取得

**MCPツール（動画生成）:**
- `mcp__t2v-fal-veo3-fast__veo3_fast_submit` - 動画生成開始
- `mcp__t2v-fal-veo3-fast__veo3_fast_status` - 生成状況確認
- `mcp__t2v-fal-veo3-fast__veo3_fast_result` - 生成結果取得

**🔒 セキュリティについて:**
- ファイル削除コマンド（`rm`, `rmdir`, `mv`等）は一切許可していません
- 危険なgitコマンド（`git reset`, `git clean`, `git rm`等）は除外し、必要なもののみ許可
- 主に読み取り専用コマンドと必要最小限の書き込み権限のみを許可
- 実行環境はGitHub Actionsの分離された環境のため、ローカルシステムへの影響はありません

## 🔐 ステップ2: Secrets設定

### 2.1 必要なSecrets

以下のキーの設定が必要です：

| Secret名 | 説明 | 取得方法 |
|---------|------|----------|
| `CLAUDE_CODE_OAUTH_TOKEN` | Claude API Key (必須) | [Anthropic Console](https://console.anthropic.com/)でAPI Keyを作成 |
| `PAT_TOKEN` | GitHub Personal Access Token (オプション) | Settings → Developer settings → Personal access tokens → Tokens (classic) |

### 2.2 CLAUDE_CODE_OAUTH_TOKENの取得方法

1. [Anthropic Console](https://console.anthropic.com/)にアクセス
2. ログインまたはアカウント作成
3. 左側メニューの「API Keys」をクリック
4. 「Create Key」をクリック
5. キー名を入力（例：`github-actions-video-bg-removal`）
6. 作成されたAPIキーをコピー（⚠️この画面でしか表示されません）

### 2.3 PAT_TOKENの取得方法（オプション）

1. GitHubにログイン
2. Settings → Developer settings → Personal access tokens → Tokens (classic)
3. 「Generate new token (classic)」をクリック
4. 以下の権限を選択：
   - `repo` (リポジトリへの完全アクセス)
   - `workflow` (GitHub Actionsワークフローの更新)
5. 「Generate token」をクリック
6. 作成されたトークンをコピー（⚠️この画面でしか表示されません）

### 2.4 Secrets設定手順

**2つの方法があります：**

#### 方法1: GitHub CLI（推奨・簡単）

**前提条件:**
- GitHub CLI (`gh`) がインストール済み
- `gh auth login` で認証済み

```bash
# カレントディレクトリがリポジトリ内の場合
gh secret set CLAUDE_CODE_OAUTH_TOKEN --app actions
# ↑ 実行後、APIキーを安全に入力（画面に表示されません）

# PAT_TOKEN（必要に応じて）
gh secret set PAT_TOKEN --app actions

# 設定確認
gh secret list --app actions
```

**特定のリポジトリに設定する場合:**
```bash
gh secret set CLAUDE_CODE_OAUTH_TOKEN --app actions --repo owner/repo-name
```

#### 方法2: GitHub Web UI（従来通り）

1. **GitHubリポジトリページ**にアクセス
2. **Settings**タブをクリック
3. 左サイドバーの**Secrets and variables** → **Actions**をクリック
4. **New repository secret**をクリック
5. 以下を順番に追加：

**CLAUDE_CODE_OAUTH_TOKENの追加：**
- **Name**: `CLAUDE_CODE_OAUTH_TOKEN`
- **Secret**: 先ほどコピーしたClaude APIキー
- **Add secret**をクリック

**PAT_TOKENの追加（必要に応じて）：**
- **Name**: `PAT_TOKEN`  
- **Secret**: 先ほどコピーしたPersonal Access Token
- **Add secret**をクリック

### 2.5 設定確認

設定完了後、Secretsページに以下が表示されることを確認：
- ✅ `CLAUDE_CODE_OAUTH_TOKEN` (Updated X minutes ago)
- ✅ `PAT_TOKEN` (Updated X minutes ago) ※設定した場合

## 📁 ステップ3: ディレクトリ構造

```
your-repo/
├── .github/
│   ├── workflows/
│   │   ├── create-video-with-bg-removal.yml
│   │   └── issue-bg-removal-trigger.yml
│   └── ISSUE_TEMPLATE/
│       └── video-bg-removal-request.md
├── .claude/
│   ├── mcp-kamuicode.json
│   └── settings.json
├── README.md
└── (他のファイル)
```

## 🎛️ ステップ4: GitHub権限設定（必要に応じて）

**ほとんどの場合、新しいリポジトリでは標準でONになっているため設定不要です。**

ワークフローが権限エラーで失敗する場合のみ、以下を確認してください：

**Settings** → **Actions** → **General** → **Workflow permissions**
- ✅ "Read and write permissions" を選択
- ✅ "Allow GitHub Actions to create and approve pull requests" をチェック

**よくあるエラー:**
- `Permission denied to create branch` → 上記設定を確認
- `Resource not accessible by integration` → PR作成権限を確認

## 🧪 ステップ5: テスト実行

### 5.1 Issueテンプレートを使用したテスト

1. **Issues**タブに移動
2. **New issue**をクリック
3. **動画背景除去リクエスト**を選択
4. プロンプトを入力（例: "猫が公園で遊んでいる様子"）
5. 背景色とモデルを選択（オプション）
6. **Submit new issue**をクリック

### 5.2 手動テスト

1. **Actions**タブに移動
2. **Create Video with Background Removal**ワークフローを選択
3. **Run workflow**をクリック
4. 動画プロンプトを入力（例: "猫が公園で遊んでいる様子"）
5. 背景色とrembgモデルを選択
6. **Run workflow**を実行

### 5.3 動作確認

実行後、以下を確認：

- [ ] 元動画が生成されている
- [ ] フレームが抽出されている
- [ ] 背景除去が実行されている
- [ ] 最終動画が生成されている
- [ ] Pull Requestが自動作成されている

---

**サポート:**
- Issue報告: GitHub Issues
- ドキュメント: README.md