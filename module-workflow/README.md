# kamuicode-workflow: AI-Powered Video Generation Workflows

🎬 **モジュール化されたAI動画生成システム** - GitHub Actionsで完全自動化された動画制作パイプライン

## 🌟 概要

kamuicode-workflowは、Kamui Codeを活用したClaude Code SDK & Gemini CLI ActionのAIエージェントによるGithub actionのクリエイティブワークフローシステムです。

**⚠️ 重要**: このシステムのモジュールは再利用可能なコンポーネントとして設計されており、品質向上と機能改善のため継続的に調整される可能性があります。最新の変更内容については、個別のモジュールファイルのコミット履歴をご確認ください。

## 🚀 クイックスタート

### 1. セットアップ
詳細なセットアップ手順は **[SETUP.md](SETUP.md)** を参照してください。

### 2. ワークフロー実行
1. GitHub リポジトリの **Actions** タブを開く
2. 使用したいオーケストレータを選択
3. **Run workflow** をクリック
4. プロンプトを入力して実行

## 🎭 オーケストレータ（9種類）

オーケストレータは複数のモジュールを組み合わせて、特定の目的に最適化されたワークフローを実行します。

### 🆕 0. `orchestrator-news-video-generation.yml` **（v0.3.0新機能）**
**【プロフェッショナルニュース番組生成】完全統合ワークフロー**

```mermaid
graph LR
    A[🚀 Setup<br/>ブランチ・フォルダ作成] --> B[📰 News Planning<br/>ニュース企画 CCSDK]
    B --> C[🎨 Anchor Image<br/>アンカー画像生成]
    C --> D[🎬 Base Video<br/>ベース動画生成]
    B --> E[🎵 Audio Generation<br/>音声生成]
    D --> F[👄 Lipsync<br/>リップシンク生成]
    E --> F
    F --> G[🔍 Subtitle Analysis<br/>字幕タイミング解析 GCA]
    G --> H[📝 Subtitle Overlay<br/>字幕追加 ffmpeg]
    B --> I[🎬 Title Background<br/>タイトル背景生成]
    I --> J[🎨 Title Composition<br/>タイトルテキスト合成]
    J --> K[🎞️ Title Frame<br/>タイトルフレーム追加 ffmpeg]
    H --> K
    K --> L[📝 Create PR<br/>プルリクエスト作成]
```

**特徴:**
- **完全自動ニュース番組制作**: 概念から完成品まで20-30分
- **多言語対応**: 自動翻訳機能付き
- **プロレベル品質**: ffmpeg統合による高品質動画編集
- **リップシンク同期**: Gemini Vision解析による高精度字幕タイミング
- **カスタマイズ可能**: 画像・動画・音声モデルを自由選択

**生成物:**
- 📺 AIアナウンサー動画（リップシンク同期）
- 🎵 プロ品質ナレーション
- 📝 多言語字幕（完全同期）
- 🎬 カスタムタイトルフレーム
- 📰 統合ニュース動画（完成品）

### 1. `orchestrator-video-generation.yml`
**【基本1動画版】線形ワークフロー**

```mermaid
graph LR
    A[🚀 Setup<br/>ブランチ・フォルダ作成] --> B[🎯 Planning<br/>AI企画 CCSDK]
    B --> C[🎨 Image Generation<br/>画像生成 Imagen4 Ultra + CCSDK]
    C --> D[⚙️ Video Prompt Optimization<br/>プロンプト最適化 CCSDK]
    D --> E[🎬 Video Generation<br/>動画生成 Vidu Q1 + CCSDK]
    E --> F[📝 Create PR<br/>プルリクエスト作成]
```

**特徴:** 
- 順次実行によるシンプルな依存関係
- エラー追跡が容易
- 初回利用に最適

### 2. `orchestrator-video-generation-dual.yml`  
**【2動画版】並列処理ワークフロー**

