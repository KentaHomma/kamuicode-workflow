# 📰 Professional News Video Generation System Release

**Version:** v0.3.0-news-video  
**Release Date:** 2025-07-21

## 🌟 新機能

### 🎯 完全統合ニュース動画生成システム
プロフェッショナルなニュース番組を完全自動化で生成する包括的なシステムを追加しました。概念から完成品まで約20-30分で高品質なニュース動画を生成できます。

#### 新しいオーケストレータ
- **`orchestrator-news-video-generation.yml`** - ニュース動画生成のメインオーケストレータ
  - 企画→画像→動画→音声→リップシンク→字幕→タイトルフレームの完全自動化パイプライン
  - 12の統合モジュールによる高度な処理フロー
  - 多言語対応（翻訳機能付き）
  - マルチモデル対応（画像・動画・音声・リップシンクモデルを自由選択）

#### 新しい専門モジュール（7つ追加、計2,005行）

- **`module-news-planning-ccsdk.yml`** - AI ニュース企画立案モジュール（405行）
  - Claude AIによる詳細なニュース制作企画
  - 原稿翻訳・アンカー設定・音声設定の自動化
  - タイトル生成（英語、30文字程度）・要約作成
  - 性別統一設定（女性アンカー）

- **`module-audio-generation-kc-multi-model-ccsdk.yml`** - マルチモデル音声生成モジュール（225行）
  - 複数の音声生成エンジン対応
  - `kamuicode-usage.md`からの動的ツール選択
  - 音声設定のJSON形式カスタマイズ
  - **依存ファイル**: `kamuicode-usage.md`（モデル情報管理）

- **`module-lipsync-generation-kc-multi-model-ccsdk.yml`** - リップシンク生成モジュール（233行）
  - 動画と音声の高精度同期
  - マルチモデル対応リップシンク処理
  - リップシンク設定のJSON形式カスタマイズ
  - **依存ファイル**: `kamuicode-usage.md`（モデル情報管理）

- **`module-lipsync-video-analysis-gca.yml`** - リップシンク解析モジュール（223行）
  - Gemini Visionによるリップシンク動画解析
  - 音声と口の動きから字幕タイミング決定
  - 字幕タイミング設定JSON自動生成
  - 高精度な字幕同期実現

- **`module-subtitle-overlay-ffmpeg-ccsdk.yml`** - 字幕オーバーレイモジュール（275行）
  - ffmpegとClaude Code SDKの統合
  - 日本語・英語字幕対応
  - タイミング設定に基づく正確な字幕表示
  - エラーハンドリング・ログ機能付き

- **`module-video-title-frame-ffmpeg-ccsdk.yml`** - タイトルフレーム挿入モジュール（310行）
  - ffmpegによる動画先頭へのタイトルフレーム追加
  - 元動画の解像度・フレームレート維持
  - タイトル表示時間のカスタマイズ
  - 音声・映像の完全統合処理

### 🔧 技術スタック拡張

#### ffmpeg統合
- 動画処理・字幕オーバーレイ・タイトルフレーム挿入
- プロフェッショナルレベルの動画編集機能
- 解像度・フレームレート・音質の完全保持

#### Gemini Vision統合
- リップシンク動画の高精度解析
- 音声と口の動きの同期分析
- 字幕タイミングの自動最適化

#### マルチモデル音声・リップシンク
- 複数の音声生成エンジン対応
- 動的モデル選択システム
- カスタム音声設定のJSON対応

## 📚 ドキュメント更新

### 更新されたファイル
- **dynamic-video-generation-architecture.md** - システム構成図の大幅更新
  - 新しい7つのモジュール追加
  - GCA（Gemini CLI Action）処理層の完全統合
  - MCP層のKamui Code統一表記
  - 9つのオーケストレータ対応

- **kamuicode-usage.md** - モデル情報データベース更新
  - 音声・リップシンク関連モデル追加
  - 新しいMCPツール情報統合

## 🚀 使用方法

### ニュース動画生成の基本手順
1. GitHub Actionsの **orchestrator-news-video-generation** を選択
2. 以下のパラメータを入力：
   - **concept**: "最新技術ニュース"
   - **news-content**: "AIの最新動向について..."
   - **target-language**: "japanese"
   - **image-model**: "t2i-fal-imagen4-ultra"
   - **video-model**: "i2v-fal-hailuo-02-pro"
   - **audio-model**: "t2s-elevenlabs-v2"
   - **lipsync-model**: "lipsync-wav2lip"
