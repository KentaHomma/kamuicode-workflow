# AI Music Video Generator Workflow

音楽主導のAIミュージックビデオ生成ワークフロー

## 🎵 概要

このGitHub Actionsワークフローは、**音楽コンセプト**から完全なミュージックビデオを自動生成します。

### 🎯 生成フロー

1. **🎼 音楽生成** - Google Lyriaで30-40秒の音楽を生成
2. **🎨 画像生成** - 音楽に合わせた3枚の画像を生成（Imagen4 Fast）
3. **🎬 動画生成** - 画像から5秒動画を3つ生成（Hailuo-02 Pro）
4. **🎞️ 動画統合** - 音楽の長さに合わせて動画を編集・連結・統合

### 🔧 技術スタック

- **音楽生成**: Google Lyria（30-40秒、高品質）
- **画像生成**: Imagen4 Fast（1024x1024、高速）
- **動画生成**: Hailuo-02 Pro（5秒、高品質）
- **動画編集**: FFmpeg（連結・音楽統合）
- **AI統合**: Claude Code SDK + kamuicode MCP

## 🚀 使用方法

### 手動実行

1. リポジトリの**Actions**タブに移動
2. **Create AI Music Video**ワークフローを選択
3. **Run workflow**ボタンをクリック
4. 音楽コンセプトを入力（例: "サイバーパンク都市の夜景、エレクトロニック"）
5. **Run workflow**を実行

## 📁 出力構造

```
[音楽コンセプト]-[タイムスタンプ]/
├── music/
│   ├── generated-music.wav        # 生成された音楽
│   └── music-url.txt             # 音楽ファイルのURL
├── images/
│   ├── segment-1.png             # 画像1（メイン）
│   ├── segment-2.png             # 画像2（アクセント）
│   └── segment-3.png             # 画像3（トランジション）
├── videos/
│   ├── segment-1.mp4             # 動画1（5秒）
│   ├── segment-2.mp4             # 動画2（5秒）
│   └── segment-3.mp4             # 動画3（5秒）
├── final/
│   └── final-music-video.mp4     # 最終ミュージックビデオ
├── planning/
│   ├── music-video-strategy.md   # 戦略計画
│   └── video-strategy.txt        # 動画戦略
└── analysis/
    └── music-analysis.md         # 音楽分析結果
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

## 🎵 音楽コンセプト例

### 🌆 都市・未来系
```
"サイバーパンク都市の夜景、エレクトロニック、ネオンライト"
"未来の東京、シンセウェイブ、80年代レトロ"
```

### 🎮 ゲーム・アニメ系
```
"ロボットダンス、テクノ音楽、メカニカル"
"魔法の森、ケルト音楽、ファンタジー"
```

### 🌊 自然・環境系
```
"海の波、アンビエント、リラックス"
"宇宙空間、エレクトロニック、壮大"
```

## 🤖 技術的詳細

### AI Agent構成
- **音楽計画Agent**: 音楽コンセプトの分析と戦略立案
- **音楽生成Agent**: Google Lyria実行
- **音楽分析Agent**: 生成音楽の構造分析
- **画像生成Agent**: Imagen4 Fast実行（3並列）
- **動画生成Agent**: Hailuo-02 Pro実行（3並列）
- **動画統合Agent**: FFmpeg実行と最終統合

### 並列処理最適化
- 画像生成: 3つ並列実行
- 動画生成: 3つ並列実行
- 依存関係: 音楽→画像→動画→統合

## 📄 ライセンス

MIT License

## 👥 貢献

Issues、Pull Requests大歓迎です！

---

🎵 **AI-generated Music Video Workflow** - Powered by [Claude Code SDK](https://github.com/anthropics/claude-code) & [kamuicode MCP](https://www.kamui.ai/ja)