# Skills

Claude Code skills collection.

## Skills

| Skill | Description |
|-------|-------------|
| [aladin-openapi](./aladin-openapi/) | Aladin Open API integration guide — book search, used book seller scraping, search fallback strategies |

## Install

먼저 이 저장소를 클론합니다.

```bash
git clone https://github.com/geniuskey/skills.git
cd skills
```

아래에서 사용하는 도구에 맞는 섹션을 따라 설치하세요.

---

### Claude Code

**Linux / macOS**

```bash
# 프로젝트 레벨 설치
mkdir -p .claude/skills/aladin-openapi
cp -r aladin-openapi/SKILL.md .claude/skills/aladin-openapi/
cp -r aladin-openapi/references .claude/skills/aladin-openapi/

# 글로벌 설치 (모든 프로젝트에서 사용)
mkdir -p ~/.claude/skills/aladin-openapi
cp -r aladin-openapi/SKILL.md ~/.claude/skills/aladin-openapi/
cp -r aladin-openapi/references ~/.claude/skills/aladin-openapi/
```

**Windows (PowerShell)**

```powershell
# 프로젝트 레벨 설치
New-Item -ItemType Directory -Force -Path .claude\skills\aladin-openapi
Copy-Item aladin-openapi\SKILL.md .claude\skills\aladin-openapi\
Copy-Item -Recurse aladin-openapi\references .claude\skills\aladin-openapi\

# 글로벌 설치 (모든 프로젝트에서 사용)
New-Item -ItemType Directory -Force -Path $env:USERPROFILE\.claude\skills\aladin-openapi
Copy-Item aladin-openapi\SKILL.md $env:USERPROFILE\.claude\skills\aladin-openapi\
Copy-Item -Recurse aladin-openapi\references $env:USERPROFILE\.claude\skills\aladin-openapi\
```

| 범위 | Linux / macOS | Windows |
|------|---------------|---------|
| 프로젝트 | `.claude/skills/aladin-openapi/` | `.claude\skills\aladin-openapi\` |
| 글로벌 | `~/.claude/skills/aladin-openapi/` | `%USERPROFILE%\.claude\skills\aladin-openapi\` |

---

### Roo Code

**Linux / macOS**

```bash
# 프로젝트 레벨 설치
mkdir -p .roo/skills/aladin-openapi
cp -r aladin-openapi/SKILL.md .roo/skills/aladin-openapi/
cp -r aladin-openapi/references .roo/skills/aladin-openapi/

# 글로벌 설치 (모든 프로젝트에서 사용)
mkdir -p ~/.roo/skills/aladin-openapi
cp -r aladin-openapi/SKILL.md ~/.roo/skills/aladin-openapi/
cp -r aladin-openapi/references ~/.roo/skills/aladin-openapi/
```

**Windows (PowerShell)**

```powershell
# 프로젝트 레벨 설치
New-Item -ItemType Directory -Force -Path .roo\skills\aladin-openapi
Copy-Item aladin-openapi\SKILL.md .roo\skills\aladin-openapi\
Copy-Item -Recurse aladin-openapi\references .roo\skills\aladin-openapi\

# 글로벌 설치 (모든 프로젝트에서 사용)
New-Item -ItemType Directory -Force -Path $env:USERPROFILE\.roo\skills\aladin-openapi
Copy-Item aladin-openapi\SKILL.md $env:USERPROFILE\.roo\skills\aladin-openapi\
Copy-Item -Recurse aladin-openapi\references $env:USERPROFILE\.roo\skills\aladin-openapi\
```

| 범위 | Linux / macOS | Windows |
|------|---------------|---------|
| 프로젝트 | `.roo/skills/aladin-openapi/` | `.roo\skills\aladin-openapi\` |
| 글로벌 | `~/.roo/skills/aladin-openapi/` | `%USERPROFILE%\.roo\skills\aladin-openapi\` |

---

### OpenCode

**Linux / macOS**

```bash
# 프로젝트 레벨 설치
mkdir -p .opencode/skills/aladin-openapi
cp -r aladin-openapi/SKILL.md .opencode/skills/aladin-openapi/
cp -r aladin-openapi/references .opencode/skills/aladin-openapi/

# 글로벌 설치 (모든 프로젝트에서 사용)
mkdir -p ~/.config/opencode/skills/aladin-openapi
cp -r aladin-openapi/SKILL.md ~/.config/opencode/skills/aladin-openapi/
cp -r aladin-openapi/references ~/.config/opencode/skills/aladin-openapi/
```

**Windows (PowerShell)**

```powershell
# 프로젝트 레벨 설치
New-Item -ItemType Directory -Force -Path .opencode\skills\aladin-openapi
Copy-Item aladin-openapi\SKILL.md .opencode\skills\aladin-openapi\
Copy-Item -Recurse aladin-openapi\references .opencode\skills\aladin-openapi\

# 글로벌 설치 (모든 프로젝트에서 사용)
New-Item -ItemType Directory -Force -Path $env:USERPROFILE\.config\opencode\skills\aladin-openapi
Copy-Item aladin-openapi\SKILL.md $env:USERPROFILE\.config\opencode\skills\aladin-openapi\
Copy-Item -Recurse aladin-openapi\references $env:USERPROFILE\.config\opencode\skills\aladin-openapi\
```

> OpenCode는 `~/.claude/skills/` 경로도 호환됩니다. Claude Code용으로 설치했다면 OpenCode에서도 자동으로 인식됩니다.

| 범위 | Linux / macOS | Windows |
|------|---------------|---------|
| 프로젝트 | `.opencode/skills/aladin-openapi/` | `.opencode\skills\aladin-openapi\` |
| 글로벌 | `~/.config/opencode/skills/aladin-openapi/` | `%USERPROFILE%\.config\opencode\skills\aladin-openapi\` |

---

### 설치 확인

설치 후 각 도구에서 알라딘 API 관련 질문을 하면 스킬이 자동으로 활성화됩니다.

## License

MIT
