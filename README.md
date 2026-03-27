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

安装后请重启 Codex，使新 skill 生效。  
After installing, restart Codex so the new skill is discovered.

## 作用 / What It Does

`staged-commit-message` 只基于 `git diff --staged` 生成唯一且最终的中文 Git commit message，并使用保守的类型判定和固定输出规则。  
`staged-commit-message` generates one final Chinese Git commit message from `git diff --staged` only, with conservative type selection and fixed output rules.

## 目录说明 / Directory Note

安装器需要指向包含 `SKILL.md` 的目录，因此这里的安装路径是 `staged-commit-message`。  
The installer must point to the directory that contains `SKILL.md`, so the install path here is `staged-commit-message`.