3. **Run workflow** をクリック
4. 約20-30分でプロフェッショナルなニュース動画が生成

### 生成される成果物
- 高品質アンカー画像（Imagen4 Ultra品質）
- リップシンク同期動画
- 多言語対応音声（プロ品質）
- 正確なタイミングの字幕
- カスタムタイトルフレーム
- プロフェッショナルな最終動画
- 詳細な制作ドキュメント
- GitHub PRでの美しい表示

## 🎯 技術的改善

### GitHub Token統一
- 全モジュールで `github_pat` 使用に統一
- セキュリティ・権限管理の一元化
- 既存モジュールの更新（5ファイル）

### ファイル構造統一
- image-url.txtの保存場所をGCAアプローチに統一
- 一貫したディレクトリ構造の実現
- 全モジュール間でのファイル管理統一

### システム規模の拡張
- **新規追加**: 2,005行のコード追加
- **合計モジュール数**: 22モジュール（+7）
- **合計オーケストレータ**: 9種類（+1）
- **技術統合**: ffmpeg + Gemini Vision + マルチモデルAI

## 🔄 互換性

### 既存システムへの影響
- **完全後方互換性**: 既存のワークフローには影響なし
- **モジュール式設計**: 新機能は独立して動作
- **技術スタック拡張**: ffmpeg統合による新たな可能性

### 必要な設定
- **Anthropic API Key**: Claude Code SDK利用
- **Gemini API Key**: Gemini Vision解析
- **GitHub PAT Token**: PR作成・リポジトリ操作
- **kamuicode MCP**: 全AI生成サービス統合
- **ffmpeg**: 動画編集・字幕・タイトルフレーム処理

---

## 📋 ファイル変更履歴

### 新規追加 (7ファイル)
- `orchestrator-news-video-generation.yml` (276行)
- `module-news-planning-ccsdk.yml` (405行)
- `module-audio-generation-kc-multi-model-ccsdk.yml` (225行)
- `module-lipsync-generation-kc-multi-model-ccsdk.yml` (233行)
- `module-lipsync-video-analysis-gca.yml` (223行)
- `module-subtitle-overlay-ffmpeg-ccsdk.yml` (275行)
- `module-video-title-frame-ffmpeg-ccsdk.yml` (310行)

### 技術改善 (8ファイル)
- GitHub token統一 (5ファイル)
- ファイル構造統一 (3ファイル)
- アーキテクチャ図更新 (1ファイル)
- kamuicode設定更新 (1ファイル)

---

# 🎬 Multi-Model AI Generation System Release

**Version:** v0.2.0-multi-model  
**Release Date:** 2025-07-20

## 🌟 新機能

### 🎯 マルチモデル対応ワークフローシステム
任意のAIモデルを選択して最適な画像・動画生成を実行できる包括的なシステムを追加しました。

#### 新しいオーケストレータ
- **`orchestrator-multi-model-video-test.yml`** - マルチモデル対応動画生成オーケストレータ
  - t2v/i2v/r2v全動画タイプ対応
  - 画像・動画モデルの自由選択
  - 条件分岐による効率的実行

- **`orchestrator-multi-model-image-test.yml`** - マルチモデル対応画像生成オーケストレータ
  - 画像生成に特化したテストワークフロー
  - 5種類の画像生成モデル対応
  - 高速実行（動画生成なし）
  - モデル比較・プロンプトテストに最適

#### 新しいモジュール
- **`module-image-generation-kc-multi-model-ccsdk.yml`** - マルチモデル画像生成モジュール
  - 5種類の画像生成モデル対応（Imagen3, Imagen4 Ultra/Fast, Flux Schnell, Rundiffusion Photo Flux）
  - `kamuicode-usage.md`からの動的ツール選択
  - モデル非依存設計による新モデル自動対応
  - **依存ファイル**: `kamuicode-usage.md`（モデル情報管理）

- **`module-video-generation-kc-multi-model-ccsdk.yml`** - マルチモデル動画生成モジュール
  - 3種類の動画生成モデル対応（Veo3 Fast, Hailuo-02 Pro, Vidu Q1）
  - t2v（テキスト→動画）、i2v（画像→動画）、r2v（参照動画）全タイプ統合
  - 統合インターフェースによる条件分岐処理
  - **依存ファイル**: `kamuicode-usage.md`（モデル情報管理）

#### 機能強化
- **`module-planning-ccsdk.yml`** - 計画モジュールを更新
  - モデル指定機能追加（`image-model-type`, `video-model-type`）
  - モデル未指定時：汎用プロンプト生成
  - モデル指定時：`kamuicode-usage.md`から特性を読み取り最適化
  - **依存ファイル**: `kamuicode-usage.md`（モデル最適化時）

