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

## 🎭 オーケストレータ（5種類）

オーケストレータは複数のモジュールを組み合わせて、特定の目的に最適化されたワークフローを実行します。

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

## 🧩 モジュール詳細（12種類）

各オーケストレータは以下のモジュールを組み合わせて動作します。

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

#### `module-planning-ccsdk.yml` 
**Claude Code SDK版 AI企画**
```yaml
AI: Claude (Opus/Sonnet)
機能: コンセプトから複数動画の制作計画を立案
出力: 各動画用の画像プロンプト（英語）+ ビデオコンセプト
対応: 最大8動画まで
特徴: 高度な創作性、詳細な企画書生成
```

#### `module-planning-gca.yml`
**Gemini CLI Action版 AI企画**  
```yaml
AI: Gemini Pro  
機能: 同様の企画機能をGemini APIで実行
特徴: 高速処理、コスト効率重視
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
        O1["orchestrator-video-generation.yml<br/>1動画版"]
        O2["orchestrator-video-generation-dual.yml<br/>2動画版"]
        O3["orchestrator-video-generation-dual-with-analysis.yml<br/>2動画版+分析"]
        O4["orchestrator-video-generation-quad.yml<br/>4動画版"]
        O5["orchestrator-gemini-i2v-generation-analysis.yml<br/>Gemini統合版"]
        O6["orchestrator-banner-advertisement-creation.yml<br/>バナー広告版"]
    end
    
    subgraph "再利用可能モジュール層"
        M1["module-setup-branch.yml<br/>ブランチ・フォルダ作成"]
        M2["module-planning-ccsdk.yml<br/>AI企画"]
        M3["module-image-generation-kc-t2i-fal-imagen4-ultra-ccsdk.yml<br/>画像生成"]
        M4["module-video-prompt-optimization-ccsdk.yml<br/>プロンプト最適化"]
        M5["module-video-generation-kc-r2v-fal-vidu-q1-ccsdk.yml<br/>動画生成"]
        M6["module-video-analysis-gca.yml<br/>動画品質分析"]
        M7["module-create-pr.yml<br/>PR作成"]
        M8["module-banner-planning-ccsdk.yml<br/>バナー企画"]
        M9["module-banner-text-overlay-kc-i2i-fal-flux-kontext-max-ccsdk.yml<br/>テキストオーバーレイ"]
    end
    
    subgraph "AIエージェント処理層"
        A1["Claude Code SDK<br/>AI エージェント実行環境"]
        A2["Gemini CLI Action<br/>Google AI実行環境"]
    end
    
    subgraph "外部AI APIサービス層"
        T1["kamuicode MCP<br/>Imagen4, Vidu Q1, Hailuo-02"]
        T2["Anthropic API<br/>Claude Opus/Sonnet"]
        T3["Google AI API<br/>Gemini Pro/Vision"]
    end
    
    O1 --> M1
    O1 --> M2
    O1 --> M3
    O1 --> M4
    O1 --> M5
    O1 --> M7
    
    O2 --> M1
    O2 --> M2
    O2 --> M3
    O2 --> M4
    O2 --> M5
    O2 --> M7
    
    O3 --> M1
    O3 --> M2
    O3 --> M3
    O3 --> M4
    O3 --> M5
    O3 --> M6
    O3 --> M7
    
    O6 --> M1
    O6 --> M8
    O6 --> M3
    O6 --> M9
    O6 --> M7
    
    M2 --> A1
    M3 --> A1
    M4 --> A1
    M5 --> A1
    M8 --> A1
    M9 --> A1
    
    M6 --> A2
    
    A1 --> T1
    A1 --> T2
    A2 --> T3
    
    T1 --> T2
    T1 --> T3
```

## 📄 ライセンス

MIT License - 商用利用、改変、再配布が可能です。


## 🏷️ バージョン履歴

- **v0.0.1**: 初回リリース

---

🎬 **AI動画生成の新時代を、kamuicode-workflowで体験してください！**