# Setup Guide

AI Music Video Generator Workflow Setup Instructions

## 📋 Prerequisites

- GitHub repository (with Actions enabled)
- Claude API access
- kamuicode MCP server configuration

## 🔧 Step 1: Repository Clone and Configuration

### 1.1 Repository Clone

```bash
# Clone the repository
git clone https://github.com/KentaHomma/kamuicode-workflow.git
```

### 1.2 Workflow File Setup

```bash
# Create workflow directory
mkdir -p .github/workflows

# Copy workflow file
cp kamuicode-workflow/music-video-workflow/create-music-video.yml .github/workflows/

# Verify file was copied correctly
ls -la .github/workflows/
```

### 1.3 Claude Code Configuration

#### MCP Configuration File Setup

```bash
# Create Claude configuration directory
mkdir -p .claude

# Copy kamuicode MCP configuration file
# .claude/mcp-kamuicode.json configuration is required
cp kamuicode-workflow/music-video-workflow/.claude/mcp-kamuicode.json .claude/

# Verify configuration file was placed correctly
ls -la .claude/
```

#### Claude Code Permissions Configuration (Important)

**Permission Analysis Method:**
This configuration was created by analyzing all processes that Claude Code actually executes in detail. This is crucial.

**Correct Analysis Procedure:**
1. **Analyze --allowedTools parameters** - Understand basic allowed tools
2. **Analyze Claude Code instruction content** - Check what is actually executed in PROMPT
3. **Identify additional Bash commands** - Identify downloads, video editing, system commands, etc.
4. **Comprehensive permission configuration** - Cover all necessary permissions

Analyzing only `--allowedTools` is insufficient; you need to analyze the actual processes (download, ffmpeg, sleep, etc.) that Claude Code executes.

Create `.claude/settings.json` file and configure permissions for tools used in the workflow:

```bash
# Create settings.json file
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
      "mcp__t2i-fal-imagen4-ultra__imagen4_ultra_submit",
      "mcp__t2i-fal-imagen4-ultra__imagen4_ultra_status", 
      "mcp__t2i-fal-imagen4-ultra__imagen4_ultra_result",
      "mcp__t2i-fal-imagen4-fast__imagen4_fast_submit",
      "mcp__t2i-fal-imagen4-fast__imagen4_fast_status",
      "mcp__t2i-fal-imagen4-fast__imagen4_fast_result",
      "mcp__r2v-fal-vidu-q1__vidu_q1_submit",
      "mcp__r2v-fal-vidu-q1__vidu_q1_status",
      "mcp__r2v-fal-vidu-q1__vidu_q1_result",
      "mcp__t2v-fal-veo3-fast__veo3_fast_submit",
      "mcp__t2v-fal-veo3-fast__veo3_fast_status",
      "mcp__t2v-fal-veo3-fast__veo3_fast_result",
      "mcp__i2v-fal-hailuo-02-pro__hailuo_02_submit",
      "mcp__i2v-fal-hailuo-02-pro__hailuo_02_status",
      "mcp__i2v-fal-hailuo-02-pro__hailuo_02_result",
      "mcp__t2m-google-lyria__lyria_generate"
    ]
  }
}
EOF

# Verify configuration file was created
cat .claude/settings.json
```

**⚠️ Important**: Without this configuration, Claude Code cannot use MCP tools and the workflow will fail.

**Workflow Analysis Results - Verified Working Permissions List:**

**Basic Bash Commands:**
- `Bash(curl:*)` - File downloads
- `Bash(wget:*)` - Alternative download method
- `Bash(sleep:*)` - Wait processing
- `Bash(stat:*)` - File information check
- `Bash(find:*)` - File search
- `Bash(ls:*)` - Directory listing
- `Bash(cat:*)` - File content display
- `Bash(head:*)` - File header display
- `Bash(mkdir:*)` - Directory creation
- `Bash(date:*)` - Date/time retrieval
- `Bash(jq:*)` - JSON processing
- `Bash(tr:*)` - String conversion
- `Bash(wc:*)` - Character/line counting
- `Bash(echo:*)` - Output
- `Bash(npx:*)` - Node.js execution
- `Bash(open:*)` - File opening

**Safe Git Commands (Minimum Required):**
- `Bash(git:checkout:*)` - Branch creation/switching
- `Bash(git:config:*)` - User configuration
- `Bash(git:push:*)` - Push to remote
- `Bash(git:add:*)` - Staging
- `Bash(git:diff:*)` - Change verification
- `Bash(git:commit:*)` - Commit
- `Bash(git:pull:*)` - Pull from remote

**MCP Tools (Generative AI):**
- `mcp__t2i-fal-imagen4-ultra__*` - High-quality image generation
- `mcp__t2i-fal-imagen4-fast__*` - Fast image generation
- `mcp__r2v-fal-vidu-q1__*` - Reference video generation
- `mcp__t2v-fal-veo3-fast__*` - Text-to-video generation
- `mcp__i2v-fal-hailuo-02-pro__*` - Image-to-video generation
- `mcp__t2m-google-lyria__*` - Music generation

