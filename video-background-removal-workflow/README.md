# AI Video Background Removal Workflow

動画生成と背景除去を自動化するAIワークフロー

## 🎬 概要

このGitHub Actionsワークフローは、**テキストプロンプト**から動画を生成し、**自動的に背景を除去**します。

### 🎯 処理フロー

1. **🎬 動画生成** - Veo3でテキストから動画を生成
2. **🎨 背景除去** - rembgで各フレームの背景を除去
3. **🎞️ 動画再構築** - 処理済みフレームから背景なし動画を再構築
4. **📁 結果保存** - 元動画、処理済み動画、フレームを保存

### 🔧 技術スタック

- **動画生成**: Veo3 Fast（テキストから動画、4-6秒）
- **背景除去**: rembg（複数モデル対応）
- **動画処理**: FFmpeg（フレーム分割・結合）
- **AI統合**: Claude Code SDK + kamuicode MCP

## 🚀 使用方法

### Issue テンプレート（推奨）

1. **New Issue**をクリック
2. **動画背景除去リクエスト**を選択
3. プロンプトを入力（例: "猫が公園で遊んでいる様子"）
4. 背景色とモデルを選択（オプション）
5. **Submit new issue**をクリック

### 手動実行

1. リポジトリの**Actions**タブに移動
2. **Create Video with Background Removal**ワークフローを選択
3. **Run workflow**ボタンをクリック
4. 動画プロンプトを入力
5. 背景色とrembgモデルを選択
6. **Run workflow**を実行

## 🎨 背景色オプション

- **transparent** - 透明背景（WebM出力）
- **white** - 白背景（MP4出力）
- **black** - 黒背景（MP4出力）
- **green** - 緑背景（MP4出力）

## 🤖 rembgモデル

- **u2net** - 汎用（デフォルト）
- **u2netp** - 軽量版
- **u2net_human_seg** - 人物特化
- **isnet-anime** - アニメ特化

## 📁 出力構造

```
[プロンプト]-[タイムスタンプ]/
├── original/
│   └── generated-video.mp4      # 元動画
├── frames/
│   ├── frame_0001.png          # 抽出フレーム
│   └── ...
├── processed/
│   ├── frame_0001.png          # 背景除去済みフレーム
│   └── ...
├── final/
│   └── video-background-removed.mp4  # 最終動画
└── README.md                   # 処理詳細
```

## 🎬 動画プロンプト例

### 🐱 動物系
```
"猫が公園で遊んでいる様子"
"犬が海辺を走っている"
"鳥が空を飛んでいる"
```

### 🚗 乗り物系
```
"車が街を走っている"
"電車が駅に到着する"
"飛行機が空を飛んでいる"
```

### 🌸 自然系
```
"桜の花びらが風に舞っている"
"雲が空を流れている"
"波が岸に打ち寄せている"
```

## 🔧 セットアップ要件

### 必要なSecrets

```yaml
CLAUDE_CODE_OAUTH_TOKEN: # Claude API Key
PAT_TOKEN: # GitHub Personal Access Token
```

### 必要なファイル

```
.claude/
└── mcp-kamuicode.json    # kamuicode MCP設定
```

### 権限設定

```yaml
permissions:
  contents: write
  pull-requests: write
  actions: read
  issues: write
```

## 🤖 技術的詳細

### AI Agent構成
- **動画生成Agent**: Veo3実行
- **背景除去Agent**: rembg + ffmpeg実行

### 処理最適化
- フレーム単位での背景除去
- 透明背景はWebM、不透明背景はMP4で出力
- 30fps固定レート

## 📄 ライセンス

MIT License

## 👥 貢献

Issues、Pull Requests大歓迎です！

---

🎬 **AI Video Background Removal Workflow** - 🤖 Powered by [Claude Code SDK](https://github.com/anthropics/claude-code) & [kamuicode MCP](https://www.kamui.ai/ja)