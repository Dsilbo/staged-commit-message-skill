# staged-commit-message-skill

本仓库发布一个 Codex skill：`staged-commit-message`。  
This repository publishes a Codex skill: `staged-commit-message`.

## 仓库结构 / Repository Structure

```text
staged-commit-message-skill/
  README.md
  staged-commit-message/
    SKILL.md
    agents/
      openai.yaml
```

## 安装 / Install

### 方式一：手动安装 / Manual Install

1. 下载或克隆本仓库。  
   Download or clone this repository.
2. 将 `staged-commit-message` 目录复制到你的 Codex skills 目录。  
   Copy the `staged-commit-message` directory into your Codex skills directory.
   
   - Windows: `%USERPROFILE%\.codex\skills\`
   - macOS / Linux: `~/.codex/skills/`
3. 重启 Codex，使新 skill 生效。  
   Restart Codex so the new skill is discovered.

### 方式二：通过安装脚本 / Installer Script

如果你已经在使用 Codex 的 skill installer，也可以直接从 GitHub 安装。  
If you already use the Codex skill installer, you can install directly from GitHub.

按 GitHub 仓库路径安装：  
Install from the GitHub repo path:

```bash
python scripts/install-skill-from-github.py --repo Dsilbo/staged-commit-message-skill --path staged-commit-message
```

按 GitHub 页面 URL 安装：  
Install from the GitHub URL:

```bash
python scripts/install-skill-from-github.py --url https://github.com/Dsilbo/staged-commit-message-skill/tree/main/staged-commit-message
```

## 作用 / What It Does

`staged-commit-message` 只基于 `git diff --staged` 生成唯一且最终的中文 Git commit message，并使用保守的类型判定和固定输出规则。  
`staged-commit-message` generates one final Chinese Git commit message from `git diff --staged` only, with conservative type selection and fixed output rules.

## 目录说明 / Directory Note

安装器需要指向包含 `SKILL.md` 的目录，因此这里的安装路径是 `staged-commit-message`。  
The installer must point to the directory that contains `SKILL.md`, so the install path here is `staged-commit-message`.
