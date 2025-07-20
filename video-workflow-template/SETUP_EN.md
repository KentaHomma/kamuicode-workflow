# Setup Guide

## Step 1: Repository Clone and Setup

### 1.1 Repository Clone

```bash
# Clone the repository
git clone https://github.com/KentaHomma/kamuicode-workflow.git
```

## Directory Structure

```
your-repo/
├── .github/
│   ├── workflows/
│   │   ├── create-video-from-prompt.yml    # Manual execution workflow
│   │   └── issue-video-trigger.yml         # Issue-triggered workflow
│   └── ISSUE_TEMPLATE/
│       └── video-request.md                 # Issue template
└── .claude/
    └── mcp-kamuicode.json                   # MCP configuration (requires separate setup)
```

## Steps

### 1.2 File Copy

```bash
# Create workflow directory
mkdir -p .github/workflows
mkdir -p .github/ISSUE_TEMPLATE

# Copy workflow files
cp kamuicode-workflow/video-workflow-template/create-video-from-prompt.yml .github/workflows/
cp kamuicode-workflow/video-workflow-template/issue-video-trigger.yml .github/workflows/

# Issue template (optional)
cp kamuicode-workflow/video-workflow-template/.github/ISSUE_TEMPLATE/video-request.md .github/ISSUE_TEMPLATE/

# Verify files were copied correctly
ls -la .github/workflows/
ls -la .github/ISSUE_TEMPLATE/
```

### 2. GitHub Secrets Configuration

#### 2.1 Required Secrets

| Secret Name | Description | How to Obtain |
|-------------|-------------|---------------|
| `CLAUDE_CODE_OAUTH_TOKEN` | Claude API Key (Required) | Create API Key at [Anthropic Console](https://console.anthropic.com/) |
| `PAT_TOKEN` | GitHub Personal Access Token (Optional) | Settings → Developer settings → Personal access tokens → Tokens (classic) |

#### 2.2 How to Obtain CLAUDE_CODE_OAUTH_TOKEN

1. Access [Anthropic Console](https://console.anthropic.com/)
2. Login or create account
3. Click "API Keys" in left menu
4. Click "Create Key"
5. Enter key name (e.g., `github-actions-video-generation`)
6. Copy the created API key (⚠️ Only displayed on this screen)

#### 2.3 How to Obtain PAT_TOKEN (Optional)

1. Login to GitHub
2. Settings → Developer settings → Personal access tokens → Tokens (classic)
3. Click "Generate new token (classic)"
4. Select the following permissions:
   - `repo` (Full repository access)
   - `workflow` (GitHub Actions workflow updates)
5. Click "Generate token"
6. Copy the created token (⚠️ Only displayed on this screen)

#### 2.4 Secrets Configuration Steps

**Two methods available:**

**Method 1: GitHub CLI (Recommended - Easy)**

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

**Method 2: GitHub Web UI (Traditional)**

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

#### 2.5 Configuration Verification

After configuration is complete, verify the following appears on the Secrets page:
- ✅ `CLAUDE_CODE_OAUTH_TOKEN` (Updated X minutes ago)
- ✅ `PAT_TOKEN` (Updated X minutes ago) ※If configured

### 3. Claude Code Configuration Preparation

#### 3.1 MCP Configuration File Placement

```bash
# Create Claude configuration directory
mkdir -p .claude

# Place kamuicode MCP configuration file
cp kamuicode-workflow/video-workflow-template/.claude/mcp-kamuicode.json .claude/

# Verify configuration file was placed correctly
ls -la .claude/
```

#### 3.2 Claude Code Permissions Configuration (Important)

**Permission Analysis Method:**
This configuration was created by analyzing all processes that Claude Code actually executes in detail. This is crucial.

**Correct Analysis Procedure:**
1. **Analyze --allowedTools parameters** - Understand basic allowed tools
2. **Analyze Claude Code instruction content** - Check what is actually executed in PROMPT
3. **Identify additional Bash commands** - Identify downloads, file operations, system commands, etc.
4. **Comprehensive permission configuration** - Cover all necessary permissions

Analyzing only `--allowedTools` is insufficient; you need to analyze the actual processes that Claude Code executes.

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
- `mcp__t2m-google-lyria__*` - Music generation

**Important**: This configuration has been verified to work with actual workflows.

**🔒 Security Notes:**
- File deletion commands (`rm`, `rmdir`, `mv`, etc.) are not permitted
- Dangerous git commands (`git reset`, `git clean`, `git rm`, etc.) are excluded, only necessary ones are allowed
- Mainly read-only commands and minimum required write permissions only
- Execution environment is GitHub Actions isolated environment, so no impact on local systems

### 4. GitHub Permissions Configuration (If Needed)

**In most cases, this is already ON by default in new repositories and no configuration is needed.**

Only check the following if workflow fails with permission errors:

**Settings** → **Actions** → **General** → **Workflow permissions**
- ✅ Select "Read and write permissions"  
- ✅ Check "Allow GitHub Actions to create and approve pull requests"

**Common Errors:**
- `Permission denied to create branch` → Check above settings
- `Resource not accessible by integration` → Check PR creation permissions

## Usage

### Using Issue Template

1. Issues → New Issue
2. Select "AI Video Generation Request (I2V)"
3. Fill in concept and submit

### Manual Execution

1. Actions → Create Video from Prompt
2. Run workflow → Enter prompt → Run

### Comment Trigger

```markdown
@create-video-i2v
Concept: Silhouette walking along sunset coastline
```

## Troubleshooting

### Workflow doesn't start
- Verify Secrets are configured correctly
- Verify workflow files are in default branch

### Video generation fails
- Verify API key validity
- Verify MCP configuration file content
- Check Actions logs

### Cannot trigger from Issues
- Verify `issue-video-trigger.yml` exists
- Verify repository permission settings