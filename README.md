# Claude Code: Editing Shorts

Setup notes for editing short-form videos (Instagram Reels) with Claude Code, using the
[video-edit skill](https://github.com/tenfoldmarc/video-edit-skill) by @tenfoldmarc
([guide](https://guides.tenfoldmarketing.com/video-edit)).

## What the skill does

Drop raw vertical talking-head clips in a folder, type `/video-edit`, and Claude:

- transcribes every clip and keeps your best take of each line
- cuts the dead air and adds punch-in zooms
- cuts you out of the background so big words can sit behind your head
- adds captions, motion graphics and subtle sound effects in one of three looks:
  - **Bold**: kinetic words around you, the big ones behind your head
  - **Cool girl**: warm grade, yellow serif italic, your own reels orbiting around you
  - **Cool dude**: dark editorial grade, tiny spaced capitals and big serif words
- renders a reel that stays inside Instagram's safe zone, plus a phone-ready copy

You review it and give notes ("captions too small", "cut the pause before X"); each re-render takes about 2 minutes.

## Review notes (checked before installing)

- The code is about 1,000 lines of Python. It runs ffmpeg locally on your clips, has no hidden code and sends nothing to outside servers.
  Its rules say your original clips are never modified or deleted.
- Setup downloads three Python packages (`faster-whisper`, `av`, `pillow`), six sound effects from HeyGen's GitHub, and,
  on first render, the video renderer HyperFrames (by HeyGen), pinned to version `0.8.34`.
- **Free.** No API keys or paid services are required. [Apify](https://apify.com) (free tier) is optional and pulls in your reels automatically;
  without it, you drop the reel files into the project folder yourself.
- Built and tested on a Mac; Windows support is newer. On Windows and Intel Macs, the background cutout runs on the processor alone, so it's slower.
  Each cutout uses about 1.5 GB of RAM.
- The skill's repo has no license file: fine for personal use, but don't copy its code into a public project.

## Setup on your computer

### 1. Install the required programs (one time)

| | Mac (Terminal) | Windows (PowerShell) |
|---|---|---|
| ffmpeg | `brew install ffmpeg` | `winget install --id Gyan.FFmpeg -e` |
| Node.js 22+ | `brew install node` | `winget install --id OpenJS.NodeJS.LTS -e` |
| Python 3.10+ | `brew install python` | `winget install --id Python.Python.3.12 -e` |

- Mac: needs [Homebrew](https://brew.sh) for the `brew` commands.
- Windows: after installing, **quit Claude Code completely and reopen it** so it sees the new programs.
  On a Snapdragon (ARM) laptop, add `--architecture x64` to the Python line.

You also need [Claude Code](https://docs.anthropic.com/en/docs/claude-code) (the Code tab in the Claude desktop app, or `claude` in a terminal).

### 2. Install the skill

Open Claude Code and paste:

```
Install this skill for me: https://github.com/tenfoldmarc/video-edit-skill
```

Or run this in a terminal yourself (needs [git](https://git-scm.com/downloads)):

```bash
git clone https://github.com/tenfoldmarc/video-edit-skill "$HOME/.claude/skills/video-edit"
```

### 3. First run

In Claude Code, type:

```
/video-edit
```

The first time, it asks for your name, Instagram handle, default look and folders (clips default to `~/Downloads`,
projects to `~/reel-edits`), checks your tools and sets everything up (about 2 minutes, once). If something is missing,
it prints the exact command to fix it.

### 4. Optional: Apify (pull your reels automatically)

1. Create a free account at https://apify.com and copy your API token (Settings > API & Integrations).
2. In a terminal, run (replace `YOUR_TOKEN`):
   ```bash
   claude mcp add apify -e APIFY_TOKEN=YOUR_TOKEN -- npx -y @apify/actors-mcp-server
   ```
   Claude Code stores the token in plain text in `~/.claude.json`, so keep that file private.
3. Reopen Claude Code.

## Usage

```
/video-edit I just dropped 5 clips in my downloads
```

```
/video-edit use the cool girl look when I say "the cool girl aesthetic", cool dude after that, bold everywhere else
```

Give notes on a version:

```
the captions feel too small, and cut the pause before "or you can get"
```

**Filming tips:** shoot vertical in 4K if your phone allows (punch-ins stay sharp), keep the camera still for each setup,
and repeat a line until it feels right. The skill picks the best take.

## Updating the skill

Paste into Claude Code:

```
Update the video-edit skill from https://github.com/tenfoldmarc/video-edit-skill
```

Or in a terminal: `git -C "$HOME/.claude/skills/video-edit" pull`