```mermaid
graph LR
    A[🚀 Setup<br/>ブランチ・フォルダ作成] --> B[🎯 Planning<br/>AI企画 CCSDK total_videos: 2]
    B --> C1[🎨 Image Generation 1<br/>Imagen4 Ultra + CCSDK]
    B --> C2[🎨 Image Generation 2<br/>Imagen4 Ultra + CCSDK]
    C1 --> D1[⚙️ Video Prompt Optimization 1<br/>CCSDK]
    C2 --> D2[⚙️ Video Prompt Optimization 2<br/>CCSDK]
    D1 --> E1[🎬 Video Generation 1<br/>Vidu Q1 + CCSDK]
    D2 --> E2[🎬 Video Generation 2<br/>Vidu Q1 + CCSDK]
    E1 --> F[📝 Create PR<br/>プルリクエスト作成]
    E2 --> F
```

**特徴:**
- 並列実行による効率化
- video_index パラメータで出力先を分離
- 2つの独立したパイプライン

### 3. `orchestrator-video-generation-dual-with-analysis.yml`
**【2動画＋分析版】品質評価付きワークフロー**

```mermaid
graph LR
    A[🚀 Setup<br/>ブランチ・フォルダ作成] --> B[🎯 Planning<br/>AI企画 CCSDK total_videos: 2]
    B --> C1[🎨 Image Generation 1<br/>Imagen4 Ultra + CCSDK]
    B --> C2[🎨 Image Generation 2<br/>Imagen4 Ultra + CCSDK]
    C1 --> D1[⚙️ Video Prompt Optimization 1<br/>CCSDK]
    C2 --> D2[⚙️ Video Prompt Optimization 2<br/>CCSDK]
    D1 --> E1[🎬 Video Generation 1<br/>Vidu Q1 + CCSDK]
    D2 --> E2[🎬 Video Generation 2<br/>Vidu Q1 + CCSDK]
    E1 --> G1[🔍 Video Analysis 1<br/>品質分析 GCA + Gemini Vision]
    E2 --> G2[🔍 Video Analysis 2<br/>品質分析 GCA + Gemini Vision]
    G1 --> F[📝 Create PR<br/>プルリクエスト作成]
    G2 --> F
```

**特徴:**
- Gemini Vision APIによる商用品質評価
- 各動画に対応する analysis-{index} フォルダ作成
- 技術品質・視覚的インパクト・商用利用可能性の評価

### 4. `orchestrator-video-generation-quad.yml`
**【4動画版】大規模並列ワークフロー**

```mermaid
graph LR
    A[🚀 Setup<br/>ブランチ・フォルダ作成] --> B[🎯 Planning<br/>AI企画 CCSDK total_videos: 4]
    B --> C1[🎨 Image Generation 1<br/>Imagen4 Ultra + CCSDK]
    B --> C2[🎨 Image Generation 2<br/>Imagen4 Ultra + CCSDK]
    B --> C3[🎨 Image Generation 3<br/>Imagen4 Ultra + CCSDK]
    B --> C4[🎨 Image Generation 4<br/>Imagen4 Ultra + CCSDK]
    C1 --> D1[⚙️ Video Prompt Optimization 1<br/>CCSDK]
    C2 --> D2[⚙️ Video Prompt Optimization 2<br/>CCSDK]
    C3 --> D3[⚙️ Video Prompt Optimization 3<br/>CCSDK]
    C4 --> D4[⚙️ Video Prompt Optimization 4<br/>CCSDK]
    D1 --> E1[🎬 Video Generation 1<br/>Vidu Q1 + CCSDK]
    D2 --> E2[🎬 Video Generation 2<br/>Vidu Q1 + CCSDK]
    D3 --> E3[🎬 Video Generation 3<br/>Vidu Q1 + CCSDK]
    D4 --> E4[🎬 Video Generation 4<br/>Vidu Q1 + CCSDK]
    E1 --> F[📝 Create PR<br/>プルリクエスト作成]
    E2 --> F
    E3 --> F
    E4 --> F
```

**特徴:**
- 最大4つまでの video_index 対応
- リソース集約的な実行
- 統一テーマでの多角的表現

### 5. `orchestrator-gemini-i2v-generation-analysis.yml`
**【Gemini統合版】Gemini CLI Action中心ワークフロー**

