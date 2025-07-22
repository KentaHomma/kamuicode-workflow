# kamuicode Workflows

Claude Code SDK と kamuicode MCP を活用したAI生成ワークフロー集

## 🌟 概要

GitHub ActionsでClaude Code SDKとkamuicode MCPを使用してAIコンテンツを生成するワークフローテンプレート集です。目的に応じて最適なワークフローを選択できます。

**🆕 v0.3.0 最新機能**: プロフェッショナルニュース動画生成システムが追加されました。概念から完成品まで20-30分でプロレベルのニュース番組を完全自動化で生成できます。

## 📦 ワークフロー一覧

### 🏗️ [Module Workflow](./module-workflow/) 🆕 v0.3.0
モジュール化されたAI動画・バナー・ニュース生成システム。再利用可能なコンポーネントとマルチエージェント協調により、高品質なコンテンツを効率的に生成します。

**🆕 最新機能 (v0.3.0-news-video):**
- **📰 ニュース動画生成**: プロフェッショナルニュース番組の完全自動制作
- **🎵 音声・リップシンク**: 高品質ナレーション＋リップシンク同期
- **📝 字幕システム**: Gemini Vision解析による正確な字幕タイミング
- **🎬 ffmpeg統合**: プロレベル動画編集（字幕・タイトルフレーム）
- **🌐 多言語対応**: 自動翻訳機能付き

**継続機能 (v0.2.0-):**
- **マルチモデル対応**: 画像5種類・動画3種類から自由選択
- **t2v/i2v/r2v統合**: テキスト→動画、画像→動画、参照動画生成
- **動的モデル選択**: kamuicode-usage.mdから自動ツール特定
- **モデル最適化**: 各AIモデルの特性に合わせたプロンプト自動生成

**主要機能:**
- **📰 ニュース動画**: AIアナウンサー、音声、字幕、タイトル統合
- **動画生成**: 任意のAIモデル組み合わせによる高品質動画制作
- **バナー生成**: コンセプトとテキストから最大4枚のバナー広告を自動作成
- **品質分析**: Gemini Visionによる商用品質評価
- **9種類のオーケストレータ**: 用途に応じた最適ワークフロー選択

**⚠️ 注意**: モジュールは再利用性と品質向上のため継続的に調整される可能性があります。

### 🎵 [Music Video Workflow](./music-video-workflow/)
音楽と動画を統合した音楽動画を自動生成。Google Lyria + Imagen4 + Hailuo-02 Proの組み合わせ。

### 📹 [Video Workflow Template](./video-workflow-template/)
基本的な動画生成ワークフロー。学習用途や簡単な動画作成に最適。

### 🎬 [Video with Background Removal](./video-background-removal-workflow/)
背景除去機能を含む動画生成ワークフロー。

### 🔍 [Gemini I2V Analysis](./gemini-i2v-workflow/)
Gemini APIと連携した画像から動画生成の分析ワークフロー。

## 🚀 クイックスタート

1. **用途に合うワークフローを選択**
2. **該当フォルダのSETUP.mdを参照**
3. **必要なSecretsとMCP設定を追加**
4. **ワークフローを実行**

## 📋 ワークフロー選択ガイド

| 用途 | 推奨ワークフロー | 特徴 |
|------|------------------|------|
| 🆕 ニュース番組制作 | Module Workflow (News Video) | AIアナウンサー、音声、字幕、タイトル統合 |
| 🆕 マルチモデル動画生成 | Module Workflow (Multi-Model) | 任意のAIモデル選択、t2v/i2v/r2v対応 |
| 高品質・大規模生成 | Module Workflow | モジュール化、マルチエージェント |
| バナー広告作成 | Module Workflow (Banner) | コンセプトから最大4枚まで自動生成 |
| 音楽動画作成 | Music Video | 音楽+動画統合 |
| 学習・基本用途 | Video Template | シンプル、導入しやすい |
| 背景除去が必要 | Background Removal | 特殊処理対応 |
| 分析重視 | Gemini I2V | Gemini統合分析 |

## 🛠️ 必要な環境

- **Claude Code SDK**: AI エージェント実行環境
- **kamuicode MCP**: 画像・動画・音声・リップシンク生成サービス（マルチモデル対応）
- **Anthropic API Key**: Claude AI利用
- **Gemini API Key**: Gemini Vision解析（ニュース動画で推奨）
- **GitHub Actions**: ワークフロー実行環境
- **GitHub PAT Token**: プルリクエスト作成
- **🆕 ffmpeg**: プロレベル動画編集（ニュース動画で使用）

### 🆕 v0.3.0対応状況
- **マルチモデル**: 画像5種類・動画3種類・音声・リップシンク対応
- **ffmpeg統合**: プロフェッショナル動画編集機能
- **Gemini Vision**: 高精度リップシンク解析
- **多言語対応**: 自動翻訳機能

## 🤝 貢献

新しいワークフローテンプレートの追加やバグ修正のPRを歓迎します。

## 📄 ライセンス

MIT License

---

🤖 **Powered by [Claude Code SDK](https://docs.anthropic.com/en/docs/claude-code) & kamuicode MCP**