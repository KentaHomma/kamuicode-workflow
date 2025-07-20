# AI Video Generation Workflow

AI video generation workflow using Claude Code SDK and kamuicode MCP

## Overview
This GitHub Actions workflow automatically executes the following:
1. **Imagen4 Ultra** high-quality image generation
2. **Vidu Q1** reference video generation
3. Submit results as pull request

## Workflows

### 1. `create-video-from-prompt.yml`
Manual execution video generation workflow. Run directly from GitHub Actions.

### 2. `issue-video-trigger.yml`
Issue-triggered workflow. Write in Issues or comments in the following format:

```
@create-video-i2v
Concept: [Describe video concept or content]
```

## Required Configuration

### Secrets
- `CLAUDE_CODE_OAUTH_TOKEN`: Claude API key
- `PAT_TOKEN`: (Optional) GitHub Personal Access Token

### MCP Configuration
`.claude/mcp-kamuicode.json` file is required

### Repository Permissions
Configure the following in Settings → Actions → General:
- **Workflow permissions**: "Read and write permissions" (for branch creation and file saving)
- **Allow GitHub Actions to create and approve pull requests**: ✓ (for automatic PR creation)

## Setup

1. Copy workflow files to `.github/workflows/`
2. Copy Issue template to `.github/ISSUE_TEMPLATE/` (optional)
3. Configure required Secrets

## Usage

### Method 1: Manual Execution
1. GitHub Actions → Select `Create Video from Prompt`
2. Click `Run workflow`
3. Enter prompt and generate video

### Method 2: Via Issue
1. Create new Issue
2. Select template "AI Video Generation Request (I2V)"
3. Fill in concept and submit
4. Video generation starts automatically

### Method 3: Comment Trigger
In existing Issue or PR comments:
```
@create-video-i2v
Concept: [Video concept]
```

## License
MIT License

---

🎬 **AI Video Generation Workflow** - Powered by [Claude Code SDK](https://github.com/anthropics/claude-code) & [kamuicode MCP](https://www.kamui.ai/ja)