- **`module-planning-gca.yml`** - Gemini版計画モジュールを更新
  - 同様のモデル指定機能を追加
  - モデル最適化ロジック統合
  - **依存ファイル**: `kamuicode-usage.md`（モデル最適化時）

- **`orchestrator-multi-model-video-test.yml`** - PR作成問題を修正
  - t2v動画生成時のPR作成スキップ問題を解決
  - 条件分岐ロジックの改善

## 📚 ドキュメント更新

### 更新されたファイル
- **module-workflow/README.md** - マルチモデル機能の詳細説明を追加
  - 新オーケストレータの概要とフロー図
  - マルチモデルモジュールの仕様説明
  - オーケストレータ数：5種類→7種類
  - モジュール数：12種類→15種類

- **module-workflow/SETUP.md** - セットアップ手順を更新
  - 新ファイル群をファイルリストに追加
  - ディレクトリ構造の更新
  - 新機能の使用方法説明

### 新しいアーキテクチャ
マルチモデル対応により、システムアーキテクチャが大幅に拡張され、15種類のモジュール構成による柔軟な組み合わせが可能に。

## 🚀 使用方法

### 基本的な使用手順
1. GitHub Actionsの **orchestrator-multi-model-video-test** を選択
2. 以下のパラメータを入力：
   - **concept**: "未来的なデスクでの作業風景"
   - **video_model_type**: `i2v-fal-hailuo-02-pro` または `t2v-fal-veo3-fast` または `r2v-fal-vidu-q1`
   - **image_model_type**: `t2i-fal-imagen4-ultra` または `t2i-fal-flux-schnell` など
3. **Run workflow** をクリック
4. 約10-20分で最適化された動画が生成される

### 利用可能モデル（現時点）

#### 画像生成モデル
- **`t2i-google-imagen3`**: 高品質・写実的、プロンプト理解力が高い
- **`t2i-fal-imagen4-ultra`**: 最高品質、細部まで精密（商用利用）
- **`t2i-fal-imagen4-fast`**: バランス型、安定した品質
- **`t2i-fal-flux-schnell`**: 超高速生成（1-2秒）
- **`t2i-fal-rundiffusion-photo-flux`**: フォトリアリスティック特化

#### 動画生成モデル
- **`t2v-fal-veo3-fast`**: テキスト→動画、自然な動き（画像生成スキップ）
- **`i2v-fal-hailuo-02-pro`**: 画像→動画、高品質（1080p/4-6秒）
- **`r2v-fal-vidu-q1`**: 参照動画生成、スタイル模倣（720p/4秒）

### 成果物
- 指定モデルで生成された高品質画像・動画
- モデル特性に最適化されたプロンプト
- 制作プロセス全体のドキュメント
- GitHub PRでの美しい表示

## 🔄 互換性

### 既存システムへの影響
- **既存ワークフローには影響なし**: 従来のオーケストレータは引き続き利用可能
- **後方互換性**: 既存の計画モジュール呼び出しも正常動作
- **モジュール式設計**: 新機能は独立して動作

### 必要な設定
- **Anthropic API Key**: Claude AI利用
- **GitHub PAT Token**: PR作成
- **kamuicode MCP**: 各種AI生成サービス
- **`.claude/mcp-kamuicode.json`**: MCP設定（全モデル対応）

## 🎯 技術的改善

### 動的モデル選択
- **自動ツール特定**: `kamuicode-usage.md`からMCPツール名を動的に読み取り
- **ワイルドカード許可**: `mcp__t2i-*`, `mcp__i2v-*`などで新モデル自動対応
- **モデル非依存設計**: 新しいkamuicodeモデル追加時も自動で利用可能
- **📄 kamuicode-usage.md**: モデル情報管理の中核ファイル（設置必須）

### 条件分岐処理
- **t2v最適化**: 画像生成をスキップして効率化
- **i2v/r2v統合**: 画像生成→最適化→動画生成の完全パイプライン
- **PR作成修正**: 全パターンで正常実行を保証

### プロンプト最適化
- **デフォルト**: 汎用的で高品質なプロンプト
- **モデル指定時**: 各モデルの特性に合わせた最適化
- **自動学習**: `kamuicode-usage.md`の更新で自動的に最新特性を反映

---

## 📋 ファイル変更履歴

