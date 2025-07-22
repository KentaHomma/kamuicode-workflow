# kamuicode Workflows

AI content generation workflow templates using Claude Code SDK and kamuicode MCP

## 🌟 Overview

A collection of workflow templates for generating AI content using Claude Code SDK and kamuicode MCP in GitHub Actions. Choose the optimal workflow based on your specific needs.

**🆕 v0.2.0 New Features**: Multi-model support enables free selection from 5 image generation models and 3 video generation models for optimal content creation combinations.

## 📦 Workflow List

### 🏗️ [Module Workflow](./module-workflow/) 🆕 v0.2.0
Modularized AI video & banner generation system. Efficiently generates high-quality content through reusable components and multi-agent collaboration.

**🆕 Latest Features (v0.2.0-multi-model):**
- **Multi-model Support**: Free selection from 5 image models & 3 video models
- **t2v/i2v/r2v Integration**: Text-to-video, image-to-video, reference video generation
- **Dynamic Model Selection**: Automatic tool detection from kamuicode-usage.md
- **Model Optimization**: Auto-generated prompts tailored to each AI model's characteristics

**Key Features:**
- **Video Generation**: High-quality video production with any AI model combination
- **Banner Generation**: Automatically create up to 4 banner ads from concept and text
- **Quality Analysis**: Commercial-grade evaluation using Gemini Vision
- **8 Orchestrators**: Optimal workflow selection for different use cases

**⚠️ Note**: Modules may be continuously adjusted for improved reusability and quality.

### 🎵 [Music Video Workflow](./music-video-workflow/)
Automatic music video generation combining audio and video. Google Lyria + Imagen4 + Hailuo-02 Pro integration.

### 📹 [Video Workflow Template](./video-workflow-template/)
Basic video generation workflow. Perfect for learning purposes and simple video creation.

### 🎬 [Video with Background Removal](./video-background-removal-workflow/)
Video generation workflow with background removal functionality.

### 🔍 [Gemini I2V Analysis](./gemini-i2v-workflow/)
Image-to-video generation analysis workflow integrated with Gemini API.

## 🚀 Quick Start

1. **Select a workflow for your use case**
2. **Refer to SETUP.md in the respective folder**
3. **Add required Secrets and MCP configuration**
4. **Run the workflow**

## 📋 Workflow Selection Guide

| Use Case | Recommended Workflow | Features |
|----------|---------------------|----------|
| 🆕 Multi-model video generation | Module Workflow (Multi-Model) | Any AI model selection, t2v/i2v/r2v support |
| High-quality, large-scale generation | Module Workflow | Modular, multi-agent |
| Banner advertisement creation | Module Workflow (Banner) | Auto-generate up to 4 banners from concept |
| Music video creation | Music Video | Audio+video integration |
| Learning, basic use | Video Template | Simple, easy to adopt |
| Background removal needed | Background Removal | Special processing |
| Analysis-focused | Gemini I2V | Gemini integration |

## 🛠️ Requirements

- **Claude Code SDK**: AI agent execution environment
- **kamuicode MCP**: Image, video, music generation services (multi-model support)
- **Anthropic API Key**: Claude AI access
- **GitHub Actions**: Workflow execution environment
- **GitHub PAT Token**: Pull request creation

### 🆕 Multi-Model Support
kamuicode MCP supports 5 image generation models and 3 video generation models, enabling optimal AI selection for different use cases.

## 🤝 Contributing

We welcome PRs for new workflow templates and bug fixes.

## 📄 License

MIT License

---

🤖 **Powered by [Claude Code SDK](https://docs.anthropic.com/en/docs/claude-code) & kamuicode MCP**