```mermaid
graph LR
    A[🚀 Setup<br/>ブランチ・フォルダ作成] --> B[🎯 Planning<br/>AI企画 Gemini CLI Action]
    B --> C[🎨 Image Generation<br/>画像生成 Imagen4 Fast + GCA]
    C --> D[🎬 Video Generation<br/>動画生成 Hailuo-02 Pro + GCA]
    D --> E[🔍 Video Analysis<br/>動画分析 Gemini Vision]
    E --> F[📝 Create PR<br/>プルリクエスト作成]
    
    style B fill:#e1f5fe
    style C fill:#e1f5fe
    style D fill:#e1f5fe
    style E fill:#e1f5fe
```

**特徴:**
- Claude Code SDK を使わずGemini CLI Actionで統一
- 異なるAIモデル組み合わせのテスト用
- Gemini APIキーのみで動作

### 6. `orchestrator-banner-advertisement-creation.yml`
**【バナー広告作成版】AI自動バナー生成ワークフロー**

```mermaid
graph LR
    A[🚀 Setup<br/>ブランチ・フォルダ作成] --> B[🎯 Banner Planning<br/>バナー企画 CCSDK]
    B --> C1[🎨 Base Image 1<br/>Imagen4 Ultra + CCSDK]
    B --> C2[🎨 Base Image 2<br/>Imagen4 Ultra + CCSDK]
    B --> C3[🎨 Base Image 3<br/>Imagen4 Ultra + CCSDK]
    B --> C4[🎨 Base Image 4<br/>Imagen4 Ultra + CCSDK]
    C1 --> D1[📝 Text Overlay 1<br/>Flux Kontext Max + CCSDK]
    C2 --> D2[📝 Text Overlay 2<br/>Flux Kontext Max + CCSDK]
    C3 --> D3[📝 Text Overlay 3<br/>Flux Kontext Max + CCSDK]
    C4 --> D4[📝 Text Overlay 4<br/>Flux Kontext Max + CCSDK]
    D1 --> E[📝 Create PR<br/>プルリクエスト作成]
    D2 --> E
    D3 --> E
    D4 --> E
    
    style B fill:#fff3e0
    style D1 fill:#fff3e0
    style D2 fill:#fff3e0
    style D3 fill:#fff3e0
    style D4 fill:#fff3e0
```

**特徴:**
- コンセプトとテキストから最大4つのバナーを生成
- 企画→ベース画像→テキスト合成の3段階品質管理
- Flux Kontext Maxによる高品質テキストオーバーレイ
- SNS、Web広告、印刷物など多用途対応

### 7. `orchestrator-multi-model-video-test.yml` 🆕
**【マルチモデル対応版】任意のAIモデル選択動画ワークフロー**

```mermaid
graph LR
    A[🚀 Setup<br/>ブランチ・フォルダ作成] --> B[🎯 Planning<br/>モデル適応型企画 CCSDK]
    B --> C{動画モデル<br/>タイプ判定}
    C -->|t2v-*| D1[🎬 Video Generation<br/>テキスト→動画]
    C -->|i2v-*/r2v-*| E[🎨 Image Generation<br/>任意モデル選択]
    E --> F[⚙️ Video Prompt Optimization<br/>プロンプト最適化]
    F --> D2[🎬 Video Generation<br/>画像→動画]
    D1 --> G[📝 Create PR<br/>プルリクエスト作成]
    D2 --> G
    
    style B fill:#e8f5e8
    style E fill:#e8f5e8
    style D1 fill:#e8f5e8
    style D2 fill:#e8f5e8
```

**特徴:**
- **t2v/i2v/r2v全動画タイプ対応**: テキスト→動画、画像→動画、参照動画生成
- **動的モデル選択**: `kamuicode-usage.md`から自動的にツール特定
- **モデル最適化**: 指定モデルの特性に合わせたプロンプト生成
- **条件分岐実行**: t2vは画像生成をスキップして効率化
- **📄 kamuicode-usage.md**: モデル情報管理ファイル（設置必須）
- **利用可能モデル**:
  - 画像: `t2i-google-imagen3`, `t2i-fal-imagen4-ultra`, `t2i-fal-imagen4-fast`, `t2i-fal-flux-schnell`, `t2i-fal-rundiffusion-photo-flux`
  - 動画: `t2v-fal-veo3-fast`, `i2v-fal-hailuo-02-pro`, `r2v-fal-vidu-q1`

