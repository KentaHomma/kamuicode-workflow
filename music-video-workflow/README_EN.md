# AI Music Video Generator Workflow

Music-driven AI music video generation workflow

## 🎵 Overview

This GitHub Actions workflow automatically generates complete music videos from **music concepts**.

### 🎯 Generation Flow

1. **🎼 Music Generation** - Generate 30-40 second music with Google Lyria
2. **🎨 Image Generation** - Generate 3 images matching the music (Imagen4 Fast)
3. **🎬 Video Generation** - Generate 3x 5-second videos from images (Hailuo-02 Pro)
4. **🎞️ Video Integration** - Edit, concatenate, and integrate videos to match music length

### 🔧 Technology Stack

- **Music Generation**: Google Lyria (30-40 seconds, high quality)
- **Image Generation**: Imagen4 Fast (1024x1024, high speed)
- **Video Generation**: Hailuo-02 Pro (5 seconds, high quality)
- **Video Editing**: FFmpeg (concatenation, music integration)
- **AI Integration**: Claude Code SDK + kamuicode MCP

## 🚀 Usage

### Manual Execution

1. Navigate to repository **Actions** tab
2. Select **Create AI Music Video** workflow
3. Click **Run workflow** button
4. Enter music concept (e.g., "Cyberpunk city nightscape, electronic")
5. Execute **Run workflow**

## 📁 Output Structure

```
[music-concept]-[timestamp]/
├── music/
│   ├── generated-music.wav        # Generated music
│   └── music-url.txt             # Music file URL
├── images/
│   ├── segment-1.png             # Image 1 (main)
│   ├── segment-2.png             # Image 2 (accent)
│   └── segment-3.png             # Image 3 (transition)
├── videos/
│   ├── segment-1.mp4             # Video 1 (5 seconds)
│   ├── segment-2.mp4             # Video 2 (5 seconds)
│   └── segment-3.mp4             # Video 3 (5 seconds)
├── final/
│   └── final-music-video.mp4     # Final music video
├── planning/
│   ├── music-video-strategy.md   # Strategy planning
│   └── video-strategy.txt        # Video strategy
└── analysis/
    └── music-analysis.md         # Music analysis results
```

## 🔧 Setup Requirements

### Required Secrets

```yaml
CLAUDE_CODE_OAUTH_TOKEN: # Claude API Key
PAT_TOKEN: # GitHub Personal Access Token
```

### Required Files

```
.claude/
└── mcp-kamuicode.json    # kamuicode MCP configuration
```

### Permission Configuration

```yaml
permissions:
  contents: write
  pull-requests: write
  actions: read
  issues: write
```

## 🎵 Music Concept Examples

### 🌆 Urban/Future

```
"Cyberpunk city nightscape, electronic, neon lights"
"Future Tokyo, synthwave, 80s retro"
```

### 🎮 Game/Anime

```
"Robot dance, techno music, mechanical"
"Magical forest, Celtic music, fantasy"
```

### 🌊 Nature/Environment

```
"Ocean waves, ambient, relaxing"
"Outer space, electronic, epic"
```

## 🤖 Technical Details

### AI Agent Configuration
- **Music Planning Agent**: Music concept analysis and strategy planning
- **Music Generation Agent**: Google Lyria execution
- **Music Analysis Agent**: Generated music structure analysis
- **Image Generation Agent**: Imagen4 Fast execution (3 parallel)
- **Video Generation Agent**: Hailuo-02 Pro execution (3 parallel)
- **Video Integration Agent**: FFmpeg execution and final integration

### Parallel Processing Optimization
- Image generation: 3 parallel executions
- Video generation: 3 parallel executions
- Dependencies: Music → Images → Videos → Integration

## 📄 License

MIT License

## 👥 Contributing

Issues and Pull Requests are welcome!

---

🎵 **AI-generated Music Video Workflow** - Powered by [Claude Code SDK](https://github.com/anthropics/claude-code) & [kamuicode MCP](https://www.kamui.ai/ja)