**Important**: This configuration has been verified to work with actual workflows.

**🔒 Security Notes:**
- File deletion commands (`rm`, `rmdir`, `mv`, etc.) are not permitted
- Dangerous git commands (`git reset`, `git clean`, `git rm`, etc.) are excluded, only necessary ones are allowed
- Mainly read-only commands and minimum required write permissions only
- Execution environment is GitHub Actions isolated environment, so no impact on local systems

## 🔐 Step 2: Secrets Configuration

### 2.1 Required Secrets

The following keys need to be configured:

| Secret Name | Description | How to Obtain |
|-------------|-------------|---------------|
| `CLAUDE_CODE_OAUTH_TOKEN` | Claude API Key (Required) | Create API Key at [Anthropic Console](https://console.anthropic.com/) |
| `PAT_TOKEN` | GitHub Personal Access Token (Optional) | Settings → Developer settings → Personal access tokens → Tokens (classic) |

### 2.2 How to Obtain CLAUDE_CODE_OAUTH_TOKEN

1. Access [Anthropic Console](https://console.anthropic.com/)
2. Login or create account
3. Click "API Keys" in left menu
4. Click "Create Key"
5. Enter key name (e.g., `github-actions-music-video`)
6. Copy the created API key (⚠️ Only displayed on this screen)

### 2.3 How to Obtain PAT_TOKEN (Optional)

1. Login to GitHub
2. Settings → Developer settings → Personal access tokens → Tokens (classic)
3. Click "Generate new token (classic)"
4. Select the following permissions:
   - `repo` (Full repository access)
   - `workflow` (GitHub Actions workflow updates)
5. Click "Generate token"
6. Copy the created token (⚠️ Only displayed on this screen)

### 2.4 Secrets Configuration Steps

**Two methods available:**

#### Method 1: GitHub CLI (Recommended - Easy)

**Prerequisites:**
- GitHub CLI (`gh`) installed
- Authenticated with `gh auth login`

```bash
# If current directory is within repository
gh secret set CLAUDE_CODE_OAUTH_TOKEN --app actions
# ↑ After execution, safely input API key (not displayed on screen)

# PAT_TOKEN (if needed)
gh secret set PAT_TOKEN --app actions

# Verify configuration
gh secret list --app actions
```

**To set for specific repository:**
```bash
gh secret set CLAUDE_CODE_OAUTH_TOKEN --app actions --repo owner/repo-name
```

#### Method 2: GitHub Web UI (Traditional)

1. Access **GitHub repository page**
2. Click **Settings** tab
3. Click **Secrets and variables** → **Actions** in left sidebar
4. Click **New repository secret**
5. Add the following in order:

**Adding CLAUDE_CODE_OAUTH_TOKEN:**
- **Name**: `CLAUDE_CODE_OAUTH_TOKEN`
- **Secret**: Claude API key copied earlier
- Click **Add secret**

**Adding PAT_TOKEN (if needed):**
- **Name**: `PAT_TOKEN`  
- **Secret**: Personal Access Token copied earlier
- Click **Add secret**

### 2.5 Configuration Verification

After configuration is complete, verify the following appears on the Secrets page:
- ✅ `CLAUDE_CODE_OAUTH_TOKEN` (Updated X minutes ago)
- ✅ `PAT_TOKEN` (Updated X minutes ago) ※If configured

## 📁 Step 3: Directory Structure

```
your-repo/
├── .github/
│   └── workflows/
│       └── create-music-video.yml
├── .claude/
│   └── mcp-kamuicode.json
├── README.md
└── (other files)
```

## 🎛️ Step 4: GitHub Permissions Configuration (If Needed)

**In most cases, this is already ON by default in new repositories and no configuration is needed.**

Only check the following if workflow fails with permission errors:

**Settings** → **Actions** → **General** → **Workflow permissions**
- ✅ Select "Read and write permissions"
- ✅ Check "Allow GitHub Actions to create and approve pull requests"

**Common Errors:**
- `Permission denied to create branch` → Check above settings
- `Resource not accessible by integration` → Check PR creation permissions

## 🧪 Step 5: Test Execution

### 5.1 Manual Test

1. Navigate to **Actions** tab
2. Select **Create AI Music Video** workflow
3. Click **Run workflow**
4. Enter music concept (e.g., "Cyberpunk city techno music")
5. Execute **Run workflow**

### 5.2 Operation Verification

After execution, verify the following:

- [ ] Music file is generated
- [ ] 3 images are generated
- [ ] 3 videos are generated
- [ ] Final music video is generated
- [ ] Pull Request is automatically created

---

**Support:**
- Issue reporting: GitHub Issues
- Documentation: README.md
- Usage examples: EXAMPLES.md