### 8. `orchestrator-multi-model-image-test.yml` 🆕
**【マルチモデル対応版】任意のAIモデル選択画像ワークフロー**

```mermaid
graph LR
    A[🚀 Setup<br/>ブランチ・フォルダ作成] --> B[🎯 Planning<br/>モデル適応型企画 CCSDK]
    B --> C[🎨 Image Generation<br/>任意モデル選択]
    C --> D[📝 Create PR<br/>プルリクエスト作成]
    
    style B fill:#fff8e1
    style C fill:#fff8e1
```

**特徴:**
- **画像生成特化**: 動画生成を行わず画像のみに集中
- **全モデル対応**: 5種類の画像生成モデルから選択
- **高速実行**: 画像生成のみのため短時間で完了
- **モデル比較**: 同じプロンプトで異なるモデルの結果比較に最適
- **📄 kamuicode-usage.md**: モデル情報管理ファイル（設置必須）
- **利用可能モデル**:
  - `t2i-google-imagen3`: 高品質・写実的
  - `t2i-fal-imagen4-ultra`: 最高品質（商用利用）
  - `t2i-fal-imagen4-fast`: バランス型・高速
  - `t2i-fal-flux-schnell`: 超高速生成
  - `t2i-fal-rundiffusion-photo-flux`: フォトリアリスティック特化

## 🧩 モジュール詳細（22種類）🆕

各オーケストレータは以下のモジュールを組み合わせて動作します。

### 🆕 ニュース動画生成専門モジュール（v0.3.0新機能）

#### `module-news-planning-ccsdk.yml` 🆕
**ニュース企画立案**
```yaml
AI: Claude Code SDK
機能: プロフェッショナルニュース番組の制作企画
特徴: 
  - 原稿翻訳・アンカー設定・音声設定の自動化
  - タイトル生成（英語、30文字程度）・要約作成
  - 性別統一設定（女性アナウンサー）
出力: 音声プロンプト、画像プロンプト、動画コンセプト、タイトル情報
```

#### `module-audio-generation-kc-multi-model-ccsdk.yml` 🆕  
**マルチモデル音声生成**
```yaml
技術: kamuicode MCP + Claude Code SDK
機能: 複数の音声生成エンジン対応
特徴:
  - 動的モデル選択システム
  - JSON形式音声設定カスタマイズ
  - プロ品質ナレーション生成
依存: kamuicode-usage.md（モデル情報管理）
```

#### `module-lipsync-generation-kc-multi-model-ccsdk.yml` 🆕
**リップシンク生成**  
```yaml
技術: kamuicode MCP + Claude Code SDK
機能: 動画と音声の高精度同期
特徴:
  - マルチモデルリップシンク処理
  - JSON設定によるカスタマイズ
  - 自然な口の動きを生成
依存: kamuicode-usage.md（モデル情報管理）
```

#### `module-lipsync-video-analysis-gca.yml` 🆕
**リップシンク解析**
```yaml
AI: Gemini Vision (GCA)
機能: リップシンク動画の高精度解析
特徴:
  - 音声と口の動きから字幕タイミング決定
  - 字幕タイミング設定JSON自動生成
  - Gemini Visionによる高精度解析
出力: subtitle-timing.json, 動画解析レポート
```

#### `module-subtitle-overlay-ffmpeg-ccsdk.yml` 🆕  
**字幕オーバーレイ**
```yaml
技術: ffmpeg + Claude Code SDK
機能: 動画への字幕追加
特徴:
  - 日本語・英語字幕対応
  - タイミング設定に基づく正確な字幕表示
  - エラーハンドリング・ログ機能
処理: ffmpegによるプロレベル字幕処理
```

