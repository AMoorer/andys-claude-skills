---
name: claude-desktop-github-skill-grabber
description: Downloads Claude skills from GitHub repositories, zips each skill folder, and saves them to a specified location for easy upload to Claude Desktop
---

# Claude Desktop GitHub Skill Grabber

This skill automates the process of downloading Claude skills from GitHub repositories and preparing them for upload to Claude Desktop.

## Purpose

When users want to download multiple skills from a GitHub repository containing Claude skills, this skill:
1. Downloads the entire repository
2. Identifies all folders containing SKILL.md files
3. Creates individual ZIP files for each skill
4. Saves them to a user-specified directory
5. Provides a summary of what was downloaded

## When to Use This Skill

Use this skill when the user:
- Wants to download skills from a GitHub repository
- Mentions getting skills from git/GitHub
- Asks to "grab skills" or "download skills" from a repository
- Wants to prepare skills for upload to Claude Desktop
- Provides a GitHub repository URL and wants to extract skills from it

## Required Information

To execute this skill, you need:
1. **Repository URL** - The GitHub repository containing Claude skills (e.g., `https://github.com/user/repo`)
2. **Target Directory** - Where to save the zipped skills (e.g., `Z:\LIBRARY\SOFTWARE\CLAUDE\SKILLS`)

If the user doesn't provide either of these, ask for them.

## Execution Process

### Step 1: Validate Input
- Confirm the GitHub repository URL is valid
- Confirm the target directory path
- If target directory doesn't exist, create it

### Step 2: Download Repository
Use PowerShell commands to:
```powershell
$repoZipUrl = "https://github.com/[user]/[repo]/archive/refs/heads/[branch].zip"
# Download the repository as a ZIP file
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
$webClient = New-Object System.Net.WebClient
$webClient.DownloadFile($repoZipUrl, $tempZipFile)
```

**Important**: For GitHub repository URLs:
- Convert `https://github.com/user/repo` to `https://github.com/user/repo/archive/refs/heads/master.zip`
- Or use `main` instead of `master` if that's the default branch
- The user may provide a URL to a specific branch or folder - handle accordingly

### Step 3: Extract and Identify Skills
```powershell
# Extract the downloaded ZIP
Expand-Archive -Path $zipFile -DestinationPath $extractDir -Force

# Find all directories containing SKILL.md
$skillFolders = Get-ChildItem -Path $repoFolder.FullName -Directory | 
    Where-Object { Test-Path "$($_.FullName)\SKILL.md" }
```

### Step 4: Zip Each Skill
```powershell
foreach ($folder in $skillFolders) {
    $skillName = $folder.Name
    $zipPath = Join-Path $targetDir "$skillName.zip"
    
    # Remove existing zip if it exists
    if (Test-Path $zipPath) {
        Remove-Item $zipPath -Force
    }
    
    # Compress the folder
    Compress-Archive -Path $folder.FullName -DestinationPath $zipPath -CompressionLevel Optimal
}
```

### Step 5: Clean Up and Report
- Remove temporary files
- Report the number of skills found and processed
- List all the skill names that were zipped
- Remind the user how to upload them to Claude Desktop

## Output Format

After processing, provide output like:

```
✓ Downloaded repository: [repo-name]
✓ Found [X] skills
✓ Zipped all skills to: [target-directory]

Skills downloaded:
  • skill-name-1
  • skill-name-2
  • skill-name-3
  ...

Next steps:
1. Open Claude Desktop
2. Go to Settings → Capabilities → Skills
3. Click "Upload skill" and select the ZIP files from:
   [target-directory]
```

## Error Handling

Handle these common errors:
- **Invalid GitHub URL**: Ask the user to provide a valid GitHub repository URL
- **Repository doesn't contain skills**: Inform the user that no folders with SKILL.md were found
- **Target directory not accessible**: Verify the path is valid and writable
- **Network errors**: Inform the user if download fails

## Example Usage

**User**: "Grab the skills from https://github.com/ComposioHQ/awesome-claude-skills and put them in Z:\LIBRARY\SOFTWARE\CLAUDE\SKILLS"

**Claude**: 
1. Identifies this as a skill-grabbing request
2. Extracts repo URL and target directory
3. Downloads and processes
4. Reports results

## Technical Notes

- Use Windows PowerShell commands (user is on Windows)
- Use `$env:TEMP` for temporary file operations
- Always clean up temporary files after processing
- Default to `master` branch but try `main` if master fails
- Handle both full repo URLs and tree/directory URLs if possible

## Platform Compatibility

This skill is designed for Windows systems with PowerShell. For other platforms, adapt the commands accordingly:
- **macOS/Linux**: Use bash with `curl`, `unzip`, and `zip` commands
- Detect the platform using `$IsWindows`, `$IsMacOS`, `$IsLinux` PowerShell variables

## Safety Considerations

- Only process repositories from trusted sources
- Preview the skills found before zipping (show the list)
- Don't execute any code from the downloaded skills, only package them
- Warn users to audit skills before uploading to Claude Desktop