### 新規追加 (5ファイル)
- `module-image-generation-kc-multi-model-ccsdk.yml`
- `module-video-generation-kc-multi-model-ccsdk.yml`
- `orchestrator-multi-model-video-test.yml`
- `orchestrator-multi-model-image-test.yml`
- `kamuicode/kamuicode-usage.md` (📄 モデル情報管理ファイル)

### 機能強化 (2ファイル)
- `module-planning-ccsdk.yml` - モデル指定機能追加
- `module-planning-gca.yml` - モデル指定機能追加

### 文書更新 (2ファイル)
- `kamuicode-workflow/module-workflow/README.md`
- `kamuicode-workflow/module-workflow/SETUP.md`

---

# 🎨 Banner Advertisement Creation System Release

**Version:** v0.1.0-banner  
**Release Date:** 2025-07-20

## 🌟 新機能

### 🎯 AI自動バナー生成ワークフロー
コンセプトとテキストから高品質なバナー広告を自動生成する包括的なシステムを追加しました。

#### 新しいワークフロー
- **`orchestrator-banner-advertisement-creation.yml`** - メインのバナー生成オーケストレータ
  - 最大4枚のバナーを並列生成
  - 企画→画像→テキスト合成の3段階

#### 新しいモジュール
- **`module-banner-planning-ccsdk.yml`** - AIバナー企画モジュール
  - Claude AIによる詳細な制作計画立案
  - テキスト配置エリアを考慮した画像プロンプト生成
  - 最大8バナーまで対応

- **`module-banner-text-overlay-kc-i2i-fal-flux-kontext-max-ccsdk.yml`** - テキスト合成モジュール
  - Flux Kontext Maxによる高品質テキストオーバーレイ
  - 指定テキストの一字一句変更禁止制御

#### 機能強化
- **`module-create-pr.yml`** - PR作成モジュールを更新
  - バナー画像の自動埋め込み表示対応
  - バナー専用セクション追加
  - 動的ディレクトリ構造対応

## 📚 ドキュメント更新

### 更新されたファイル
- **module-workflow/README.md** - バナー機能の詳細説明を追加
- **module-workflow/SETUP.md** - セットアップ手順にバナー関連ファイルを追加
- **kamuicode-workflow/README.md** - ワークフロー選択ガイドを更新

### 新しいアーキテクチャ図
システムアーキテクチャ図にバナー生成コンポーネントを統合し、12種類のモジュール構成を可視化。

## 🚀 使用方法

### 基本的な使用手順
1. GitHub Actionsの **orchestrator-banner-advertisement-creation** を選択
2. 以下のパラメータを入力：
   - **concept**: "新商品プロモーション"
   - **text_content**: "SALE 50% OFF"
   - **banner_size**: "square_1_1"
   - **total_banners**: "2"
3. **Run workflow** をクリック
4. 約10-15分で高品質バナーが生成される

### 成果物
- 各バナー用のベース画像（Imagen4 Ultra品質）
- テキスト合成済み最終バナー
- 制作プロセス全体のドキュメント
- GitHub PRでの美しい表示

## 🎯 バナーサイズ対応
- **1:1 (square)**: SNS投稿に最適
- **16:9 (landscape)**: Web広告向け
- **9:16 (portrait)**: モバイル広告特化

## 🔄 互換性

### 既存システムへの影響
- 既存の動画生成ワークフローには影響なし
- モジュール式設計により独立して動作
- 同一のClaude Code SDK・kamuicode MCP環境で動作

### 必要な設定
- **Anthropic API Key**: Claude AI利用
- **GitHub PAT Token**: PR作成
- **kamuicode MCP**: 画像生成・テキスト合成

---

## 📋 ファイル変更履歴

### 新規追加 (4ファイル)
- `orchestrator-banner-advertisement-creation.yml`
- `module-banner-planning-ccsdk.yml`
- `module-banner-text-overlay-kc-i2i-fal-flux-kontext-max-ccsdk.yml`
- `module-create-pr.yml` (更新版)

### 文書更新 (3ファイル)
- `kamuicode-workflow/module-workflow/README.md`
- `kamuicode-workflow/module-workflow/SETUP.md`
- `kamuicode-workflow/README.md`

---

## 🙏 謝辞

これらのリリースは、Claude Code SDK、kamuicode MCP、および各種AI技術の統合により実現されました。マルチモデル対応により、AI生成コンテンツの新たな可能性を切り開く重要なマイルストーンです。

バナー生成システムとマルチモデル対応により、kamuicode-workflowは真に包括的なAIクリエイティブプラットフォームへと進化しました。