#### `module-video-title-frame-ffmpeg-ccsdk.yml` 🆕
**タイトルフレーム挿入**  
```yaml
技術: ffmpeg + Claude Code SDK
機能: 動画先頭へのタイトルフレーム追加
特徴:
  - 元動画の解像度・フレームレート維持
  - タイトル表示時間のカスタマイズ
  - 音声・映像の完全統合処理
処理: ffmpegによる高品質動画編集
```

### 📋 セットアップ・管理モジュール

#### `module-setup-branch.yml`
**ブランチ・フォルダ作成**
```yaml
機能: タイムスタンプベースの作業ブランチとフォルダを自動作成
出力: branch-name, folder-name
例: ブランチ "video/20250719-16387654321", フォルダ "video-20250719-16387654321"
```

#### `module-create-pr.yml`  
**プルリクエスト作成**
```yaml
機能: 生成した全コンテンツを美しいPRとして整理・公開
特徴: 画像・動画の埋め込み表示、詳細な制作レポート付き
```

#### `module-create-summary.yml`
**サマリー作成**  
```yaml
機能: プロジェクト全体のREADME.md生成
内容: 制作概要、ファイル一覧、品質評価結果
```

### 🎯 AI企画モジュール

#### `module-planning-ccsdk.yml` 🆕
**Claude Code SDK版 AI企画（モデル適応型）**
```yaml
AI: Claude (Opus/Sonnet)
機能: コンセプトから複数動画の制作計画を立案
出力: 各動画用の画像プロンプト（英語）+ ビデオコンセプト
対応: 最大8動画まで
特徴: 高度な創作性、詳細な企画書生成
新機能: image-model-type, video-model-type指定でモデル最適化
```

#### `module-planning-gca.yml`
**Gemini CLI Action版 AI企画**  
```yaml
AI: Gemini Pro  
機能: 同様の企画機能をGemini APIで実行
特徴: 高速処理、コスト効率重視
オプション: image-model-type, video-model-type指定でモデル最適化
```

### 🎨 画像生成モジュール

#### `module-image-generation-kc-t2i-fal-imagen4-ultra-ccsdk.yml`
**Imagen4 Ultra 高品質画像生成**
```yaml
AI: Google Imagen4 Ultra
品質: 最高品質（2048x2048対応）
用途: 商用利用、高解像度が必要な場合
処理時間: 中程度（2-3分）
特徴: 細部まで精密、写実的表現が得意
```

#### `module-image-generation-kc-t2i-fal-imagen4-fast-gca.yml`
**Imagen4 Fast 高速画像生成**
```yaml  
AI: Google Imagen4 Fast
品質: 高品質（1024x1024）
用途: プロトタイプ、迅速な確認
処理時間: 高速（1-2分）
特徴: バランス型、安定した品質
```

#### `module-image-generation-kc-multi-model-ccsdk.yml` 🆕
**マルチモデル対応画像生成**
```yaml
AI: 動的選択（kamuicode-usage.mdから自動特定）
対応モデル: Imagen3, Imagen4 Ultra/Fast, Flux Schnell, Rundiffusion Photo Flux
機能: model-type指定でMCPツールを動的選択
特徴: モデル非依存設計、新モデル自動対応
依存: kamuicode-usage.md（モデル情報管理ファイル）
用途: モデル比較、最適モデル選択実験
```

### 🎬 動画生成モジュール

#### `module-video-generation-kc-r2v-fal-vidu-q1-ccsdk.yml`
**Vidu Q1 参照動画生成**
```yaml
AI: Vidu Q1  
方式: 画像→動画（I2V）
品質: 720p, 4-6秒
特徴: 自然な動き、高い一貫性
用途: 汎用的な動画生成
```

#### `module-video-generation-kc-i2v-fal-hailuo-02-pro-gca.yml`
**Hailuo-02 Pro 高品質動画生成**
```yaml
AI: Hailuo-02 Pro
方式: 画像→動画（I2V）  
品質: 1080p, 4-6秒
特徴: 商用品質、細かい動作表現
用途: 高品質が要求される場面
```

#### `module-video-generation-kc-multi-model-ccsdk.yml` 🆕
**マルチモデル対応動画生成**
```yaml
AI: 動的選択（kamuicode-usage.mdから自動特定）
対応タイプ: t2v（テキスト→動画）、i2v（画像→動画）、r2v（参照動画）
対応モデル: Veo3 Fast, Hailuo-02 Pro, Vidu Q1
機能: model-type指定でMCPツールとパラメータを動的選択
特徴: 統合インターフェース、条件分岐処理
依存: kamuicode-usage.md（モデル情報管理ファイル）
用途: モデル比較、最適動画生成パイプライン構築
```

### 🔧 最適化・分析モジュール

#### `module-video-prompt-optimization-ccsdk.yml`
**動画プロンプト最適化**
```yaml
機能: 生成画像を分析し、最適な動画生成プロンプトを作成
AI: Claude (画像解析 + プロンプト生成)
効果: 動画生成精度の大幅向上
出力: 最適化されたプロンプト + 分析レポート
```

#### `module-video-analysis-gca.yml`
**動画品質分析**
```yaml
AI: Gemini Vision
分析観点: 商用利用での厳格な評価
評価項目:
  - 技術品質（解像度、フレームレート、色再現）
  - 視覚的インパクト（構図、ライティング、美感）  
  - コンテンツ適合性（ブランド安全性、ターゲット適合性）
  - 商用利用可能性（SNS広告、企業PR、TV CM適性）
出力: 詳細な改善提案付き評価レポート
```

### 📄 バナー生成モジュール

#### `module-banner-planning-ccsdk.yml`
**バナー制作企画**
```yaml
AI: Claude (Opus/Sonnet)
機能: コンセプトとテキストから複数バナーの制作計画を立案
出力: 各バナー用の画像プロンプト + レイアウト戦略
対応: 最大8バナーまで
特徴: テキスト配置エリアを考慮した企画設計
```

#### `module-banner-text-overlay-kc-i2i-fal-flux-kontext-max-ccsdk.yml`
**テキストオーバーレイ**
```yaml
AI: Flux Kontext Max
機能: ベース画像に指定テキストを高品質で合成
品質: 商用利用可能レベル
特徴: 一字一句変更せずにテキストを配置
用途: バナー広告、SNS投稿、プロモーション素材
処理: レイアウト・フォント・色彩の最適化
```

## 🏗️ システムアーキテクチャ

```mermaid
graph TB
    subgraph "オーケストレータ層"
        ON["🆕 orchestrator-news-video-generation.yml<br/>ニュース動画版"]
        O1["orchestrator-video-generation.yml<br/>1動画版"]
        O2["orchestrator-video-generation-dual.yml<br/>2動画版"]
        O3["orchestrator-video-generation-dual-with-analysis.yml<br/>2動画版+分析"]
        O4["orchestrator-video-generation-quad.yml<br/>4動画版"]
        O5["orchestrator-gemini-i2v-generation-analysis.yml<br/>Gemini統合版"]
        O6["orchestrator-banner-advertisement-creation.yml<br/>バナー広告版"]
        O7["🆕 orchestrator-multi-model-video-test.yml<br/>マルチモデル動画版"]
        O8["🆕 orchestrator-multi-model-image-test.yml<br/>マルチモデル画像版"]
    end
    
    subgraph "再利用可能モジュール層"
        subgraph "基本モジュール"
            M1["module-setup-branch.yml<br/>ブランチ・フォルダ作成"]
            M7["module-create-pr.yml<br/>PR作成"]
            M15["module-create-summary.yml<br/>サマリー作成"]
        end
        
        subgraph "AI企画モジュール"
            M2["module-planning-ccsdk.yml<br/>AI企画 (CCSDK)"]
            M2G["module-planning-gca.yml<br/>AI企画 (GCA)"]
        end
        
        subgraph "画像・動画生成モジュール"
            M3["module-image-generation-kc-t2i-fal-imagen4-ultra-ccsdk.yml<br/>画像生成 (Ultra)"]
            M3M["🆕 module-image-generation-kc-multi-model-ccsdk.yml<br/>マルチモデル画像生成"]
            M4["module-video-prompt-optimization-ccsdk.yml<br/>プロンプト最適化"]
            M5["module-video-generation-kc-r2v-fal-vidu-q1-ccsdk.yml<br/>動画生成 (Vidu)"]
            M5M["🆕 module-video-generation-kc-multi-model-ccsdk.yml<br/>マルチモデル動画生成"]
            M6["module-video-analysis-gca.yml<br/>動画品質分析"]
        end
        
        subgraph "バナー生成モジュール"
            M8["module-banner-planning-ccsdk.yml<br/>バナー企画"]
            M9["module-banner-text-overlay-kc-i2i-fal-flux-kontext-max-ccsdk.yml<br/>テキストオーバーレイ"]
        end
        
        subgraph "🆕 ニュース動画生成モジュール (v0.3.0)"
            M10["🆕 module-news-planning-ccsdk.yml<br/>ニュース企画立案"]
            M11["🆕 module-audio-generation-kc-multi-model-ccsdk.yml<br/>マルチモデル音声生成"]
            M12["🆕 module-lipsync-generation-kc-multi-model-ccsdk.yml<br/>リップシンク生成"]
            M13["🆕 module-lipsync-video-analysis-gca.yml<br/>リップシンク解析"]
            M14["🆕 module-subtitle-overlay-ffmpeg-ccsdk.yml<br/>字幕オーバーレイ"]
            M16["🆕 module-video-title-frame-ffmpeg-ccsdk.yml<br/>タイトルフレーム"]
        end
    end
    
    subgraph "AIエージェント処理層"
        A1["Claude Code SDK<br/>AI エージェント実行環境"]
        A2["Gemini CLI Action<br/>AI エージェント実行環境"]
    end
    
    subgraph "🆕 技術統合層"
        F1["🆕 ffmpeg<br/>プロ動画編集"]
        KC["Kamui Code MCP<br/>全画像・動画・音声・リップシンク生成"]
    end
    
    subgraph "外部AI APIサービス層"
        T2["Anthropic API<br/>Claude Opus/Sonnet"]
        T3["Google AI API<br/>Gemini Pro/Vision"]
    end
    
    %% ニュース動画オーケストレータの接続
    ON --> M1
    ON --> M10
    ON --> M3M
    ON --> M5M
    ON --> M11
    ON --> M12
    ON --> M13
    ON --> M14
    ON --> M16
    ON --> M7
    
    %% 基本オーケストレータの接続
    O1 --> M1
    O1 --> M2
    O1 --> M3
    O1 --> M4
    O1 --> M5
    O1 --> M7
    
    %% バナーオーケストレータの接続
    O6 --> M1
    O6 --> M8
    O6 --> M3
    O6 --> M9
    O6 --> M7
    
    %% マルチモデルオーケストレータの接続
    O7 --> M1
    O7 --> M2
    O7 --> M3M
    O7 --> M5M
    O7 --> M7
    
    O8 --> M1
    O8 --> M2
    O8 --> M3M
    O8 --> M7
    
    %% AIエージェントへの接続
    M2 --> A1
    M2G --> A2
    M3 --> A1
    M3M --> A1
    M4 --> A1
    M5 --> A1
    M5M --> A1
    M6 --> A2
    M8 --> A1
    M9 --> A1
    M10 --> A1
    M11 --> A1
    M12 --> A1
    M13 --> A2
    M14 --> A1
    M16 --> A1
    
    %% 技術層への接続
    A1 --> KC
    A2 --> KC
    M14 --> F1
    M16 --> F1
    
    %% 外部APIへの接続
    A1 --> T2
    A2 --> T3
    KC --> T2
    KC --> T3
```

## 📄 ライセンス

MIT License - 商用利用、改変、再配布が可能です。


## 🏷️ バージョン履歴

- **v0.0.1**: 初回リリース

---

🎬 **AI動画生成の新時代を、kamuicode-workflowで体験してください！**