---
name: sv-edits
description: SV Academy's own editing skill. Make and edit real design files by prompting in Claude Code or Codex, free, without opening an editing app: layered PSD with live type, .ai with SVG and PDF for the print shop, mockups (an app screen in a phone, a screen swapped in one call, a logo or post on a product shot), vectors from words (logo lockup, icon set, die-cut sticker with a cut line), a launch kit (an AI-made logo turned into a real vector with named parts, a logo guide, app icons; a design placed in perspective into a photo or a Blender scene as a swappable smart object; a print-ready business card), and clips you already have, trimmed, cut together, made vertical and exported as MP4. The agent installs free command-line tools once, shows every result in the chat and saves it as a new file next to the original. Trigger on "SV Edits", "make a PSD", "the printer wants an .ai", "logo in every format", "Thai text on this photo", "phone mockup", "logo lockup", "icon set", "die-cut sticker", "vectorize my logo", "put my design in a scene", "billboard mockup", "business card for the print shop", "Blender scene", "trim my video", "ทำไฟล์ PSD", "ไฟล์โลโก้สำหรับโรงพิมพ์", "ทำม็อกอัป", "ม็อกอัพ", "ม็อคอัพ", "ม็อคอัป", "สติกเกอร์ไดคัท", "สติ๊กเกอร์ไดคัท", "ไดคัต", "ทำโลโก้เป็นเวกเตอร์", "วางงานบนบิลบอร์ด", "นามบัตรส่งโรงพิมพ์", "ตัดคลิป". Not for new video from words, titles, captions or motion graphics (SV Motion), AI-generated images, or Office files.
---

# SV Edits. Real design files, by prompting.

ไฟล์งานออกแบบตัวจริง สั่งด้วยการพิมพ์

SV Academy's own working method for design files with an agent. The files are the ones designers and print shops use every day: a layered PSD, an .ai with its SVG and PDF, an MP4. You make and edit them by prompting in Claude Code or Codex, for free. You never open an editing app: the agent drives three free command-line tools on your own computer, shows you every result as a picture in the chat, and saves it as a new file next to the original.

Your files stay on your computer, and no file goes to an editing service or account. Your AI provider (Anthropic or OpenAI) sees what the agent reads and the previews it looks at, like any image in a chat. Other network use: the one-time download of the tools, the macOS notarization check, `doctor`'s reachability check of github.com, and, only after a yes, a font, desktop app or Blender download (in Codex, also the one fetch of this file).

The tools are three open source editors from storytold (Apache-2.0 or MIT): one for photos and PSD files, one for logos and vector files, one for footage. The agent installs only their command-line builds, pinned below, and runs them headless as `photocraft-cli`, `vectorcraft-cli` and `filmcraft-cli`. Their own `--help` and `commands` lists are the authority on flags.

For scenes, SV Edits also drives **Blender** (free, open source) as its scene maker, always in the background: see "SV Blender". It is optional, and asked for only when a scene is wanted.

This is one self-contained file. Everything the skill needs is in here, including the two helper scripts and the SV Blender script at the end, under "Helper scripts". Pre-flight writes the helper for this computer first.

**How to read this file.** It is long, and most of it is code: the two helper scripts and the SV Blender script. Every rule comes before the heading "Helper scripts": read all of that part before the first step, in pieces if your tool cuts long output (for example `sed -n '1,400p'`, then the next 400 lines, up to "Helper scripts"). The helper blocks after it are code: Pre-flight step 2 extracts them and holds their sha256 pins, so there is no need to read past "Helper scripts". The SV Blender script is the last block; "SV Blender" extracts it and holds its pin.

## Prompt only: the person never needs to open an app

This is the core of SV Edits. Hold to it on every step.

- The person installs SV Edits once, with one prompt, and from then on only types what they want. They never need to open an editing app, a timeline, a layers panel or an installer.
- The skill installs command-line tools and registers their MCP servers. The helper never keeps, installs or launches an app: no DMG, no MSI, no `.app`, no `.exe` other than the `*-cli.exe` tools. On Windows the release zip also holds the desktop app; the helper deletes it with the download, unrun. The one exception is "Want to see it in the app?": a person who asks to see a file in the free desktop app gets it installed by those separate steps, and only then. Blender is the other app SV Edits can use: it is installed only after its own question ("SV Blender") and only ever run in the background with `-b`.
- Never start anything that opens a window: no `open`, `start`, `Invoke-Item`, `xdg-open`, no `--bridge`, `--connect` or `--control`, no Blender without `-b`, and none of the GUI-only MCP tools listed under "Working inside a document". The only exception: the person asks the agent to open a file in an app they installed (see "Want to see it in the app?").
- **Every result comes back to the chat as a picture.** The agent makes a preview with `look` (or `frames` for a clip) and opens it itself: Claude Code with the Read tool, Codex with its image viewer. Never ask the person to open a file to check it.
- **Every result is a new file next to the original.** The original is fingerprinted first and checked after. See "Where files go".
- Anything that needs hands in an app is out of scope: tracking a moving subject to reframe, editing on a visual timeline, and hand-off files for a human editor (no FCPXML, EDL or OTIO). If a job cannot be finished by prompting alone, say so in one sentence and stop. Never suggest the person open an editor to finish it: the optional app is for looking and small changes by hand, never a step a job depends on.
- **Tool advice that points to an app is not passed on.** Some warnings are written for the desktop apps, for example the vector tool's "install them, or replace them with Find Font" (Find Font is a menu in the app). Report the warning word for word, then give the prompt-only fix instead: an installed face named in the prompt, `--outline-text`, or installing the font file after a yes (see "Fonts this computer does not have").

**Status tags.** Every step carries one:

- **tested**: ran on macOS on the pinned builds, on real files, on 8, 9 or 10 Oct 2026. The photo tool moved from 0.3.0 to 0.5.0 on 10 Oct (0.5.0 keeps a four-corner Distort on a smart object, which L2 needs). Steps marked tested on 8 or 9 Oct ran on photo 0.3.0; on 10 Oct a fresh home folder ran install, doctor, wire, keep, check, name, look, 1a, 2f's canvas change, L1, L2, L3 and SV Blender on 0.5.0, and 1a gave the same line bounds as on 0.3.0.
- **documented**: in the tool's own `--help` or command list, not run.
- **untested**: not run. Before an untested step, say so to the person in one sentence. Every Windows line is untested until a Windows laptop runs it: on Windows, say once per job that the Windows route is untested, and name the untested outputs at hand-over.

## When to use

Load SV Edits when the person wants to make or change a PSD, AI, SVG, PDF, EPS, photo or existing clip by prompting, without opening an editor. Typical requests: "make a PSD", "layered PSD from my brand", "edit this PSD", "change the words in my PSD", "what layers are in this PSD", "export every layer", "PSD to PNG", "make an .ai", "save my logo as .ai", "the printer wants an .ai", "logo in every format", "logo as PDF for the printer", "logo to SVG", "white version of my logo", "one colour logo", "brand board", "every size from one design", "Instagram sizes", "Story size of this PSD", "put Thai text on this photo", "export for LINE", "export for print", "trim my video", "cut these parts together", "vertical version for Reels", "9:16 from my footage", "grab a frame from this video", "cover from my video", "edit without Photoshop", "no Adobe", "photocraft", "vectorcraft", "filmcraft", "ทำไฟล์ PSD", "แก้ไฟล์ PSD", "แก้ข้อความใน PSD", "แยกเลเยอร์", "ทำไฟล์ .ai", "ไฟล์โลโก้สำหรับโรงพิมพ์", "โลโก้สีขาว", "ทำรูปทุกขนาด", "ทำขนาดสตอรี่", "ใส่ข้อความภาษาไทยบนรูป", "ตัดคลิป", "ต่อคลิป", "ทำคลิปแนวตั้ง", "แคปภาพจากวิดีโอ", "ทำปกจากวิดีโอ", "phone mockup", "app screen in a mockup", "swap the screen in my mockup", "logo on my product photo", "post on a product shot", "logo lockup", "icon set", "icons for my features", "die-cut sticker", "sticker with a cut line", "ทำม็อกอัป", "ม็อกอัพ", "ม็อคอัพ", "ม็อคอัป", "ใส่หน้าจอแอปในมือถือ", "เปลี่ยนหน้าจอในม็อกอัป", "วางโลโก้บนรูปสินค้า", "ทำไอคอน", "สติกเกอร์ไดคัท", "สติ๊กเกอร์ไดคัท", "สติ๊กเกอร์ไดคัต", "ไดคัต", "open it in PhotoCraft", "อยากเปิดดูในโปรแกรม", "vectorize my logo", "trace my AI logo", "my logo is only a PNG", "logo parts", "logo guide", "app icon from my logo", "favicon", "put my design into a scene", "put my design on this photo in perspective", "billboard mockup", "shopfront sign mockup", "LED screen mockup", "tote mockup", "product shot with my logo", "Blender scene", "use my .blend", "business card", "print-ready business card", "card with bleed and crop marks", "ทำโลโก้เป็นเวกเตอร์", "แปลงโลโก้ AI เป็นเวกเตอร์", "ทำไอคอนแอป", "วางงานในฉาก", "วางงานบนบิลบอร์ด", "ม็อกอัปป้าย", "ทำฉากใน Blender", "ทำนามบัตร", "นามบัตรส่งโรงพิมพ์".

Not for: new video from words, titles, captions, subtitles or motion graphics on video (SV Motion); AI-generated images; Office files.

## Three ways in

Pick the way from what the person has. Ask only if you cannot tell.

| You have | Way | What you do |
| --- | --- | --- |
| The Claude Code or Codex session where you built your product (**Easiest**) | 1. From your product | Read the product's code without changing it: logo, colours, fonts, name and promise, each with `file:line`. Say the brand back. Then make a layered PSD post where every line is live type, and a brand .ai with SVG and PDF. |
| A PSD, AI, SVG, PDF or photo | 2. From a file you have | Open it and report what is inside (size, layers, live text, fonts, warnings). Then edit it by the person's words: change words, colours or sizes, export for print or for LINE. Save new files next to it; never touch the original. |
| Phone or camera footage, or a reel made earlier | 3. From a clip you already have | Probe it, show frames and a cut list in seconds, and wait for a yes. Then trim, cut together, reorder, make a vertical version, grab frames (they become PSD covers) and export MP4. Titles and captions go to SV Motion. |

All three end in the same place: new files next to the source, a preview of each one shown in the chat, the original proven unchanged by `check`, and a short list of what was done, what the tools warned and what is untested. From then on the three are the same: you look, you fix it in words, and the original never changes. (จากนั้นทั้งสามทางเหมือนกัน: ดูก่อน แก้ด้วยคำพูด และไฟล์ต้นฉบับไม่เปลี่ยน)

**Design jobs on top of ways 1 and 2.** Mockups (an app screen in a phone, a screen swapped in one call, a logo or post on a product shot) and vectors from words (a logo lockup, an icon set, a die-cut sticker with a cut line) start from the product or from a file: see "Mockups and vectors from words". The launch kit (an AI-made logo made into a real vector, a design placed into a scene in perspective, a business card for the print shop) is under "Launch kit", and scenes built in Blender under "SV Blender".

**The file wins.** If the person's description and the file disagree, follow the file, say so and ask. For example: a "vector logo" whose `info` shows one embedded image and no paths; an .ai that opens as a placeholder page; a "4K clip" that probes at 1920x1080; a logo whose words are live text in a font this computer does not have.

## SV Motion or SV Edits

They are two skills that hand work to each other, never two ways to do the same job.

| | SV Motion | SV Edits |
| --- | --- | --- |
| Job | Makes **new** video from words | Edits **what you already have** |
| Input | A live site, a product's code, a brief, or a clip to put words on | A PSD, AI, SVG, PDF, photo, or a clip that already exists |
| Output | Launch reels, motion graphics, Thai titles and captions, every video size from one file, one video per name on a list | Layered PSD, .ai with SVG and PDF, trimmed and cut MP4, vertical crops, frame grabs, loudness, H.264 or ProRes |
| Words on video | Yes, always here | Never. The footage tool draws Thai as empty boxes (tested 8 Oct) |

Route by these rules:

- **Any title, caption, subtitle or animated text on video goes to SV Motion.** SV Edits never burns words into a clip.
- A reel that SV Motion made already has words on it. For another size of it, ask SV Motion to render that size from its own file. Cropping it here cuts the words off (tested 9 Oct: a 16:9 reel cropped to 9:16 lost both edges of every title).
- Where they meet: a frame grabbed here becomes a PSD cover with live Thai type (way 3, preset 3e); a trimmed clip from here is dropped into an SV Motion composition, which adds the titles; the logo PNG and colours from way 1 give SV Motion the same brand.
- **The handoff.** SV Motion is SV Academy's video skill (`sv-motion`, <https://github.com/sva-admin/sv-motion>). If it is loaded, trim the clip here first when asked, then give SV Motion the clip's path and the words. If it is not installed, say in one sentence that words on video are SV Motion's job, and give its install line: `Install the skill from https://github.com/sva-admin/sv-motion, then get this computer ready for it.` Neither skill transcribes: the person types the words, one line per sentence, in the order they are spoken.

## Pin the versions: photo 0.5.0, vector 0.5.0, film 0.2.1

Run every tool through the helper, which runs only the pinned build. Never latest, never a redirect URL, never a copy found on PATH. A class on one Wi-Fi must get identical behaviour, and these tools ship almost daily (the vector tool had three releases on 8 Oct alone). Upgrade on purpose: change the pin table in both helpers and the stamp on their first line, run every preset again, then publish.

URL form: `https://github.com/storytold/<app>/releases/download/v<version>/<asset>`, checked against the `SHA256SUMS.txt` in the same release. Sizes are bytes; digests are GitHub's asset digests, re-read on 9 Oct 2026 (photo 0.5.0: read on 10 Oct 2026 with `gh api` and checked against its `SHA256SUMS.txt`).

| Tool | Helper verb | macOS (universal CLI zip) | Windows x64 | Windows arm64 | Windows x86 |
| --- | --- | --- | --- | --- | --- |
| Photos and PSD, 0.5.0 | `photo` | `photocraft-cli-0.5.0-macos-universal.zip`, 57,316,934, `8af20fa1254a17f75cd99ccf1d25b6c9ad5cfb51c6a47ff7463c228ef25ad01c` | `photocraft-0.5.0-windows-x64-portable.zip`, 67,717,284, `da0402c19aa1b65f3cde310461cc6e41d391791e6097659a34006e526fb9d9cd` | `photocraft-0.5.0-windows-arm64-portable.zip`, 65,047,499, `eafcc98083b427fecf50c968e7083f6252878e04518b615355c810d66319ab72` | `photocraft-0.5.0-windows-x86-portable.zip`, 65,425,226, `3b5b328bef203a7e0f8eb49ae7c34aecabb29c36eb62b64b4b44dacdbe025eab` |
| Logos and vectors, 0.5.0 | `vector` | `vectorcraft-cli-0.5.0-macos-universal.zip`, 64,607,865, `64307411bc17988827dd5ce417798b3b5c20450a1ce5a4c7738202b24030b3d0` | `vectorcraft-0.5.0-windows-x64-portable.zip`, 77,469,769, `98929a4a49383a16cc2e12da92281efc0bd2c7dc44112adf11859689b2c2e4c1` | `vectorcraft-0.5.0-windows-arm64-portable.zip`, 73,123,015, `2ba33f1c3e6a0bca2ac06386bbfb5472d82004522d2e6aceb93a71c9aab28e71` | `vectorcraft-0.5.0-windows-x86-portable.zip`, 74,188,285, `72df3fc76ac2a2db8d5cfee0d67664126e4b59699b668bae69ced845352efb65` |
| Footage, 0.2.1 (way 3 only) | `film` | `filmcraft-cli-0.2.1-macos-universal.zip`, 24,675,100, `18535c3c82e4de9156251d73cf4d8f3626638ee3979063dcf4c966e811b666f6` | `filmcraft-0.2.1-windows-x64-portable.zip`, 33,724,271, `5306251e23de051523895dfa791ad3e3324c9865944df3e6a4ad590521657942` | no build: x64 under emulation on Windows 11, x86 on Windows 10 (untested) | `filmcraft-0.2.1-windows-x86-portable.zip`, 32,902,572, `e401205d3b4dd124398660b33bdd890364832c90704e1fdc9a5995037f27e1d0` |

What is inside each zip (read from each repository's `.github/workflows/release.yml` and the `packaging/windows/package.ps1` it calls, at the pinned tags, on 9 Oct): the macOS zip holds the one signed `<app>-cli` binary. The Windows portable zip holds one folder, `<app>-<version>-windows-<arch>-portable\`, with `<app>.exe` (the desktop app) and `<app>-cli.exe` (the command-line tool, a console program), plus licences. The helper keeps only `<app>-cli.exe` and the licences. The desktop app is deleted with the download and never runs.

Download sizes: photo and vector together, 122 MB on a Mac, 145 MB on Windows x64, 138 MB on arm64, 140 MB on x86. The footage tool adds 25 MB (Mac) to 34 MB (Windows).

On disk after install: on a Mac, about 245 MB for photo and vector (112 MB and 131 MB unpacked) plus 62 MB for footage, and about 330 MB free at the peak of the install while a tool unpacks; on Windows x64, about 130 MB for photo and vector, because only the command-line tools are kept.

Signer: macOS Developer ID, Team ID `DJ6XS33FX8` (Learning Machines LLC), notarized. Windows: Azure Trusted Signing, a Learning Machines subject, an issuer starting `CN=Microsoft ID Verified CS`, with a timestamp.

## Pre-flight: run this first

Do these steps in order, in the person's session, the first time SV Edits is used on a computer. The person only answers one question (step 3).

**Step 1. Find out where you are.** Agent shells keep no variables between calls, so read this once and write the full helper line every time after. Go by the shell you actually have, not by the agent's name.

| Your shell | Check | Helper line (written as `SVE` below) |
| --- | --- | --- |
| bash or zsh on macOS | `uname -s` says `Darwin`; `uname -m` says `arm64` or `x86_64` (both use the universal build); `sw_vers -productVersion` is 11 or newer | `bash ~/.sv-edits/edits.sh` |
| bash on Windows (Git Bash or MSYS: Claude Code on Windows, or any agent in Git Bash) | `uname -s` says `MINGW64_NT...` or `MSYS_NT...` | `powershell -NoProfile -ExecutionPolicy Bypass -File "$USERPROFILE/.sv-edits/edits.ps1"` |
| PowerShell on Windows (Codex on Windows, or an agent set to PowerShell) | `$PSVersionTable.PSVersion` answers | `powershell -NoProfile -ExecutionPolicy Bypass -File "$HOME\.sv-edits\edits.ps1"` |
| Linux, including WSL on Windows | `uname -s` says `Linux` | None. Stop and say: SV Edits needs a Mac, or the agent running in Windows itself (PowerShell or Git Bash), not inside WSL. |

**The helper's home.** The helper keeps the tools, previews, jobs, clip projects and fingerprints in the folder it sits in: `~/.sv-edits` (or `%USERPROFILE%\.sv-edits`) in this file. The environment variable `SV_EDITS_HOME` overrides that folder, for example `SV_EDITS_HOME=/tmp/sve-test bash /tmp/sve-test/edits.sh doctor` for a sandbox test. Wherever this file says `~/.sv-edits`, read the helper's home. A test that changes `HOME` instead also changes the fonts the photo tool sees (it reads `~/Library/Fonts` of that home), which is how a fresh student Mac looks: test fonts that way on purpose, never by accident.

On Windows the helper reads the real CPU itself (`Win32_Processor`, so an emulated shell on an Arm laptop still reports Arm64) and picks the x64, arm64 or x86 zip. The `-ExecutionPolicy Bypass` applies to that one process only; never change the machine's policy. In Codex, see "Codex and other agents" for the folder to use if `~/.sv-edits` cannot be written.

**Step 2. Write the helper and check the machine.** Never retype the helper: extract its block from this file and check its sha256. This file is on disk: Claude Code shows the skill's path when it loads it. In Codex, save it first, from the release tag (never from `main`, which moves), with the line for your shell:

```bash
# bash or zsh (macOS, or Git Bash on Windows)
curl -fsSL --proto '=https' --create-dirs https://raw.githubusercontent.com/sva-admin/sv-edits/v1.1.0/SKILL.md -o ~/.sv-edits/SKILL.md
```

```powershell
# PowerShell on Windows (untested): curl.exe, because plain curl is Invoke-WebRequest in PowerShell 5.1, and $HOME, because 5.1 passes ~ to curl.exe as a folder named ~
curl.exe -fsSL --proto "=https" --create-dirs "https://raw.githubusercontent.com/sva-admin/sv-edits/v1.1.0/SKILL.md" -o "$HOME\.sv-edits\SKILL.md"
```

That URL is the same one the person pasted (see "Codex and other agents"), so the copy on disk is the file being followed. The macOS helper is the first block that starts with `# sv-edits helper v1:`, the Windows helper the second.

**Helper pins.** The sha256 of each helper block, extracted as below (line endings LF, ending with the block's last newline):

- macOS `edits.sh`: `bda608dd6501e8e334184107dda4a84bf53e95470fc3767ac2723dd0638da9b9`
- Windows `edits.ps1`: `d98c35e0a01d9fbb39af697b1139e5679a30e3760770a38e7520d3cc106578bc`

**If the helper is already there** (a second project, a new session), hash it **without running it**, and compare with the pin:

```bash
shasum -a 256 ~/.sv-edits/edits.sh                      # macOS; Git Bash: sha256sum "$USERPROFILE/.sv-edits/edits.ps1"
```

```powershell
(Get-FileHash -Algorithm SHA256 "$HOME\.sv-edits\edits.ps1").Hash.ToLower()    # PowerShell
```

When it matches, skip the extraction and go to `doctor`. When it differs, or there is no file, extract it:

```bash
# macOS
mkdir -p ~/.sv-edits
tr -d '\r' < "<path to SKILL.md>" | awk 'f && /^```$/ {exit} /^# sv-edits helper v1:/ {n++; if (n == 1) f = 1} f' > ~/.sv-edits/edits.sh
shasum -a 256 ~/.sv-edits/edits.sh
# Windows, from Git Bash: the same line with n == 2, written to "$USERPROFILE/.sv-edits/edits.ps1", then sha256sum
```

```powershell
# Windows, from PowerShell (untested). <path to SKILL.md> must be absolute, for example "$HOME\.sv-edits\SKILL.md":
# [IO.File] does not expand ~ and reads a relative path from the process folder, not this shell's folder.
New-Item -ItemType Directory -Force "$HOME\.sv-edits" | Out-Null
$s = [IO.File]::ReadAllText("<path to SKILL.md>") -replace "`r", ""
$m = [regex]::Matches($s, '(?ms)^# sv-edits helper v1:.*?\n(?=```$)')
[IO.File]::WriteAllText("$HOME\.sv-edits\edits.ps1", $m[1].Value, [Text.Encoding]::ASCII)
(Get-FileHash -Algorithm SHA256 "$HOME\.sv-edits\edits.ps1").Hash.ToLower()
```

The sha256 must equal the pin. If it differs, extract again; never run a helper whose hash differs from the pin, `doctor` included. Only once it matches, run:

```bash
SVE doctor
```

`doctor` only reads. It prints the OS, CPU, shell, disk, RAM, network, the helper stamp and sha256, each tool's version, sha256 and signature, whether `claude` or `codex` is on PATH, and the Thai fonts the photo tool sees. Its last lines are a PASS/FAIL table. `not installed` is fine before step 4. Its network line contacts github.com once, before the person has said yes to anything; nothing is sent.

**Step 3. Ask once.** In Claude Code, one question covers the download and the registration:

> SV Edits needs two free tools for PSD and .ai files (about 122 MB to download from github.com/storytold and about 245 MB on disk, checked against this file and kept in ~/.sv-edits; no app is installed). It will also add two editing servers to Claude Code, for this project folder only: the photo server can only reach this project folder; the vector server can reach any path, and SV Edits only gives it paths inside this project. OK?
>
> SV Edits ต้องใช้เครื่องมือฟรีสองตัวสำหรับไฟล์ PSD และ .ai (ดาวน์โหลดประมาณ 122 MB จาก github.com/storytold และใช้พื้นที่ราว 245 MB ตรวจกับไฟล์นี้แล้วเก็บไว้ที่ ~/.sv-edits ไม่มีการติดตั้งแอป) และจะเพิ่มเซิร์ฟเวอร์สำหรับแก้ไฟล์สองตัวใน Claude Code เฉพาะโฟลเดอร์โปรเจกต์นี้ โดยเซิร์ฟเวอร์รูปภาพเข้าถึงได้เฉพาะโฟลเดอร์โปรเจกต์นี้ ส่วนเซิร์ฟเวอร์เวกเตอร์เข้าถึงได้ทุกที่ในเครื่อง และ SV Edits จะส่งให้เฉพาะไฟล์ในโปรเจกต์นี้ ตกลงไหม

**In Codex, ask about the tools only**, and leave the servers out of the question:

> SV Edits needs two free tools for PSD and .ai files (about 122 MB to download from github.com/storytold and about 245 MB on disk, checked against this file and kept in ~/.sv-edits; no app is installed). OK?
>
> SV Edits ต้องใช้เครื่องมือฟรีสองตัวสำหรับไฟล์ PSD และ .ai (ดาวน์โหลดประมาณ 122 MB จาก github.com/storytold และใช้พื้นที่ราว 245 MB ตรวจกับไฟล์นี้แล้วเก็บไว้ที่ ~/.sv-edits ไม่มีการติดตั้งแอป) ตกลงไหม

Codex needs no servers for anything in this file: the helper does every job, and `codex exec` refuses MCP calls anyway. Registering them is optional and has its own question (step 5).

Use the real sizes from the tables above for the person's OS, and the real folder (in Codex it may be `<project>/.sv-edits`, see "Codex and other agents"). The footage tool is asked for separately, the first time way 3 is used (25 to 34 MB). For way 3 alone, install film only; add photo when they want contact sheets or a cover (3e). Without the photo tool, `frames` still makes the frames and they are opened one by one.

**Step 4. Install, on a yes.**

```bash
SVE install photo
SVE install vector
SVE install film        # only when way 3 is used
```

Only `install` downloads. The run verbs never do: a missing tool prints the install line and stops. What `install` does, per tool, in a fresh temporary folder (the checks below passed on 8 Oct through another installer; the helper's own full download on a fresh computer is untested until the cold test):

1. Downloads the pinned asset and that release's `SHA256SUMS.txt` over HTTPS only.
2. The size must equal the pin, and the SHA256 must equal both the pin in this file and the `SHA256SUMS.txt` line. The pin is an anchor outside the release, so a replaced asset that ships with a replaced checksum file still fails.
3. Unpacks (macOS `ditto`, Windows `Expand-Archive`) and finds exactly one `<app>-cli` (macOS) or `<app>-cli.exe` (Windows).
4. Signature. macOS: `codesign --verify --strict`, `TeamIdentifier=DJ6XS33FX8`, and `spctl --assess --type install` must say `accepted` and `Notarized Developer ID` (this check needs the internet once). Windows: `Get-AuthenticodeSignature` must give Status `Valid`, a Learning Machines subject, an issuer starting `CN=Microsoft ID Verified CS`, and a timestamp. The thumbprint is never pinned and the expiry date never compared: these signing certificates last about 3 days, and the timestamp keeps the signature valid.
5. `--version` must print the pinned version.
6. Copies only the command-line tool and its licences into `~/.sv-edits/<app>-<version>/` and checks the signature again on the copy that landed.
7. Writes `installed.json` and deletes the download, desktop app included.

Any failure deletes the download, prints the exact check that failed and exits 1. `--from <folder>` runs every check on a USB copy laid out as `<folder>/<app>-<version>/<asset>` plus that release's `SHA256SUMS.txt` beside it.

**Step 5. Register the servers.** In Claude Code, after the yes in step 3. In Codex, skip this step unless the person wants to work step by step inside one document over MCP (see "Working inside a document"); then ask first, in these words:

> In Codex the two editing servers are added for every Codex session on this computer, not only this project, and the photo server stays rooted at this folder until you move it. Add them?
>
> ใน Codex เซิร์ฟเวอร์แก้ไฟล์สองตัวนี้จะถูกเพิ่มให้ทุกเซสชันของ Codex ในเครื่องนี้ ไม่ใช่เฉพาะโปรเจกต์นี้ และเซิร์ฟเวอร์รูปภาพจะผูกกับโฟลเดอร์นี้จนกว่าจะย้าย เพิ่มไหม

With the person's project folder (the folder this session is working in):

```bash
SVE wire claude "<project folder>"      # in Claude Code: claude mcp add -s local, run inside the project folder
SVE wire codex "<project folder>"       # in Codex, only after its own yes: codex mcp add
```

This adds two servers and removes nothing else:

| Server | What it runs | Reach |
| --- | --- | --- |
| `sv-photo` | `photocraft-cli mcp --automation-read-root <project> --automation-write-root <project>` | Reads and writes only inside the project folder. Absolute paths and `..` are refused (tested 9 Oct). |
| `sv-vector` | `vectorcraft-cli mcp --headless` | Headless, no window. No folder limit, so every path given to it is absolute and inside the project. |

In Claude Code the servers are registered with local scope from inside the project folder, so they exist only when Claude Code works in that folder, and two projects open at once do not fight over the root. Codex has no project scope: `codex mcp add` writes `~/.codex/config.toml`, which every Codex session reads, so one project at a time is rooted there, and Codex asks to approve that write outside its sandbox (expect one approval prompt per command). That is why `wire codex` is optional and asked separately. On Windows the helper calls the agent's own program (`codex.cmd` or `codex.exe`, found with `Get-Command -CommandType Application`), never a `.ps1` shim, so the `--` before the server command reaches Codex (untested).

`wire` refuses a home, Desktop, Documents or Downloads folder (OneDrive copies included on Windows): the photo server is rooted at one project. Moving to another project is `SVE wire claude "<other folder>"` again. The servers' tools appear in the next session; everything in this file works through the helper in the current session, so carry on. `SVE wire claude "<folder>" --dry-run` prints the commands without running them. When an add fails, its FAIL row ends with the agent's own message (the first 300 characters): report it word for word. There is no footage server: clips go through the helper, which keeps project files out of the footage folder.

**Step 6. Run `SVE doctor` again** and show the person its PASS table. Then start the way they asked for.

| Missing | Fix |
| --- | --- |
| A tool is not installed | `SVE install photo`, `vector` or `film`, after the yes |
| No network (Codex's default sandbox) | approve the command, or start the agent with network on |
| macOS before 11, or Windows before 10 | use another computer |
| Windows on Arm with the footage tool | the helper uses x64 (Windows 11) or x86 (Windows 10) under emulation; untested |
| PowerShell refuses to run scripts | the `-ExecutionPolicy Bypass` in the helper line applies to that one process only |
| `claude` or `codex` not found by `wire` (for example in the Claude desktop app) | skip `wire`: the helper route covers everything in this file |
| Codex blocks `install` (`spctl` needs the system's policy service and the network) or `wire` (it writes `~/.codex`) | approve that command outside the sandbox; expect one approval prompt for each |

The helper needs nothing beyond what the OS ships. macOS: bash 3.2, `curl`, `shasum`, `ditto`, `codesign`, `spctl`, `sips`. Windows: PowerShell 5.1, `curl.exe`, `Get-FileHash`, `Expand-Archive`, `Get-AuthenticodeSignature`, System.Drawing. No Homebrew, Node, Python, ffmpeg, git, account, key, admin, sudo or password. If anything asks for a password, stop.

Nothing else on the machine changes: no `/usr/local`, no PATH or shell-profile edits, no quarantine, Gatekeeper, SmartScreen or execution-policy changes. Removal is printed by `SVE where`: two `mcp remove` lines and one folder delete. The agent runs them only when the person asks to remove SV Edits, after confirming once.

**Before a class, at home:** install photo and vector once, so a room of laptops does not pull them through one Wi-Fi. The macOS notarization check needs the internet once, so a laptop that installs offline fails closed.

**Before Windows laptops use it (cold test, untested so far):** on one Windows 10 and one Windows 11 laptop, run `install photo` (the signature check needs `Status Valid` through the timestamp; if `TimeStamperCertificate` comes back empty, report it, never skip the check), `wire claude "<folder>"` and `wire codex "<folder>"` for real, then `keep` two files and `check` both, and look in `%APPDATA%` for a `Photocraft` folder after the first run (`where` lists it if there is one). Then the core promise, which is tested on macOS only: from Codex (PowerShell, default sandbox) and from Git Bash, run one `photo run` job, one `vector run` job and one `vector convert`, and confirm that no window and no taskbar entry appears, that no sandbox approval is asked for on each run (one approval for the first install is expected), and write down every file and folder the tools create under `%APPDATA%` and `%LOCALAPPDATA%` (outside Codex's workspace, so a sandbox that blocks those writes shows up here).

## Where files go

| What | Where |
| --- | --- |
| Way 1 (from your product) | A new folder `sv-edits/` inside the product's folder. Nothing else in the project changes. If the project is a git repository, never commit; say the folder is new and the person decides. |
| Way 2 (from a file) | Next to the original, same folder, a new name: `<original name>-<what>.<ext>`, for example `menu-new-price.psd`, `menu-new-price.jpg`, `logo-white.svg`. |
| Way 3 (from a clip) | Next to the clip: `<clip name>-cut.mp4`, `<clip name>-9x16.mp4`, `<clip name>-frame-12s.png`. Several clips: next to the first clip, `<first clip>-cut.mp4` or `<first clip>-cut-9x16.mp4`, unless the person names another place. The edit project (`.fcproj`) lives in `~/.sv-edits/projects/<clip>-<id>/` (several clips: `projects/<first clip>-multi-<id>/`), never in the footage folder, because the footage tool writes a `FilmCraft Previews` folder beside every project. |
| Previews | `~/.sv-edits/previews/`. Disposable: they exist to be shown in the chat. |
| The job written down | Params files go in `~/.sv-edits/jobs/<job>/` (the helper's own folder, so in Codex it may be `<project>/.sv-edits/jobs/<job>/`), as UTF-8 JSON, one per command, so a fix is one changed number. `<job>` is a short ASCII name with the date, for example `post-20261010`. Never write job files into the product folder. |

Three helper verbs keep the original safe (all tested 9 Oct on macOS):

```bash
SVE keep "<original>"                         # fingerprint it before any edit
SVE name "<folder>/<original name>-<what>.<ext>"   # prints a free name: adds -v2, -v3 on a clash, never an original
SVE name "<folder>/<name>-<what>.psd" "<folder>/<name>-<what>.jpg"   # a pair: one line each, the same -vN on both
SVE check "<original>"                        # before handing over: must say unchanged
```

Use the path `name` prints for every output. **Name files that belong together in one call**: the PSD and its JPEG, the .ai with its SVG and PDF, a clip and its frame. `name` gives them all the same `-vN` (tested 9 Oct: with `post-new.psd` and `post-new-v2.psd` taken, it printed `post-new-v3.psd` and `post-new-v3.jpg`). Naming them one at a time can give `post-v2.psd` next to `post.jpg`. Every path passed to the helper (`--params-file`, `--out`, `--export`, `--in`) and every path inside JSON params is absolute. Names stay in plain ASCII where the person will send the file (LINE and email keep them intact).

**Paths on Windows.** Inside JSON (params files, `cut.jsonl`, `export.json`) write paths as `C:/Users/...` with forward slashes. In Git Bash, `pwd` and `~` give `/c/Users/...`, which the tools cannot open from inside JSON: convert with `cygpath -m "<path>"`. Never write `/c/...` and never single backslashes in JSON. The Windows helper's `name` prints forward-slash paths for this reason.

## The loop: plan, make, show, hand over

1. **Say the plan back first**, in plain sentences: which file goes in; what comes out, with sizes and names; what will not be touched. Way 1 waits once, after the brand say-back; way 3 waits for a yes on the cut list. Everything else carries on unless the person asked to approve it.
2. **`keep` the original, write the params files, run.**
3. **Show it.** `SVE look "<output>" ...` makes previews at most 1600 px wide and prints each file's pixel size and KB. Open every preview in the chat. Many outputs: one contact sheet. Thai: look at a close crop of the marks. A white logo: look at it on a dark ground. The commands for these three are below.
4. **`check`, then hand over**: file paths as clickable links; each file's pixel size and KB; which lines are your writing or translation; what the tools warned, word for word (with the prompt-only fix when the warning points to an app); what is untested.
5. **Small machine.** One job at a time. A footage export uses about 6 cores, and a 135 MB PSD used about 1 GB of RAM. On 8 GB, close other apps first and keep clips to short 1080p.

**Three looks with no helper verb** (tested 9 Oct; `<previews>` is the absolute path of the helper's `previews` folder; inline JSON as shown is for macOS, on Windows put each in a params file):

```bash
# A close crop of the Thai marks: the box is the line's bounds from type.create or info, plus a margin
SVE photo run "<file.psd>" --cmd image.crop --params '{"x":60,"y":540,"width":760,"height":130}' --out "<previews>/<name>-thai-crop.png"
# A white logo on a dark ground
SVE photo run --new '{"width":800,"height":400,"background":"#111111","name":"dark"}' \
  --cmd file.placeEmbedded --params '{"path":"<abs>/<name>-white.png","fit":true}' --out "<previews>/<name>-on-dark.png"
# One contact sheet of many outputs: list the previews look printed, three per 600 px row
SVE photo run --new '{"width":64,"height":64}' --cmd file.automate.contactSheetII \
  --params '{"input":["<preview 1>","<preview 2>","<preview 3>"],"units":"pixels","width":1800,"height":600,"resolution":72,"columns":3,"rows":1,"caption":true,"flatten":true}' \
  --out "<previews>/<job>-sheet.png"
```

Each writes only into the previews folder and warns `layers flattened`, which is expected for a preview.

Reference timings, measured on 9 Oct 2026 on the test Mac (M3 Max, 36 GB), warm, through the helper: a 1080x1350 layered PSD with a placed logo and three Thai and English type layers, 0.1 s; a brand board saved as .ai, SVG, PDF and PNG, 0.1 s; six frames and a contact sheet from a 19 s 1080p clip, 2.1 s; a 6 s two-range cut exported to H.264 at -14 LUFS, 4.1 s. Still to measure in the cold tests, on an 8 GB laptop: the install per tool, and a fresh agent's time from the install prompt to the first preview, per way.

## SV defaults

- **Originals never change.** `keep` before, `check` after, new names only. The helper refuses `--in-place`, and a batch never gets the same folder as input and output (the tool refuses that anyway).
- **Their brand leads.** The person's own colours, fonts, logo and words, never SV Academy's. Copy colour values literally from the code or from the vector tool's `recolor.colors`. Convert `oklch()` and `hsl()` to hex, show both, and write the hex in `brand.json` as `"hex": "#00c950", "value": "oklch(72.3% 0.219 149.579)"` with a note that it was converted. Never convert oklch by hand. A product has Node, so use this line (tested 9 Oct: `oklch(72.3% 0.219 149.579)` gives `#00c950`; lightness as a fraction, 72.3% is 0.723; a colour outside sRGB is clipped, so say so):

  ```bash
  node -e 'const [L,C,H]=process.argv.slice(1).map(Number),h=H*Math.PI/180,a=C*Math.cos(h),b=C*Math.sin(h),l=(L+.3963377774*a+.2158037573*b)**3,m=(L-.1055613458*a-.0638541728*b)**3,s=(L-.0894841775*a-1.291485548*b)**3,f=x=>{x=Math.min(1,Math.max(0,x));return Math.round(255*(x<=.0031308?12.92*x:1.055*x**(1/2.4)-.055)).toString(16).padStart(2,"0")};console.log("#"+f(4.0767416621*l-3.3077115913*m+.2309699292*s)+f(-1.2684380046*l+2.6097574011*m-.3413193965*s)+f(-.0041960863*l-.7034186147*m+1.707614701*s))' 0.723 0.219 149.579
  ```
- **Real assets only.** SV Edits edits; it does not generate images. No invented prices, offers, numbers or reviews. Words come from the person or their code, cited with `file:line`. Flag every line you wrote or translated, and ask the person to approve it before it is shared.
- **Logos.** Never redraw or "improve" one. Recolour only to the person's own palette. A trace is labelled a draft.
- **People.** Never change faces or bodies. Ask before featuring an identifiable person, and never make a child the subject. A person is identifiable when the face can be seen (even in profile or partly), or when a name, a name badge, a distinctive tattoo or the setting would let the audience say who it is. People seen only from behind, with none of those, are not identifiable: carry on, but say in the plan that the frame shows people from behind, and still ask when any of them could be a child.
- **Thai typography.**
  - One type layer per Thai line, split at a phrase boundary (Thai has no spaces between words, and paragraph wrapping of Thai is untested). Two lines may share one layer when the break is written into the text as `\n` with a `leading` (tested 9 Oct in M1 and V3). This rule is for type you create. A PSD you were given may hold two lines in one layer, joined by a newline (`info` shows the text with `\n`, for example `"ใบเสนอราคาหลักแสน \nรอสามเดือน"`). Keep that layer as it is: one `type.edit` with the new words, and the newline kept, placed at a phrase boundary of the new words (or dropped when the new words fit on one line). Never split a layer you were given into two without asking: the designer chose it.
  - No letter spacing on Thai.
  - Faces tested with Thai: photo tool, Sukhumvit Set, Thonburi, Sarabun, Prompt; vector tool, Thonburi, Sarabun, IBM Plex Sans Thai Looped. Sukhumvit Set and Thonburi ship with macOS. On Windows, Leelawadee UI and Tahoma ship with the OS (untested).
  - The product's own font is used only if `type.fonts` lists it (the command is under "Fonts this computer does not have"; `doctor` lists Thai faces only). A web font in the product's code, a `.woff2` included, is not installed on the computer: say which installed face you used instead. The type stays live, so once the font is installed a later prompt can switch it.
  - With any face not listed above, render the line `ที่ ปั้น น้ำ ญู` and look at a close crop first: tone marks must stack, shift left on tall letters, and ญ must drop its tail before ู.
- **Language order.** The product's own order (Thai first unless the brand is English first).
- **No em or en dashes** in any line placed on a file.
- **Adult, editorial, plain.** No stickers, emoji or cartoon effects unless asked.
- **Sizes.** IG feed 1080x1350; square 1080x1080; Story and Reels 1080x1920; video thumbnail 1280x720; link preview 1200x630; LINE share copy 1080 wide, JPEG quality 85.
- **Never enlarge past native pixels.** If a size comes out smaller than asked, say so.
- **No tag.** An edit belongs to its owner, so SV Edits stamps nothing on it.
- **Previews are seen by the AI provider.** So is anything the agent reads. Say so before working on a customer's file under an NDA.

## Fonts this computer does not have

A fresh computer has the system fonts and little else. A PSD or brand made on another computer often uses a free web font (Sarabun, Prompt, Outfit, Inter) that is not installed here. The tools do not stop for it: `type.edit` re-renders the edited words in a substitute with no warning, and `info` still names the original font (seen 9 Oct on a fresh home folder without Sarabun or Outfit). So check before you create or edit any type.

**Check.** The photo tool lists every family it can use (tested 9 Oct):

```bash
SVE photo run --new '{"width":64,"height":64}' --cmd type.fonts | grep -o -i -E '"(Sarabun|Outfit)[^"]*"'
```

Put the families from `info` or `brand.json` in the pattern; no output means not installed. `doctor` checks only a fixed list of Thai faces, so it never answers this for a Latin font. The vector tool's `info` marks each font of a file as `exact`, `substitute` or `missing`.

**A file that came with a `fonts/` folder.** An SV Edits kit ships the font files its live type uses, in `fonts/` next to the PSD and .ai, with `FONTS.txt` (which file is which) and `OFL.txt` (the free licence). Before the first check or edit, use that folder as the font source, without installing anything: give the tools a home folder whose `Library/Fonts` holds the person's own fonts plus the kit's, and run every photo and vector command of the job with it. On a Mac:

```bash
FH="$HOME/.sv-edits/fonthome/<kit folder name>"; mkdir -p "$FH/Library/Fonts"
ln -sf "$HOME"/Library/Fonts/* "$FH/Library/Fonts/" 2>/dev/null   # the person's own fonts stay visible
ln -sf "<kit>/fonts/"*.ttf "$FH/Library/Fonts/"
HOME="$FH" SVE photo run "<kit>/<name>.psd" --cmd type.fonts --params '{"family":"<family from info>"}'
HOME="$FH" SVE vector info "<kit>/<name>.ai"                         # every font exact
```

Tested 9 Oct with `photocraft-cli` and `vectorcraft-cli` run directly: with a home holding only a kit's `fonts/`, every Type layer re-rendered exactly as built and the vector tool found every font `exact`; with a font file removed, the lines came out in another face and the vector tool said `substitute`. Through the helper the same switch is untested (the helper finds its own folder from where it sits, not from `HOME`). The photo and vector MCP servers run with the normal home, so for a kit with `fonts/` work through the helper with that home instead. Installing the kit's fonts for the person (copying `fonts/` into `~/Library/Fonts`) is fix 2 below and needs a yes. On Windows, the switch does not apply: use fix 2 after a yes (untested there) or fix 1.

**Fix, by prompt only.** Name the missing font, then offer the two fixes and wait for the person's choice:

1. **Use an installed face, and keep the type live.** On a PSD, one `type.edit` per Type layer that uses the font: `{"layer":<id>,"runs":[{"start":0,"end":<number of characters in the text>,"font":"<installed face>"}]}` (tested 9 Oct: `info` then names the new face). For Thai, a tested Thai face (see "Thai typography"). On an .ai, SVG or PDF, pick the installed face for new text, or outline the existing words with `--outline-text` on export. Say which face replaced which.
2. **Install the font, after a yes.** A free font (OFL, like every Google Font) can be added for this user only, without admin: download its `.ttf` or `.otf` files from the font's own source (Google Fonts: `https://github.com/google/fonts`, folder `ofl/<family in lowercase, no spaces>/`) into `~/Library/Fonts`, then run the check again (untested as a step; the photo tool reads `~/Library/Fonts`, seen 9 Oct). Ask first: it changes this computer's font list for every app. A site's captured `.woff2` is a web file: use the `.ttf` or `.otf` instead. A paid or licensed font is never downloaded: ask for the file the person owns, or use fix 1. On Windows, use fix 1 (installing fonts there is untested).

After either fix, `look` and show a close crop of the changed lines.

## Way 1: from your product (Easiest)

The person is in the Claude Code or Codex session where they built their product. Tested 9 Oct on SV Academy's own brand, except the reading step, which follows SV Motion's way 2 (tested 29 Sep).

**Step 1. Pre-flight**, then `mkdir "<project>/sv-edits"`, and in Claude Code `SVE wire claude` with the project folder (in Codex, only if the person said yes to step 5's own question).

**Step 2. Read the code without changing it**, in this order: README, `package.json` and `docs/`; colours from `:root` CSS variables, the Tailwind theme or config, shadcn tokens and the manifest `theme_color`; fonts from `next/font`, `@font-face` and Google Fonts links; the logo from `public/logo.svg`, `app/icon.*`, the favicon, or an inline `<svg>` in a Logo component; the words: the name, the one-line promise and the call to action, in Thai and English. Note `file:line` for each. Skip framework defaults: `app/favicon.ico`, `public/next.svg`, `public/vercel.svg`, `public/file.svg`, `public/globe.svg` and `public/window.svg` from create-next-app are not the brand's logo; if they are all there is, the logo kind is `none`.

**Not a code repository.** Some products arrive as data instead: a brand file (a JSON with keys like `bg`, `ink`, `accent`, `fontDisplay`, `logo`, `taglineTh`) or a site capture (a folder with `tokens.json`, `page.html` and `visible-text.txt`). Read them the same way, with the source written as the file and the JSON key (`brand.json#accent`) or `file:line` for HTML and text:

- Colours and words: from the brand file first. Fill gaps from the capture's `tokens.json`, and words from `visible-text.txt`, never invented.
- Fonts: the family name, not the file. A `fontDisplay` that points to a `.woff2` is the site's web font: read its family from the file name or the `@font-face` in `page.html`, then check it is installed (see "Fonts this computer does not have").
- Logo, when both a PNG and an SVG exist: `look` both and compare them with the site. Use the SVG when it shows the same mark as the PNG and the page (the vector tool keeps it sharp at every size), after the fixes in step 4. Use the PNG when the SVG is a different or partial mark (a favicon, an icon without the wordmark), or when it still renders wrong after step 4. Say which one you chose and why, in the say-back.

**Step 3. Write `sv-edits/brand.json` and say it back**, then wait for changes. This is the one wait in way 1; after it, carry on to the files.

```json
{
  "name": "", "promise_th": "", "promise_en": "", "cta_th": "", "cta_en": "",
  "colors": [{"role": "background|ink|accent|soft", "value": "", "hex": "", "source": "file:line"}],
  "fonts": [{"role": "display|body", "family": "", "source": "file:line", "installed": true}],
  "logo": {"kind": "svg|component|png|none", "file": "", "source": "file:line"}
}
```

`installed` comes from the `type.fonts` check under "Fonts this computer does not have", run for each family (`doctor` lists only Thai faces, so a Latin brand font never shows there). `keep` the logo file.

**Step 4. The logo source.** An SVG file: `SVE vector info` it and `look` it first. If it renders right and `info` lists no `substitute` or `missing` font, use it as it is. Otherwise write a fixed copy to `sv-edits/logo-source.svg` (the original file never changes) with the same fixes as an inline component below:

- `fill="var(--logo-bg)"`, `currentColor` and the like: replace each with the hex it resolves to, from the `<style>` inside the SVG or the site's `:root` (a capture's `tokens.json` or `page.html`).
- Live text in a generic CSS family (`system-ui`, `sans-serif`, `serif`, `monospace`, `ui-monospace`): these are not fonts, so the vector tool substitutes one (seen 9 Oct: `system-ui` and `monospace` reported `missing`, drawn in Source Sans 3). Look at how the site draws the logo: name the face it really uses if it is installed, otherwise a close installed face (a sans for `system-ui`, Menlo on macOS for `monospace`), and say which. When the words must look exactly like the site and that face is not here, offer to install it (see "Fonts this computer does not have"). Once the face is right, `--outline-text` on the SVG export turns the words into paths, so the logo looks the same on every computer.

Then `look` the copy next to the original render and compare. An inline component: write its markup to `sv-edits/logo-source.svg` as plain SVG (untested), and `keep` the component file. JSX is not SVG: add `xmlns="http://www.w3.org/2000/svg"` and a `viewBox`; turn camelCase attributes into SVG ones (`strokeWidth` to `stroke-width`, `fillRule` to `fill-rule`, `clipRule` to `clip-rule`, `strokeLinecap` to `stroke-linecap`, `strokeLinejoin` to `stroke-linejoin`); drop `className`, `{...props}`, `aria-*` and event handlers; replace `currentColor`, `var()` and Tailwind colour classes (`fill-green-500`, `text-primary`) with the brand hex they resolve to; and write every `{expression}` as its literal value. Then `SVE look` it and compare with the site. A PNG only: use it as it is, say a vector file would be better, and never enlarge it. None: a wordmark from the name in the brand font, or Thonburi. For the PSD, a vector logo is first made into a PNG at 2x: `SVE vector convert "<logo.svg>" "<project>/sv-edits/logo-2x.png" --scale 2`.

### 1a. A layered PSD post, every line live type (tested 9 Oct)

1080x1350 by default; any size from "SV defaults". One background in the brand colour, the logo as its own layer, and one live type layer per line: the Thai promise, the English promise, the call to action.

Write one params file per command in `<jobs>` = `~/.sv-edits/jobs/<job>/`, for example `~/.sv-edits/jobs/post-20261010/`, written out as an absolute path (values are examples from SV Academy's own brand):

```json
new.json    {"width":1080,"height":1350,"background":"#070C09","name":"post"}
logo.json   {"path":"<abs>/sv-edits/logo-2x.png","scale":30,"fit":false,"center":[540,260]}
th.json     {"x":90,"y":620,"text":"สร้างซอฟต์แวร์ของคุณเอง","font":"Sukhumvit Set","size":64,"color":"#F1F6F2","name":"promise-th"}
en.json     {"x":90,"y":720,"text":"Build your own software.","font":"Sukhumvit Set","size":52,"color":"#22C55E","name":"promise-en"}
cta.json    {"x":90,"y":1200,"text":"สมัครที่นั่ง","font":"Sukhumvit Set","size":40,"color":"#A9BAAE","name":"cta-th"}
```

(Each line above is its own file; the name before it is the file name.) Then one run:

```bash
SVE photo run --new-file "<jobs>/new.json" \
  --cmd file.placeEmbedded --params-file "<jobs>/logo.json" \
  --cmd type.create --params-file "<jobs>/th.json" \
  --cmd type.create --params-file "<jobs>/en.json" \
  --cmd type.create --params-file "<jobs>/cta.json" \
  --out "<project>/sv-edits/post-1080x1350.psd"
SVE photo convert "<project>/sv-edits/post-1080x1350.psd" "<project>/sv-edits/post-1080x1350.jpg" --quality 85
# both names from one call: SVE name "<project>/sv-edits/post-1080x1350.psd" "<project>/sv-edits/post-1080x1350.jpg"
SVE photo info "<project>/sv-edits/post-1080x1350.psd" --compact
SVE look "<project>/sv-edits/post-1080x1350.jpg"
```

- `y` is the baseline of the line. `type.create` prints each line's bounds `[x0, y0, x1, y1]`: keep every line at least 64 px inside the canvas, and make a long Thai line smaller or split it at a phrase boundary when it is too wide.
- **Two bounds formats.** `type.create` and `type.edit` print `[x0, y0, x1, y1]` (left, top, right, bottom). `photo info` prints a layer's `bounds` as `[x, y, width, height]` (tested 9 Oct: the same line was `[93,384,644,432]` from `type.create` and `[93,384,551,48]` from `info`). From `info`, the right edge is x + width and the bottom is y + height.
- The placed logo becomes a Smart Object layer named after its file. `scale` is a percentage of the PNG; `fit` true scales it down to fit the canvas.
- `info` must list one Type layer per line with its text. Say "every line is live type: ask me to change any word, colour or size".
- The JPEG warns `layers flattened` and `lossy compression`: expected for a share copy. The PSD keeps the layers.

### 1b. A brand .ai with SVG and PDF (tested 9 Oct)

One artboard with the logo, the name, the promise in Thai and colour chips with their hex values, saved as .ai for the print shop, SVG for the web and PDF for anyone. Every `--export` path is absolute (a relative one lands in whatever folder the shell is in; absolute exports tested 9 Oct). The inline `--params` below are for macOS; on Windows write each to a params file in `<jobs>`.

```bash
SVE vector run \
  --cmd file.new --params '{"name":"brand","width":1200,"height":800,"units":"Pixels","colorMode":"rgb"}' \
  --cmd shape.rectangle --params '{"x":0,"y":0,"width":1200,"height":800}' \
  --cmd paint.setFill --params '{"color":"#070C09"}' --cmd paint.setStroke --params '{"none":true}' \
  --cmd file.place --params-file "<jobs>/place-logo.json" \
  --cmd object.transformEach --params '{"scaleH":20,"scaleV":20,"reference":0}' \
  --cmd object.transformEach --params '{"moveH":-70,"moveV":130}' \
  --cmd shape.rectangle --params '{"x":80,"y":560,"width":160,"height":160,"radius":16}' \
  --cmd paint.setFill --params '{"color":"#22C55E"}' --cmd paint.setStroke --params '{"none":true}' \
  --cmd text.create --params-file "<jobs>/name.json" \
  --cmd text.create --params-file "<jobs>/promise-th.json" \
  --cmd text.create --params-file "<jobs>/hex.json" \
  --cmd file.saveCopy --params-file "<jobs>/save-ai.json" \
  --export "<project>/sv-edits/brand-board.svg" --export "<project>/sv-edits/brand-board.pdf" --export "<project>/sv-edits/brand-board.png"
SVE vector info "<project>/sv-edits/brand-board.ai"
SVE look "<project>/sv-edits/brand-board.ai"
```

- `place-logo.json` is `{"path":"<abs>/logo.svg"}`. A PNG logo needs `"link":false` so it is embedded rather than linked to its file (untested), and it stays a picture inside the .ai: say it is not a vector logo.
- A placed file lands centred at its own size. Scale it about its top-left corner (`reference` 0), then move it. Work the numbers out from the logo's own size (`w` by `h`, from `SVE vector info` on the logo or its `viewBox`), for a target width `W` with its top-left at (80, 80):
  - `scaleH` and `scaleV` = 100 × W ÷ w.
  - Before the move, the top-left sits at ((1200 - w) ÷ 2, (800 - h) ÷ 2), and scaling about that corner keeps it there. So `moveH` = 80 - (1200 - w) ÷ 2 and `moveV` = 80 - (800 - h) ÷ 2.
  - For example, a 120x120 logo at W 180: scale 150, move -460 and -260 (tested 9 Oct). The numbers in the block above (scale 20, move -70 and 130) fit only a 900 px logo. Look at the preview and adjust.
- Text params: `{"x":80,"y":420,"text":"<name>","size":72,"font":"Thonburi","color":"#F1F6F2"}`. One chip and one hex label per brand colour.
- **A chip the colour of the ground disappears** (the background colour's own chip on that background). Give any chip that is close to the board's colour a 2 pt outline in the ink colour, straight after its rectangle and fill, in place of `{"none":true}`: `--cmd paint.setStroke --params '{"color":"<ink hex>"}' --cmd stroke.set --params '{"weight":2,"align":"inside"}'` (tested 9 Oct: a `#070C09` chip on a `#070C09` board showed as a light outlined square). Look at the preview: every chip must be visible.
- `.ai` is written only by `file.saveCopy` with an absolute path: `{"path":"<abs>/brand-board.ai","format":"ai"}`. `convert` to .ai is refused. The .ai is a PDF-compatible file the vector tool can open again; opening it in other editors is untested, so say "PDF-compatible .ai" when handing it over.
- Every shape gets a 1 pt black stroke unless `paint.setStroke` is `{"none":true}`.
- `info` lists each font as `exact` or `substitute`. Report a `substitute` word for word.

If the logo is a vector file, also make the logo kit (preset 2d) into `sv-edits/logo/`.

### 1c. Every size from one master (tested 9 Oct for a same-shape size; the other shapes untested as a set)

The master is the post's params files in `<jobs>`. For each size, run 1a again with a new `new.json` (width and height) and new positions, written to `<jobs>/<w>x<h>/`, never by stretching: 1080x1080, 1080x1920, 1200x630, 1280x720. Keep the same words, fonts and colours, so every size is on brand and every line stays live. Name each `post-<w>x<h>.psd` and `.jpg`, and show one contact sheet of all of them.

The same shape at another size (1080x1350 to 540x675) is one command, and the type stays live (tested 9 Oct): `SVE photo run "<master.psd>" --cmd image.imageSize --params '{"width":540,"resample":"lanczos"}' --out "<new.psd>"` (on Windows, the JSON in a params file). A PSD that did not come from way 1 has no params files: use preset 2f.

**Then:** list any file the person might want inside the product (for example `public/og.jpg` from the 1200x630), and copy only after a yes, never over an existing file without asking.

## Way 2: from a file you have

**Step 1. Pre-flight** (photo for PSD and photos, vector for AI, SVG, PDF and EPS). Root `sv-photo` at the folder that holds the file if the person wants to keep working in it over MCP.

**Step 2. `keep` the file, then open it and report what is inside**, in plain words:

| File | Command | Report |
| --- | --- | --- |
| PSD | `SVE photo info "<file>" --compact` | Size, colour mode, every layer with id, kind (Type, Smart Object, Pixel, Group), name, and for Type layers the words, font and size |
| AI, SVG, PDF, EPS | `SVE vector info "<file>"` | Artboards, colour mode, object counts by kind, every font as `exact`, `substitute` or `missing`, and every warning |
| JPEG, PNG, TIFF, WebP | `SVE photo info "<file>" --compact` | Size in pixels |

Then `SVE look "<file>"` and show it. Check the size first: ask before a PSD over about 500 MB. Stop and ask when:

- an .ai opens as a placeholder page (it was saved without PDF compatibility): ask for a PDF, SVG or EPS of it;
- the file is HEIC (an iPhone photo): the pinned photo tool does not open it. On a Mac, `sips -s format jpeg "<file>.heic" --out "<file>.jpg"` makes a JPEG copy next to it (sips ships with macOS). On Windows, ask for a JPEG (photos sent through LINE arrive as JPEG);
- `info` shows a logo whose words are live text in a font this computer does not have;
- an edit touches a Type layer whose font `type.fonts` does not list: `type.edit` would re-render it in a substitute without a warning, and `info` would still name the old font. Run the check under "Fonts this computer does not have" for every font `info` reports, before the first edit, name each missing one, and offer its two prompt-only fixes (switch to an installed face, or install the free font after a yes).

**Step 3. Edit by the person's words**, write the params files, run, show, `check`, hand over.

| They say | Command (tested 9 Oct unless marked) |
| --- | --- |
| "Change the words" in a PSD | `type.edit` with `{"layer":<id>,"text":"<new words>"}`: the styles stay. Ids come from `info` on the file you are editing. |
| "Make the first word green and bigger" | `type.edit` with `{"layer":<id>,"runs":[{"start":0,"end":5,"color":"#22C55E","size":80}]}`. A `color` key next to `text` is ignored: colour and size go in `runs`. |
| "Change the colours" of a logo or .ai | `select.all`, then `recolor.colors` to list them, then `recolor.apply` with `{"map":[{"from":["#0f1b2d"],"to":"#ffffff"}]}` |
| "Make it smaller", same shape | `image.imageSize` with `{"width":<px>,"resample":"lanczos"}`. Never enlarge. |
| "Make it square" or another shape, a photo (a layered PSD: 2f) | `image.crop` to the shape around the subject, then `image.imageSize`. Read the crop params with `SVE photo commands --json --filter crop`. Ask rather than cut through a face. |
| "Export for LINE" | `SVE photo convert "<in>" "<out>.jpg" --quality 85`, 1080 wide (resize first if wider) |
| "Export for print" | A logo or .ai: PDF (its page has no fonts; check as in 2d) plus the .ai. A PSD: the PSD itself plus a full-size `.tif` (`convert`), at the printer's pixel size (A4 at 300 dpi is 2480x3508). Send RGB and let the printer convert (see "When it fails" on black). |

Every PSD edit is saved as a new PSD (layers kept) and a JPEG to share; both get their names from one `SVE name` call, so they match.

### 2a. A Thai headline on a photo (each piece tested 8 and 9 Oct)

Base: the photo, sized first (2c). One `type.create` per line, Thai then English, in the brand's colour and an installed face, with a stroke so it reads on any photo: `layer.layerStyle.stroke` with `{"size":4,"position":"outside","color":"#000000","opacity":100}`, placed **in the same run, straight after that line's `type.create`**. With no `layer` key it strokes the layer just created (tested 9 Oct: two lines, each `type.create` followed by its stroke, both stroked). Do not reuse the `layer` id that `type.create` printed in a later run: ids change when the PSD is saved and opened again (seen 9 Oct: printed 6, `info` on the saved file said 4, and the stroke failed with "no such layer"). To stroke a line in a PSD that is already saved, take the id from `info` on that saved file and pass it as `"layer":<id>` (tested 9 Oct). A stroke without `opacity` 100 does not show. Save `<photo>-headline.psd` (live type) and `<photo>-headline.jpg`. Show a close crop of the Thai marks.

### 2b. Every layer of a PSD as PNG (tested 8 Oct on an 8-layer 135 MB PSD)

`info` first, then one run with one `layer.exportAs` per layer, and one flat PNG (inline `--params` as shown are for macOS; on Windows, one params file per layer):

```bash
SVE photo run "<file.psd>" --cmd layer.exportAs --params '{"layer":9,"path":"<abs>/<file>-layer-9-<name>.png"}' \
  --cmd layer.exportAs --params '{"layer":8,"path":"<abs>/<file>-layer-8-<name>.png"}'
SVE photo convert "<file.psd>" "<abs>/<file>-flat.png"
```

Put them in a new folder next to the PSD, `<file>-layers/`, with a `README.txt` listing each layer's id, name, blend mode and bounds. Show one contact sheet.

### 2c. Every size from one master photo (tested 9 Oct)

Per size: crop to the shape around the subject, then resize to the width with lanczos. Feed 1080x1350, square 1080x1080, Story 1080x1920, link preview 1200x630, LINE copy 1080 wide. A folder of photos: one action list, `[["image.imageSize",{"width":1080,"resample":"lanczos"}]]`, and `SVE photo batch --actions <list.json> --in "<folder>" --out "<folder>-1080" --format jpg --quality 85`, only for photos wider than 1080 (tested 8 Oct on a batch of 35). JPEG, not WebP: WebP comes out lossless only and bigger.

### 2d. A logo in every format a printer asks for (tested 8 and 9 Oct)

From a vector logo (SVG, PDF, EPS or a PDF-compatible .ai), into a new folder `<logo>-kit/` next to it:

```bash
SVE vector convert "<logo>" "<kit>/<name>.svg" --outline-text
SVE vector convert "<logo>" "<kit>/<name>.pdf"
SVE vector convert "<logo>" "<kit>/<name>.eps"
SVE vector convert "<logo>" "<kit>/<name>.png"
SVE vector convert "<logo>" "<kit>/<name>-2x.png" --scale 2
SVE vector run --in "<logo>" --cmd file.saveCopy --params '{"path":"<abs kit>/<name>.ai","format":"ai"}'
```

- `--outline-text` turns SVG words into paths, so the logo looks right on a computer without its font.
- **The PDF page draws its words as shapes, with no fonts.** Check it with `grep -a -c '/Font' "<kit>/<name>.pdf"` (on Windows, `Select-String -Path "<pdf>" -Pattern '/Font' -SimpleMatch`): `0` means no font objects, so the page prints the same on every computer (tested 9 Oct). `SVE vector info` on that PDF still reports `Type` objects, and that is expected: the file also carries an editable copy of the document for the vector tool, and `info` reads that copy. The `.pdf` and `.ai` from one source come out as the same bytes (the .ai is the PDF-compatible file), so the same check covers both.
- EPS flattens transparency into opaque art (the tool warns); fine for most print shops.
- **Counts.** `info` on the source, then on the kit: the `.pdf` and `.ai` have the same object counts as the source. The outlined `.svg` does not: each word becomes paths and compound paths (seen 9 Oct: 8 objects in the source, 12 in the outlined SVG). For the SVG, check instead that `info` lists no `Type` objects and no fonts, and `look` it next to the source: same shapes, same colours. PNGs have no objects, and EPS may differ after flattening; say so rather than count them.
- Write `<kit>/README.txt`, "Which file do I send?": print shop, PDF (or .ai or .eps if they ask); website, SVG; slides, Canva and LINE, the 2x PNG.
- A PNG-only logo: sizes from the PNG (never enlarged), and say a vector file is needed for print. A traced SVG (`imageTrace.makeAndExpand`) is a draft only.

### 2e. A white or one-colour logo (tested 9 Oct)

The outputs go next to the logo, with names from `SVE name`: `<logo folder>/<name>-white.svg` and `.png`. Every path is absolute (tested 9 Oct).

```bash
SVE vector run --in "<logo>" --cmd select.all --cmd recolor.colors
SVE vector run --in "<logo>" --cmd select.all --cmd recolor.apply --params-file "<jobs>/white.json" \
  --export "<logo folder>/<name>-white.svg" --export "<logo folder>/<name>-white.png"
```

When the white version is for a printer, add `--export "<logo folder>/<name>-white.pdf"` and `--cmd file.saveCopy --params-file "<jobs>/save-white-ai.json"` (`{"path":"<logo folder>/<name>-white.ai","format":"ai"}`) to the same run.

`white.json` maps every colour that `recolor.colors` listed to one colour: `{"map":[{"from":["#16a34a","#0f1b2d"],"to":"#ffffff"}]}`. A logo with a background tile turns into one white block when the tile is mapped too: leave the tile's colour out of the row, or ask. Look at the white one on a dark ground (the command is under "The loop"), and the one-colour one in the brand's primary colour. Do not convert to CMYK here (100% K comes out as `#343334`).

### 2f. Another shape of a layered PSD (tested 9 Oct: 1080x1350 to 1080x1920, type stayed live)

For "ทำขนาดสตอรี่ด้วย" on a PSD the person brought. Never enlarge and never crop through type: change the canvas, then move the lines.

1. `info` on the PSD: every layer's id, kind, bounds, and for Type layers the words and font.
2. One run, saved as a new PSD from `SVE name`:

```bash
SVE photo run "<file.psd>" \
  --cmd image.canvasSize --params-file "<jobs>/canvas.json" \
  --cmd layer.translate --params-file "<jobs>/move-4.json" \
  --out "<folder>/<file>-1080x1920.psd"
```

`canvas.json` is `{"width":1080,"height":1920,"anchor":"center","extensionColor":"#070C09"}`: the canvas grows around the old one and every layer moves with it (the result prints the `offset`). `extensionColor` fills the new area of the background: use the background's own colour. A photo background does not fill the taller frame: say so and ask whether to fill the band with a brand colour or place the photo again larger (only if it is big enough; never enlarge). `move-<id>.json` is `{"layer":<id>,"dx":0,"dy":200}`, one per line that should move, ids from step 1.

**Check every layer that touches an edge, not only the type.** From step 1 (`info` bounds are `[x, y, width, height]`), list each layer whose box reaches an edge of the old canvas: x is 0 or less, y is 0 or less, x + width is the old width or more, or y + height is the old height or more. Those are bleeds by design (a colour block off the bottom, a phone screen cut by the edge). After `canvasSize` with `anchor` center they float in the middle of the frame, so move each one back to the same edge of the new canvas with its own `layer.translate` in the same run: with `anchor` center the offset is half the growth, so for a layer that bled off the bottom `dy` = (new height - old height) ÷ 2, off the top `dy` = -(new height - old height) ÷ 2 (285 and -285 for 1350 to 1920), and the same for left and right with `dx` and the widths (seen 9 Oct: a feed post to Story needed three such moves besides the type). The background layer is filled by `extensionColor` and needs no move.
3. `info` again: the same Type layers with the same words, every line at least 64 px inside the canvas, and every layer that bled off an edge before still reaching that edge. Then `look` and show it.

A shorter or narrower shape (Story to square, feed to link preview) needs the same steps (untested) with a smaller canvas, which cuts the edges: check every Type layer's bounds first, and ask before any change that would cut a line. If the lines cannot fit, rebuild the post the way 1a does, with `type.create` from the words, fonts, sizes and colours `info` reported.

## Mockups and vectors from words

Two everyday design jobs, made from the person's own material: **mockups** (their real app screen in a phone, a new screen swapped in, their logo or post on a photo of their product) and **vectors from words** (a logo lockup, an icon set, a die-cut sticker with a cut line for the printer). They start like way 1 (from the product, brand say-back first) or way 2 (from a file the person gives), and follow the same loop: plan, `keep`, run, `look`, `check`, hand over.

| The person says | Preset | New files |
| --- | --- | --- |
| "Put my app screen in a phone mockup with my logo, name and tagline, as a layered PSD." (เอาหน้าจอแอปใส่ในม็อกอัปมือถือ พร้อมโลโก้ ชื่อ และสโลแกนของแบรนด์ ขอเป็นไฟล์ PSD แยกเลเยอร์) | M1 | `<screen>-mockup.psd` and `.png` |
| "Swap the screen in this mockup for my new screen." (เปลี่ยนหน้าจอในม็อกอัปนี้เป็นหน้าจอใหม่) | M2 | `<mockup>-new-screen.psd` and `.png` |
| "Put my logo and my post on this photo of my product." (วางโลโก้และโพสต์ของเราบนรูปสินค้านี้) | M3 | `<photo>-with-post.psd` and `.jpg` |
| "Make a logo lockup with my logo, name and tagline, as .ai, SVG, PDF and PNG." (จัดโลโก้คู่กับชื่อแบรนด์และสโลแกน เป็นไฟล์ .ai, SVG, PDF และ PNG) | V1 | `<name>-lockup.ai`, `.svg`, `.pdf`, `.png` |
| "Make 3 icons for my 3 features as SVG, in my brand colour." (ทำไอคอน 3 อันสำหรับจุดเด่น 3 ข้อของเรา เป็นไฟล์ SVG ใช้สีของแบรนด์) | V2 | `<name>-icon-1-<word>.svg` (one per icon) and `<name>-icons.ai` |
| "Turn my logo and tagline into an 80 mm round die-cut sticker with a cut line for the printer." (ทำโลโก้กับสโลแกนเป็นสติกเกอร์ไดคัททรงกลม 80 มม. พร้อมเส้นไดคัทสำหรับโรงพิมพ์) | V3 | `<name>-sticker-80mm.ai`, `.pdf`, `.svg`, `.png` |

**Status.** M1, M2 and V3 ran on 9 Oct 2026 on SV Academy's own material, and V2 over MCP. Every command block below was then run again as written, the same day, with the params files inlined the way the helper inlines them: M1 and M2 came out pixel for pixel the same as their first runs (the swap changed only the box of the screen), and V1, M3 and the command-line route of V2 ran for the first time (M3 on a stand-in photo, flat placement only). All on macOS with the pinned tools (photo 0.3.0, vector 0.5.0) called directly, with a home folder holding the kit's `fonts/` (see "Fonts this computer does not have"). Through the helper itself these blocks are untested, so each heading says "tested 9 Oct with the tools called directly; through the helper, untested". Before the first of them in a job, say once: "This design was tested with the tools called directly; running it through the SV Edits helper is untested, so I will check the result closely." (ทดสอบแล้วโดยเรียกเครื่องมือโดยตรง แต่การรันผ่านตัวช่วยของ SV Edits ยังไม่ได้ทดสอบ จะตรวจผลให้ละเอียด) Then name these outputs as untested at hand-over.

Rules for all six:

- **Real material only.** The person's own screenshot, logo, product photo and words. The phone in M1 is drawn from shapes: no mockup pack is downloaded, and no photo is generated.
- **Missing words are asked for, never filled in.** When the person gives only a screenshot or only a logo, use the screen-only (M1) or logo-only (V3) layout, or ask for the name and tagline. Words the agent writes itself are flagged and approved first, like any line it wrote.
- **Fonts first.** Run the font check before any type, and read `text.fonts` in every vector run: every family `exact`, `missingGlyphs` 0.
- **The agent's drawing is flagged.** The icons in V2 and the layout of every preset are the agent's design: say what each icon shows, and ask the person to approve it before it is shared, like a line the agent wrote.
- Values in the blocks are SV Academy's own (background `#070C09`, ink `#F1F6F2`, accent `#22C55E`, soft `#A9BAAE`, Outfit and Sarabun). Put the person's own brand in their place. Inline `--params` are for macOS; on Windows, every one goes in a params file.

### M1. An app screen in a phone mockup, built from scratch (tested 9 Oct with the tools called directly; through the helper, untested)

A 1600x1200 layered PSD, 14 layers: the brand background, a soft accent disc, a phone drawn from shapes (side buttons, a body with a drop shadow, the screen glass, the island), the screenshot as a Smart Object named `App screen` clipped to the glass, the logo, and the product name and taglines as live type.

The input is a phone screenshot at its own size (tested with 1170x2532). `keep` it, `look` at it, and ask before using one that shows names, messages, notifications or a half-loaded page.

- **No screenshot yet** (for example the person has only the product's code): ask for one taken on their phone, or of the phone-width view of their site, and wait for it. Never start a dev server, a browser or Playwright to capture one, and never draw or make up a screen.
- **The shape it needs**: portrait, about 9:19.5 (1170x2532, 1179x2556, 1080x2340 and the like). Check it with `SVE photo info`. A landscape or desktop-width screenshot does not fit the phone: it would sit as a thin strip at the top of the glass. Say so and ask for a phone screenshot; do not crop a desktop page into a phone shape.
- **Where the files go.** In way 1, in `<project>/sv-edits/`, even when the screenshot sits somewhere else (a Mac screenshot is usually on the Desktop): `<folder>` below is that folder. In way 2, `<folder>` is the screenshot's own folder. Name the pair in one call first, and use the paths it prints: `SVE name "<folder>/app-screen-1-mockup.psd" "<folder>/app-screen-1-mockup.png"`.
- **A screen only** (no logo, name or tagline given): ask for them, never fill them in. If the person wants the phone alone, leave out the Logo, Product name, Accent bar and both Tagline lines (with their `layer.setProps`), and move everything else 405.5 px left so the phone sits in the middle: the Disc `rect` x 410, the side buttons x 550.5, 550.5 and 1039.5, the body x 555.5, the glass x 573.5, the island x 744, and the screen's `center` [800, 650.17] (or the y from the formula below). `info` then lists 9 layers. Tested 9 Oct with `photocraft-cli` called directly on SV Academy's 1170x2532 screen: 9 layers, `App screen` clipped, 0.21 s.

Params files in `<jobs>` (each line is its own file, the name before it is the file name):

```json
new.json         {"width":1600,"height":1200,"background":"#070C09","name":"app-mockup"}
disc.json        {"kind":"ellipse","rect":[815.5,210,780,780],"fill":"#0E381D","name":"Disc"}
screen.json      {"path":"<abs>/app-screen-1.png","scale":38.7179,"fit":false,"center":[1205.5,650.17]}
logo.json        {"path":"<abs>/logo.png","scale":18.75,"fit":false,"center":[168,345]}
name.json        {"x":120,"y":545,"text":"SV Academy","font":"Outfit","fontStyle":"Bold","size":104,"color":"#F1F6F2","name":"Product name"}
tagline-th.json  {"x":120,"y":717,"text":"สร้างซอฟต์แวร์\nของคุณเอง ใน 3 วัน","font":"Sarabun","fontStyle":"Regular","size":60,"leading":80,"color":"#F1F6F2","name":"Tagline TH"}
tagline-en.json  {"x":120,"y":885,"text":"Build your own software. In three days.","font":"Outfit","fontStyle":"Regular","size":34,"color":"#A9BAAE","name":"Tagline EN"}
```

```bash
SVE photo run --new-file "<jobs>/new.json" \
  --cmd layer.renameLayer --params '{"name":"Background"}' \
  --cmd shape.create --params-file "<jobs>/disc.json" \
  --cmd shape.create --params '{"kind":"roundedRect","rect":[956,292,10,70],"radii":4,"fill":"#3A3D43","name":"Side button 1"}' \
  --cmd shape.create --params '{"kind":"roundedRect","rect":[956,382,10,70],"radii":4,"fill":"#3A3D43","name":"Side button 2"}' \
  --cmd shape.create --params '{"kind":"roundedRect","rect":[1445,342,10,110],"radii":4,"fill":"#3A3D43","name":"Side button 3"}' \
  --cmd shape.create --params '{"kind":"roundedRect","rect":[961,92,489,1016],"radii":76,"fill":"#121316","stroke":{"width":3,"color":"#3A3D43","opacity":100,"align":"inside"},"name":"Phone body"}' \
  --cmd layer.layerStyle.dropShadow --params '{"color":"#000000","opacity":70,"angle":90,"useGlobalLight":false,"distance":34,"size":70,"spread":0}' \
  --cmd shape.create --params '{"kind":"roundedRect","rect":[979,110,453,980],"radii":58,"fill":"#070C09","name":"Screen glass"}' \
  --cmd file.placeEmbedded --params-file "<jobs>/screen.json" \
  --cmd layer.setProps --params '{"name":"App screen","clipped":true}' \
  --cmd shape.create --params '{"kind":"roundedRect","rect":[1149.5,120,112,32],"radii":16,"fill":"#000000","name":"Island"}' \
  --cmd file.placeEmbedded --params-file "<jobs>/logo.json" \
  --cmd layer.setProps --params '{"name":"Logo"}' \
  --cmd type.create --params-file "<jobs>/name.json" \
  --cmd shape.create --params '{"kind":"rect","rect":[120,605,64,8],"fill":"#22C55E","name":"Accent bar"}' \
  --cmd type.create --params-file "<jobs>/tagline-th.json" \
  --cmd type.create --params-file "<jobs>/tagline-en.json" \
  --out "<folder>/app-screen-1-mockup.psd"
SVE photo convert "<folder>/app-screen-1-mockup.psd" "<folder>/app-screen-1-mockup.png"
SVE photo info "<folder>/app-screen-1-mockup.psd" --compact
SVE look "<folder>/app-screen-1-mockup.png"
```

- **The phone.** The glass is 453x980 at (979, 110), the body 18 px wider on every side, at (961, 92), the island centred 10 px below the top of the glass. These numbers stay the same for every brand; the phone stays dark (`#121316`, rim `#3A3D43`).
- **The screen.** `scale` = 453 ÷ screenshot width × 100; `center` = [1205.5, 160 + screenshot height × scale ÷ 200]. The top 50 px of the glass stay clear for the island. `clipped` true keeps the screenshot inside the rounded glass. A screenshot taller than the glass is cut at the bottom, like a phone mid-scroll; a shorter one leaves the glass colour below it: say so.
- **The disc** is the accent mixed into the background: about 24% accent on a dark ground (SV's is `#0E381D`), 16% on a light one. On a light ground, set the drop shadow's `opacity` to 30.
- **The logo.** `scale` = 96 ÷ logo width × 100 for a square logo (18.75 for 512 px); a wide logo at most 300 px wide and 76 px tall. Never above 100.
- **The words.** `y` is the baseline. Keep the left column between x 120 and 740 (the disc starts at 815): read each line's right edge from the `type.create` bounds and make a line smaller when it crosses 740. The Thai tagline breaks at a phrase boundary with `\n` and a `leading`.
- **Checks.** `info` lists 14 layers (9 or 8 for a screen only); `App screen` is a Smart Object with `clipped` true; `Product name`, `Tagline TH` and `Tagline EN` are Type layers with the words. Show the PNG and a close crop of the Thai tagline. Measured: 0.25 s for the PSD, 0.11 s for the PNG.

### M2. Swap the screen in one call (tested 9 Oct with the tools called directly; through the helper, untested)

```bash
SVE photo info "<folder>/<mockup>.psd" --compact          # the id of the layer named App screen
SVE photo run "<folder>/<mockup>.psd" \
  --cmd layer.select --params '{"layer":<id>}' \
  --cmd layer.smartObjects.replaceContents --params '{"layer":<id>,"path":"<abs>/app-screen-2.png"}' \
  --out "<folder>/<mockup>-new-screen.psd"
SVE photo convert "<folder>/<mockup>-new-screen.psd" "<folder>/<mockup>-new-screen.png"
```

- `layer.select` comes first, in the same run: a saved PSD does not keep its active layer, and `replaceContents` acts on the active one.
- The sv-photo server refuses `replaceContents`: this is a helper job.
- The new screen at the same pixel size as the old one was tested (1170x2532 for both). Another size is untested: look at it, and if it sits wrong, build M1 again with the new screen.
- **Proof it changed only the screen** (9 Oct): the original PSD's hash unchanged (`check`); outside the glass the two PNGs are identical (max pixel difference 0), inside the screen changed; the layer tree is the same and `App screen` is still a clipped Smart Object.
- The same two commands swap any Smart Object: the logo in M1, the post, logo or photo in M3 (tested 9 Oct). Several screens: one run per screen, each with its own name from `SVE name`.

### M3. A logo or a post on a product shot (tested 9 Oct with the tools called directly, on a stand-in photo, flat placement only; through the helper, untested)

The person's own photo of their product: a box, a shop sign, a menu board, the screen of a laptop or tablet. The post or logo lies flat on a surface that faces the camera. Perspective (a surface at an angle), curves (a mug, a bottle) and the photo's light on the print are not done: ask for a straight-on photo, and say at hand-over that it is a flat placement.

1. `SVE photo info "<photo>" --compact`: the canvas is the photo's own size, never larger. `look` at the photo and agree with the person on the box `[x, y, width, height]` of the flat surface.
2. A post goes on as a flat PNG of its PSD: `SVE photo convert "<post.psd>" "<jobs>/post.png"` (it warns `layers flattened`, expected).
3. One run:

```bash
SVE photo run --new '{"width":1600,"height":1100,"background":"#FFFFFF","name":"shot"}' \
  --cmd file.placeEmbedded --params '{"path":"<abs>/photo.jpg","fit":true}' \
  --cmd layer.setProps --params '{"name":"Photo"}' \
  --cmd shape.create --params '{"kind":"roundedRect","rect":[600,150,640,800],"radii":12,"fill":"#FFFFFF","name":"Frame"}' \
  --cmd layer.layerStyle.dropShadow --params '{"color":"#000000","opacity":45,"angle":90,"useGlobalLight":false,"distance":20,"size":50,"spread":0}' \
  --cmd file.placeEmbedded --params '{"path":"<jobs>/post.png","scale":59.2593,"fit":false,"center":[920,550]}' \
  --cmd layer.setProps --params '{"name":"Post","clipped":true}' \
  --cmd file.placeEmbedded --params '{"path":"<abs>/logo.png","scale":18.75,"fit":false,"center":[200,950]}' \
  --cmd layer.setProps --params '{"name":"Logo"}' \
  --out "<folder>/photo-with-post.psd"
SVE photo convert "<folder>/photo-with-post.psd" "<folder>/photo-with-post.jpg" --quality 85
```

- `width` and `height` in `--new` are the photo's own, from step 1, so `fit` true places it at 100%.
- **The Frame is the surface**, in the post's shape (a 1080x1350 post is 4:5, so 640x800 here). The post's `scale` = frame width ÷ post width × 100 (640 ÷ 1080 × 100 = 59.2593), its `center` the frame's centre. `clipped` true keeps the post inside the frame.
- A logo alone: leave out the Frame, shadow and Post lines. A post alone: leave out the two Logo lines. Logo `scale` = the width it should have ÷ the logo's width × 100, never above 100.
- `Photo`, `Post` and `Logo` are Smart Objects, so M2's two commands put another post or logo in (tested 9 Oct: the post swapped for another 1080x1350 post, everything else unchanged).
- `info`: the layers Background, Photo, Frame, Post (Smart Object, clipped), Logo. Show the JPEG.

### V1. A logo lockup from words (tested 9 Oct with the tools called directly; through the helper, untested)

The logo with the brand's name and tagline set beside it, as live text in the brand fonts: one file to drop on any page, slide or packaging. Horizontal here (logo left, words right).

Params files:

```json
lockup-name.json  {"x":340,"y":160,"text":"SV Academy","size":96,"font":"Outfit","style":"Bold","color":"#070C09"}
lockup-th.json    {"x":340,"y":228,"text":"สร้างซอฟต์แวร์ของคุณเอง","size":44,"font":"Sarabun","style":"Regular","color":"#A9BAAE"}
```

```bash
SVE vector run \
  --cmd file.new --params '{"name":"lockup","width":1200,"height":360,"units":"Pixels","colorMode":"rgb"}' \
  --cmd file.place --params '{"path":"<abs>/logo.png","link":false,"rect":[60,60,240,240]}' \
  --cmd object.setProps --params '{"name":"Logo"}' \
  --cmd text.create --params-file "<jobs>/lockup-name.json" --cmd object.setProps --params '{"name":"Name"}' \
  --cmd text.create --params-file "<jobs>/lockup-th.json" --cmd object.setProps --params '{"name":"Tagline TH"}' \
  --cmd text.fonts --params '{}' \
  --cmd document.export --params '{"path":"<abs>/sv-lockup.svg","format":"svg","embedFonts":true}' \
  --cmd document.export --params '{"path":"<abs>/sv-lockup.pdf","format":"pdf","preserveEditing":false}' \
  --cmd document.export --params '{"path":"<abs>/sv-lockup.png","format":"png","useArtboards":false,"scale":2,"background":"transparent"}' \
  --cmd file.saveCopy --params '{"path":"<abs>/sv-lockup.ai","format":"ai"}'
SVE look "<abs>/sv-lockup.png"
```

- The vector tool's text key is `style` (the photo tool's is `fontStyle`). `y` is the baseline. The logo's `rect` is `[x, y, width, height]`: keep the logo's own proportions.
- `text.fonts` printed Outfit Bold and Sarabun Regular `exact` with 0 missing glyphs. The PDF has no `/Font` (`grep -a -c '/Font'` gave 0): its words are shapes.
- The PNG with `useArtboards` false is cut to the art and transparent (1669x480 at `scale` 2): for slides, Canva and LINE.
- A PNG logo stays a picture inside the .ai: say it is not a vector logo. An SVG logo placed with the same `file.place` stays vector.
- No logo file: the name alone, set with `text.create` in the display font, is the wordmark.
- A stacked lockup (logo above, words centred under it) uses V3's way of centring: `text.create` with `x` at the artboard's centre, then `text.setStyle {"justify":"center"}` (untested as a lockup).
- Dark words suit a light ground. For a dark ground, run it again with the light colours and name it `-on-dark`, or make the white version (2e). For the print shop, run the logo kit (2d) on the lockup .ai: it adds the EPS and an outlined SVG.

### V2. An icon set as SVG (tested 9 Oct over MCP, and from the command line with the tools called directly; through the helper untested)

Simple line icons for the brand's features, one SVG each, plus one .ai with the set. Each icon is a plain drawing of the person's words (brackets for code, a window for an app, a calendar for days). Never copy another brand's icon.

The grid: 96 pt artboards in one row. Artboard i starts at x = i × 116 (96 pt plus the default 20 pt gap; `document.inspect` prints the artboards). Each icon stays inside 13 to 83 pt of its artboard; a 6 pt stroke with round caps and joins, no fill, one brand colour.

```bash
SVE vector run \
  --cmd file.new --params '{"name":"icons","width":96,"height":96,"units":"Points","artboards":3,"artboardLayout":{"layout":"row"}}' \
  --cmd path.create --params '{"d":"M34 30 L18 48 L34 66 M62 30 L78 48 L62 66"}' \
  --cmd paint.setFill --params '{"none":true}' --cmd paint.setStroke --params '{"color":"#22C55E"}' \
  --cmd stroke.set --params '{"weight":6,"cap":"round","join":"round"}' \
  --cmd path.create --params '{"d":"M129 21 L203 21 L203 75 L129 75 Z"}' \
  --cmd paint.setFill --params '{"none":true}' --cmd paint.setStroke --params '{"color":"#22C55E"}' \
  --cmd stroke.set --params '{"weight":6,"cap":"round","join":"round"}' \
  --cmd shape.ellipse --params '{"x":245,"y":18,"width":60,"height":60}' \
  --cmd paint.setFill --params '{"none":true}' --cmd paint.setStroke --params '{"color":"#22C55E"}' \
  --cmd stroke.set --params '{"weight":6,"cap":"round","join":"round"}' \
  --cmd select.allOnArtboard --params '{"artboard":0}' \
  --cmd document.export --params '{"path":"<abs>/sv-icon-1-code.svg","format":"svg","artboard":0,"selectedOnly":true}' \
  --cmd select.allOnArtboard --params '{"artboard":1}' \
  --cmd document.export --params '{"path":"<abs>/sv-icon-2-app.svg","format":"svg","artboard":1,"selectedOnly":true}' \
  --cmd select.allOnArtboard --params '{"artboard":2}' \
  --cmd document.export --params '{"path":"<abs>/sv-icon-3-days.svg","format":"svg","artboard":2,"selectedOnly":true}' \
  --cmd file.saveCopy --params '{"path":"<abs>/sv-icons.ai","format":"ai"}'
```

- The three shapes above only show the method: draw each icon from as many `path.create` (`d` is SVG path data in document points), `shape.rectangle`, `shape.ellipse` or `shape.line` commands as it needs, each followed by the same three paint lines.
- **`select.allOnArtboard` and `selectedOnly` true, every time.** Exporting one artboard to SVG without them keeps the other icons' paths in the file, outside its `viewBox` (seen 9 Oct). With them, each SVG holds only its own paths, in `viewBox="0 0 96 96"`, moved to the artboard's own corner (tested).
- **Over MCP** (after `wire`), the same job is: `run_command` `file.new` with the params above; `draw_path {"d":"...","fill":"none","stroke":"#22C55E","strokeWidth":6}` and `draw_shape` per part; `run_command` `stroke.set` with the round cap and join; per icon `run_command` `select.allOnArtboard` and `document.export` as above; `save_file` with an absolute `.ai` path (it wrote the .ai directly, with no warnings). Tested 9 Oct; 70 s for three icons with the agent's turns, where the command line is one run. `codex exec` refuses MCP calls: use the command line there.
- Preview: `SVE look` every SVG, or `SVE vector convert "<svg>" "<previews>/<name>.png" --scale 2` per icon and one contact sheet. Check each file: its own shapes only (`grep -c '<path'`), the brand hex, `viewBox="0 0 96 96"`.

### V3. A round die-cut sticker with a cut line (tested 9 Oct with the tools called directly; through the helper, untested)

An 80 mm round sticker for the print shop. The **cut line** is the path the shop's cutter follows: a 0.25 pt hairline with no fill, in a spot colour named `CutContour` (100% magenta), alone on its own layer `CutContour`. The art sits on layer `Art`: a white border disc 84 mm across (2 mm past the cut line, so a cut that drifts still lands on white), the badge in the brand background, an accent ring, the logo, the name and the Thai tagline as live text. The artboard is the cut line's square (the PDF's TrimBox), with 2 mm of bleed.

Params files:

```json
sticker-name.json  {"x":113.386,"y":116.281,"text":"SV Academy","size":24.125,"font":"Outfit","style":"Bold","color":"#F1F6F2"}
sticker-th.json    {"x":113.386,"y":152.728,"text":"สร้างซอฟต์แวร์\nของคุณเอง ใน 3 วัน","size":13.027,"font":"Sarabun","style":"Regular","color":"#F1F6F2"}
```

```bash
SVE vector run \
  --cmd file.new --params '{"name":"sticker-80mm","width":226.772,"height":226.772,"units":"Millimeters","colorMode":"rgb","bleed":5.669}' \
  --cmd layer.setProps --params '{"name":"Art"}' \
  --cmd swatch.new --params '{"name":"CutContour","color":{"c":0,"m":100,"y":0,"k":0},"mode":"cmyk","spot":true}' \
  --cmd shape.ellipse --params '{"x":-5.669,"y":-5.669,"width":238.11,"height":238.11}' \
  --cmd paint.setFill --params '{"color":"#FFFFFF"}' --cmd paint.setStroke --params '{"none":true}' \
  --cmd object.setProps --params '{"name":"White border"}' \
  --cmd shape.ellipse --params '{"x":7.237,"y":7.237,"width":212.297,"height":212.297}' \
  --cmd paint.setFill --params '{"color":"#070C09"}' --cmd paint.setStroke --params '{"none":true}' \
  --cmd object.setProps --params '{"name":"Badge"}' \
  --cmd shape.ellipse --params '{"x":13.992,"y":13.992,"width":198.787,"height":198.787}' \
  --cmd paint.setFill --params '{"none":true}' --cmd paint.setStroke --params '{"color":"#22C55E"}' \
  --cmd stroke.set --params '{"weight":1.206}' --cmd object.setProps --params '{"name":"Ring"}' \
  --cmd file.place --params '{"path":"<abs>/logo.png","link":false,"rect":[95.292,51.144,36.187,36.187]}' \
  --cmd object.setProps --params '{"name":"Logo"}' \
  --cmd text.create --params-file "<jobs>/sticker-name.json" \
  --cmd text.setStyle --params '{"justify":"center"}' --cmd object.setProps --params '{"name":"Product name"}' \
  --cmd shape.rectangle --params '{"x":104.701,"y":129.791,"width":17.37,"height":1.447,"radius":0.724}' \
  --cmd paint.setFill --params '{"color":"#22C55E"}' --cmd paint.setStroke --params '{"none":true}' \
  --cmd object.setProps --params '{"name":"Rule"}' \
  --cmd text.create --params-file "<jobs>/sticker-th.json" \
  --cmd text.setStyle --params '{"justify":"center","leading":17.457}' --cmd object.setProps --params '{"name":"Tagline TH"}' \
  --cmd layer.new --params '{"name":"CutContour","top":true}' \
  --cmd shape.ellipse --params '{"x":0,"y":0,"width":226.772,"height":226.772}' \
  --cmd paint.setFill --params '{"none":true}' --cmd paint.setStroke --params '{"swatch":"CutContour"}' \
  --cmd stroke.set --params '{"weight":0.25}' --cmd object.setProps --params '{"name":"Cut line"}' \
  --cmd text.fonts --params '{}' \
  --cmd document.export --params '{"path":"<abs>/sv-sticker-80mm.svg","format":"svg","embedFonts":true}' \
  --cmd document.export --params '{"path":"<abs>/sv-sticker-80mm.pdf","format":"pdf","preserveEditing":false,"bleed":{"useDocument":true}}' \
  --cmd document.export --params '{"path":"<abs>/sv-sticker-80mm.png","format":"png","useArtboards":false,"ppi":300,"background":"#E6E6E2"}' \
  --cmd file.saveCopy --params '{"path":"<abs>/sv-sticker-80mm.ai","format":"ai"}'
```

- **Every number is in points**, even with `units` Millimeters (1 mm = 2.8346 pt): 80 mm is 226.772, 2 mm of bleed 5.669, the 84 mm border 238.11. For another size, multiply every number by the new size ÷ 80, except the 0.25 pt cut line and the 2 mm bleed; the white border stays 4 mm wider than the cut.
- `x` of each centred line is the artboard's centre (113.386); `text.setStyle` `justify` center centres it there. Shorten or split a line that comes near the ring.
- **A logo only** (the person gave a logo and no words): ask for the name and tagline, never write them. If they want the logo alone, leave out the two `text.create` lines and their `text.setStyle` and `object.setProps`, the Rule rectangle and its paint and name lines, and `text.fonts`; place the logo larger, centred on the artboard: `"rect":[53.386,53.386,120,120]` for a square logo at 80 mm (120 pt, centred at 113.386; a wide logo keeps its proportions and stays inside the ring). A 512 px logo at 120 pt prints at about 307 ppi, the most it can take (see the 300 ppi rule below). Tested 9 Oct with `vectorcraft-cli` called directly: layers `Art` and `CutContour` only, `CutContour` kept in the PDF, no `/Font`, no warnings.
- **A picture logo must print at 300 ppi or more**: its width in pixels ≥ the `rect` width ÷ 72 × 300 (a 512 px logo at 36.187 pt prints at about 1,019 ppi). A vector logo (SVG) has no limit.
- The PNG is a proof only: its grey `#E6E6E2` backdrop shows the white border, and the cut line shows as a thin magenta circle. The artboard itself has no background.
- **Checks** (all passed 9 Oct): `SVE vector run --in "<abs ai>" --cmd document.inspect --params '{}'` lists the layers `Art` and `CutContour` only, the name and tagline as live text, and the `Cut line` with no fill, 0.25 pt, `CutContour`; `SVE vector run --cmd document.pdfInfo --params '{"path":"<abs pdf>"}'` gives a TrimBox of 226.772 pt square (80 mm) and a BleedBox of 84 mm; `grep -a -c '/Font'` on the PDF gives 0 (the words are shapes); `grep -a -c 'CutContour'` on the PDF is not 0 (the spot colour is kept); `text.fonts` gave both fonts `exact`, with no warnings. Measured: 0.16 s for all four files. In PowerShell (no `grep`; untested): `(Select-String -Path "<pdf>" -Pattern 'CutContour' -SimpleMatch).Count` must be more than 0, and `(Select-String -Path "<pdf>" -Pattern '/Font' -SimpleMatch).Count` must be 0.
- Hand-over: send the PDF to the print shop (the .ai if they ask), and say the art is RGB with the cut line in a CutContour spot colour. Ask the shop whether its cutter wants another name for the spot colour; change the `swatch.new` name if so. Black: see "When it fails".

## Launch kit: an AI logo made real, in a scene, at the print shop

Three jobs that take a brand from a picture to real files, each by prompting. They follow the same loop as everything else: plan, `keep`, run, `look`, `check`, hand over.

| The person says | Recipe | New files |
| --- | --- | --- |
| "Vectorize my AI logo." (ทำโลโก้ที่ AI ทำให้ เป็นไฟล์เวกเตอร์จริง) | L1 | `<logo>-vector/`: the logo as .ai, SVG, PDF and PNG, one SVG per named part, an exploded view, a logo guide, app icons and a favicon |
| "Put my design into a scene." (เอางานของเราไปวางในฉากจริง) | L2 | `<scene>-with-design.psd` and `.png`, and a swap in one call |
| "A business card for the print shop." (ทำนามบัตรส่งโรงพิมพ์) | L3 | `<name>-business-card.pdf` (PDF/X-1a, bleed, crop marks), `.ai` |

**Status.** L1, L2 and L3 ran on 10 Oct 2026 through the helper, on a fresh home folder (photo 0.5.0, vector 0.5.0, nothing else installed), on SV Academy's own raster logo and its skytrain billboard scene. The numbers below are from that run. SV Academy's finished example of all three is in the course folder, made by its own build scripts with the same commands.

**Where a scene comes from.** Any picture of a surface works for L2: a photo the person took on their phone (a blank sign, a shop window, a tote on a table), a render from SV Blender (below, which also hands over the exact corners and the scene's light), or any image they already have the right to use. Never a stock mockup downloaded for them, and never an AI-generated picture.

### L1. Vectorize my AI logo (tested 10 Oct through the helper)

A logo an AI made is a picture: soft edges, no parts, colours that drift. This turns it into a real vector logo with named parts, clean shapes and exact colours, and says plainly that the result is a redraw of their picture.

The input is the PNG or JPEG the person has (tested with a 512x512 PNG). `keep` it, `look` at it, and say the plan: trace, separate into named parts, clean, rebuild what is geometric, then the guide, icons and favicon. All job files go in `<jobs>` = `~/.sv-edits/jobs/logo-<date>/`; the outputs go in a new folder `<logo>-vector/` next to the PNG.

**Step 1. Trace.** Count the logo's real colours by eye on a close `look` (background, letters, each accent): `colors` is that count plus 2, for the soft edges. `trace.json`:

```json
{"params":{"mode":"color","colors":9,"paths":90,"corners":60,"noise":10,"method":"abutting","ignoreWhite":false,"snapCurvesToLines":false}}
```

```bash
SVE vector run --in "<abs logo.png>" --cmd select.all --cmd imageTrace.makeAndExpand --params-file "<jobs>/trace.json" \
  --export "<jobs>/1-trace.vectorcraft" --export "<vector>/<name>-trace.svg"
```

Tested on SV Academy's 7-colour tile: 9 colours gave 16 paths and 446 anchors. The `.vectorcraft` file is the vector tool's own format, used between steps; each step opens the last one with `--in`. Show the trace next to the PNG.

**Step 2. Separate and name.** Split the trace into single paths and list them:

```bash
SVE vector run --in "<jobs>/1-trace.vectorcraft" --cmd select.all --cmd object.ungroup --cmd select.all \
  --cmd object.compoundPath.release --cmd document.inspect --params '{"depth":8}' --export "<jobs>/2-released.vectorcraft"
```

Read each path's `id`, `fill` and `bounds` (`[x, y, width, height]`) from the inspect, then sort them:

- **Slivers**: a side of 3 or less, or an area under 40. Delete them.
- **The background shape**: the largest path, about the full size of the art. Every other path in the same colour is a copy of a hole (a letter's counter): delete those copies.
- **A ring or border**: two full-size paths in one other colour, one inside the other. Make them one compound path with the even-odd rule.
- **Letters**: paths in the letter colour, at least a fifth of the art's height, left to right.
- **Accents**: the rest (dots, a cursor, a leaf), top to bottom, then left to right.

Then one run deletes, joins and names every part (ids from SV Academy's run; use the ones your inspect printed):

```bash
SVE vector run --in "<jobs>/2-released.vectorcraft" \
  --cmd select.set --params '{"ids":[30,29,28,27,26,20,14,13,12,11,10,9,8,7,5]}' --cmd edit.clear \
  --cmd select.set --params '{"ids":[18,19]}' --cmd object.compoundPath.make \
  --cmd path.setFillRule --params '{"rule":"evenOdd"}' --cmd object.setProps --params '{"name":"Tile edge"}' \
  --cmd object.setProps --params '{"id":4,"name":"Tile"}' \
  --cmd object.setProps --params '{"id":15,"name":"Letter S"}' --cmd object.setProps --params '{"id":16,"name":"Letter V"}' \
  --cmd object.setProps --params '{"id":24,"name":"Prompt arrow"}' --cmd object.setProps --params '{"id":25,"name":"Prompt cursor"}' \
  --cmd object.setProps --params '{"id":22,"name":"Dot red"}' --cmd object.setProps --params '{"id":21,"name":"Dot yellow"}' \
  --cmd object.setProps --params '{"id":23,"name":"Dot green"}' --cmd layer.setProps --params '{"id":1,"name":"Logo"}' \
  --cmd document.inspect --params '{"depth":4}' --export "<jobs>/3-named.vectorcraft"
```

Names are plain words the person will recognise ("Letter S", "Dot red"), in English, one per part. Say the list back.

**Step 3. Clean.** Fewer anchors and exact colours, in one run: `object.path.simplify` on every path, then each part's fill set to its real hex. The real hex comes from the brand (the person's code or brand file, or the colours they name); without one, use the traced hex rounded to what the eye sees, and say so.

```bash
SVE vector run --in "<jobs>/3-named.vectorcraft" \
  --cmd select.set --params '{"ids":[25,24,23,22,21,16,15,4]}' --cmd object.path.simplify --params '{"tolerance":1,"cornerAngle":120}' \
  --cmd select.set --params '{"ids":[15,16]}' --cmd paint.setFill --params '{"color":"#FFFFFF"}' \
  --cmd select.set --params '{"ids":[24]}' --cmd paint.setFill --params '{"color":"#197B40"}' \
  --cmd select.none --export "<jobs>/4-clean.vectorcraft"
```

Tested: 151 anchors became 74. `cornerAngle` 120 keeps sharp corners sharp and lets curves stay smooth.

**Step 4. Rebuild what is geometric.** A traced square is never quite square and a traced dot is a lumpy polygon. Replace every part that is a plain shape with the true shape, measured from the trace:

- A rounded square: `shape.rectangle` with the bounds the inspect gave and a `radius`. Read the radius from the traced path (`document.node {"id":<id>}` lists its anchors): the straight top edge ends where the corner curve starts (SV's tile: the top edge ends at x 420 on a tile whose right side is x 511.5, so the radius is 91.5).
- A ring: two `shape.rectangle`s, the outer and inner edge (inset and radius from the two traced paths), then `object.compoundPath.make` and `path.setFillRule` evenOdd in the next run, with the two ids that run printed.
- Dots: `shape.ellipse` at one mean diameter, on one centre line, at an equal pitch (SV's three dots: 24.3 across, centres 51.54 apart).
- A bar or cursor: `shape.rectangle` at its bounds.
- Letters and drawn shapes stay as their cleaned curves. Redrawing type is a designer's job: say so.

```bash
SVE vector run --in "<jobs>/4-clean.vectorcraft" \
  --cmd select.set --params '{"ids":[4,32,25,23,22,21]}' --cmd edit.clear \
  --cmd shape.rectangle --params '{"x":0.5,"y":0.5,"width":511,"height":511,"radius":91.5}' \
  --cmd paint.setFill --params '{"color":"#0C0C14"}' --cmd paint.setStroke --params '{"none":true}' --cmd object.setProps --params '{"name":"Tile"}' \
  --cmd shape.rectangle --params '{"x":4.25,"y":4.25,"width":503.5,"height":503.5,"radius":87.25}' \
  --cmd shape.rectangle --params '{"x":13.5,"y":13.5,"width":485,"height":485,"radius":78}' \
  --cmd shape.rectangle --params '{"x":91,"y":124,"width":30,"height":5}' \
  --cmd paint.setFill --params '{"color":"#197B40"}' --cmd paint.setStroke --params '{"none":true}' --cmd object.setProps --params '{"name":"Prompt cursor"}' \
  --cmd shape.ellipse --params '{"x":64.32,"y":440.45,"width":24.3,"height":24.3}' \
  --cmd paint.setFill --params '{"color":"#FF5F57"}' --cmd paint.setStroke --params '{"none":true}' --cmd object.setProps --params '{"name":"Dot red"}' \
  --export "<jobs>/5-rebuilt.vectorcraft"
# the next run: the ring, from the two ids the run above printed (34 and 35 here)
SVE vector run --in "<jobs>/5-rebuilt.vectorcraft" \
  --cmd select.set --params '{"ids":[34,35]}' --cmd object.compoundPath.make --cmd paint.setFill --params '{"color":"#1A1A2E"}' \
  --cmd paint.setStroke --params '{"none":true}' --cmd path.setFillRule --params '{"rule":"evenOdd"}' --cmd object.setProps --params '{"name":"Tile edge"}' \
  --export "<jobs>/6-ring.vectorcraft"
```

(The block shows one dot; the others are the same three lines with their own x, colour and name.) Every new shape needs `paint.setStroke {"none":true}`, or it gets a 1 pt black outline.

**Step 5. Group, stack, save.** `document.inspect {"depth":1}` gives the ids. Group the parts in plain groups (for example Symbol, Wordmark, Accents), send the background group to the back, and save every format in one run. Names from one `SVE name` call:

```bash
SVE vector run --in "<jobs>/6-ring.vectorcraft" \
  --cmd select.set --params '{"ids":[33,40]}' --cmd object.group --cmd object.setProps --params '{"name":"Symbol"}' --cmd object.arrange.sendToBack \
  --cmd select.set --params '{"ids":[15,16]}' --cmd object.group --cmd object.setProps --params '{"name":"Wordmark"}' \
  --cmd select.set --params '{"ids":[24,36,37,38,39]}' --cmd object.group --cmd object.setProps --params '{"name":"Accents"}' --cmd object.arrange.bringToFront \
  --cmd select.none --cmd file.saveCopy --params '{"path":"<vector>/<name>-logo.ai","format":"ai"}' \
  --export "<jobs>/7-logo.vectorcraft" --export "<vector>/<name>-logo.svg" --export "<vector>/<name>-logo.pdf"
SVE vector convert "<vector>/<name>-logo.svg" "<vector>/<name>-logo.png" --scale 2
```

**A group that lands on top hides the others** (seen 10 Oct: the tile group covered the letters until it was sent to the back). Look at the PNG next to the original, in one contact sheet (the command is under "The loop"): the same shapes and colours, cleaner edges. Then the logo kit (2d) makes the EPS and outlined SVG for the print shop.

**Step 6. One SVG per part, and the exploded view.** Per part, select it alone and export it with `selectedOnly`; each file keeps the logo's full frame, so the parts line up again when stacked (Blender's logo preset imports them this way):

```bash
SVE vector run --in "<jobs>/7-logo.vectorcraft" \
  --cmd select.set --params '{"ids":[33]}' --cmd document.export --params '{"path":"<vector>/parts/tile.svg","format":"svg","selectedOnly":true}' \
  --cmd select.set --params '{"ids":[15]}' --cmd document.export --params '{"path":"<vector>/parts/letter-s.svg","format":"svg","selectedOnly":true}'
```

The exploded view puts the groups side by side on one wider artboard, each with its parts' names under it: `artboard.setProps {"index":0,"x":-60,"y":-60,"width":1832,"height":740}`, `object.move {"dx":600,"dy":0}` on the second group and `{"dx":1200,"dy":0}` on the third, a ground rectangle sent to the back in a mid-dark colour (white letters vanish on white, a near-black tile on black), and one `text.create` per label (Menlo ships with macOS). Export it as SVG and PNG.

**Step 7. The logo guide.** One A3 landscape page (`file.new` 1190.55 x 841.89 points) with the logo large (`file.place` the logo SVG with a `rect`: it stays vector), its clear space drawn as a dashed rectangle (`stroke.set {"weight":1,"dash":[4,4]}`), one chip per colour with its hex, and the logo at its minimum size. Save as .ai, PDF and PNG; `text.fonts` must say `exact` for every face. The words on the guide are the agent's: flag them.

**Step 8. App icons and favicon.** From the logo, in one run:

```bash
SVE vector run --in "<jobs>/7-logo.vectorcraft" \
  --cmd document.export --params '{"path":"<vector>/icons/icon-1024.png","scale":2,"background":"#0C0C14"}' \
  --cmd document.export --params '{"path":"<vector>/icons/icon-512.png","scale":1,"background":"#0C0C14"}' \
  --cmd document.export --params '{"path":"<vector>/icons/icon-180.png","scale":0.3515625,"background":"#0C0C14"}' \
  --cmd document.export --params '{"path":"<vector>/icons/icon-32.png","scale":0.0625}' \
  --cmd document.export --params '{"path":"<vector>/icons/icon-16.png","scale":0.03125}' \
  --cmd document.export --params '{"path":"<vector>/icons/favicon.svg","format":"svg"}'
```

`scale` is pixels per point: the target size ÷ the artboard's width. The three large sizes are opaque squares (a `background`; checked with `sips -g hasAlpha`: no), because iOS and the app stores cut their own rounded corners; 32 and 16 keep transparent corners. A logo with fine details (SV's dots and prompt) loses them at 16 px: look at the 16 and 32 at 800% and offer a simpler small version.

**Checks.** `look` every file; `info` on the .ai lists the groups and named parts; the PDF has no `/Font`; the parts folder holds one file per part, each with one shape (`grep -c '<path\|<rect\|<ellipse'`). Hand-over says: this is a vector redraw of the picture, made by the agent; letters are cleaned traces, not redrawn type; a designer should approve it before it goes to print. Measured: every step under 0.1 s.

### L2. Put my design into a scene (tested 10 Oct through the helper)

The design goes onto a surface in a picture, in perspective, as a **smart object** placed by its four corners, with the picture's own light on top, so a new design goes in with one call.

**What it needs.**

- The scene: a phone photo, a render (SV Blender writes `plate.png`), or any picture. `keep` it.
- The four corners of the surface, in pixels, clockwise from the top-left: `[[x,y],[x,y],[x,y],[x,y]]`. From SV Blender, read them from `corners.json` (`faces[].corners_px`). From a photo, `look` at it and read them off close crops of each corner (`image.crop` around the corner, as in "Three looks"), then check them with a test placement. Ask the person if a corner is hidden.
- The design as a PNG in the surface's shape: width ÷ height = the surface's real proportions (`corners.json` gives `aspect` and `suggested_design_px`; for a photo, ask the real size of the sign, poster or screen). A design in another shape is cropped or rebuilt first, never stretched. Tested with a 2400x900 design for a 14 x 5.25 m board.
- Optional, from SV Blender: `light.png` (only the surface, lit by the scene, on transparency) and the masks.

**The run.** The placed design lands centred, at 100%. `rect` is where it lands: `[cw/2 - w/2, ch/2 - h/2, cw/2 + w/2, ch/2 + h/2]` for a scene `cw` x `ch` and a design `w` x `h` (for 1920x1080 and 2400x900: `[-240, 90, 2160, 990]`). `quad` is the four corners. Params files in `<jobs>`:

```json
place.json    {"path":"<abs>/design.png","fit":false,"scale":100}
corners.json  {"rect":[-240,90,2160,990],"quad":[[652.27,225.33],[1300.28,211.41],[1304.25,457.09],[648.8,463.96]],"interpolation":"bicubic"}
levels.json   {"inBlack":0,"gamma":1,"inWhite":197}
```

**From a photo (light from the scene itself):**

```bash
SVE photo run "<abs scene.png>" \
  --cmd layer.renameLayer --params '{"name":"Scene"}' \
  --cmd file.placeEmbedded --params-file "<jobs>/place.json" --cmd layer.renameLayer --params '{"name":"Design: billboard"}' \
  --cmd edit.transform --params-file "<jobs>/corners.json" \
  --cmd select.findLayers --params '{"name":"Scene"}' --cmd layer.duplicate \
  --cmd layer.renameLayer --params '{"name":"Light: billboard (from the scene)"}' \
  --cmd image.adjustments.levels --params-file "<jobs>/levels.json" --cmd layer.arrange.bringToFront \
  --cmd layer.setProps --params '{"blend":"Multiply","clipped":true}' \
  --out "<folder>/<scene>-with-design.psd"
SVE photo convert "<folder>/<scene>-with-design.psd" "<folder>/<scene>-with-design.png"
```

The light layer is a copy of the scene, clipped to the design, in Multiply, so the surface's shading, colour of light and grain fall on the design. `inWhite` is the surface's brightest level, so the brightest part of the surface leaves the design as it is: read it with `document.pixel {"x":<x>,"y":<y>}` at four or five points inside the surface (it prints 0 to 1 per channel; take the highest red, green or blue × 255). Tested with SV's skytrain plate used as a photo: the floodlit board read 0.53 to 0.77, so `inWhite` 197, and the run took 0.27 s. A sign with its own print on it shows through: ask for a photo of the blank surface.

**From SV Blender (its own light pass and mask)**, after the `edit.transform` line, instead of the duplicate lines:

```bash
  --cmd file.placeEmbedded --params '{"path":"<abs>/light.png","fit":false,"scale":100}' \
  --cmd layer.renameLayer --params '{"name":"Light: tote-front (Blender)"}' \
  --cmd select.loadSelection --params '{"channel":"transparency"}' \
  --cmd select.findLayers --params '{"name":"Design: tote-front"}' --cmd layer.layerMask.revealSelection --cmd select.deselect \
  --cmd select.findLayers --params '{"name":"Light: tote-front"}' --cmd layer.setProps --params '{"blend":"Multiply","clipped":true}' \
```

`light.png` is white where the surface is, carrying Blender's light, and transparent everywhere else, so its transparency is also the mask: whatever stands in front of the surface (a lamp post, a strap) stays in front. One face after another in the same run, each with its own Design and Light layers. Tested 10 Oct on the tote scene (two faces in one run, 0.7 s, then a swap of the tote design: its leaf shadows and stitches stayed on the new design) and on a person's own .blend (B2).

**Checks.** `info` lists `Scene`, `Design: <face>` (a Smart Object) and `Light: <face>` (Multiply, clipped). Show the PNG and a close crop of each corner: the design's corners sit on the surface's corners.

**Swap in one call.** Another design of the same pixel size, onto the same corners (tested 10 Oct, 0.18 s):

```bash
SVE photo info "<folder>/<scene>-with-design.psd" --compact      # the id of "Design: billboard"
SVE photo run "<folder>/<scene>-with-design.psd" \
  --cmd layer.select --params '{"layer":3}' \
  --cmd layer.smartObjects.replaceContents --params '{"layer":3,"path":"<abs>/design-2.png"}' \
  --out "<folder>/<scene>-with-design-2.psd"
```

The new design must be the **same pixel size** as the one it replaces: the smart object keeps its scale, not its corners, so a bigger file would spill over the surface. `layer.select` first, in the same run (see M2). The PSD keeps the four-corner placement as a real Distort transform: open it in an editor and the smart object can still be edited and re-placed.

### L3. A business card for the print shop (tested 10 Oct through the helper)

A print-ready card: 90 x 54 mm, front and back, 3 mm bleed, crop marks, CMYK, the logo as vector and the words as outlines. Print shops ask for exactly this: a PDF/X file.

**Words come from the person**: name, what they do (the tagline), one way to reach them (site or email). No invented titles, phone numbers or addresses, and no person's details unless they give them. The layout is the agent's: flag it.

Params files (every number in points: 1 mm = 2.8346 pt; 90 mm is 255.118, 54 mm 153.071, 3 mm 8.504; the back artboard starts at x 315.118, the width plus the 60 pt gap):

```json
new.json   {"name":"business-card","width":255.118,"height":153.071,"units":"Millimeters","colorMode":"cmyk","bleed":8.504,"artboards":2,"artboardLayout":{"layout":"row","spacing":60}}
name.json  {"x":333.26,"y":56.69,"text":"SV Academy","size":13,"font":"Helvetica Neue","style":"Bold","color":{"c":60,"m":50,"y":30,"k":100}}
tag.json   {"x":333.26,"y":80,"text":"สร้างซอฟต์แวร์ของคุณเอง ใน 3 วัน","size":10,"font":"Thonburi","style":"Regular","color":{"c":60,"m":50,"y":30,"k":100}}
site.json  {"x":333.26,"y":132.28,"text":"sv-academy.org/entrepreneurs","size":8,"font":"Helvetica Neue","style":"Regular","color":{"c":85,"m":20,"y":90,"k":10}}
pdf.json   {"path":"<abs>/<name>-business-card.pdf","standard":"pdfX1a","marks":{"trim":true,"registration":false,"colorBars":false,"pageInfo":false},"bleed":{"useDocument":true},"output":{"conversion":"destination","outputIntent":"VectorCraft Generic CMYK (SWOP-like)"}}
```

```bash
SVE vector run --cmd file.new --params-file "<jobs>/new.json" \
  --cmd shape.rectangle --params '{"x":-8.504,"y":-8.504,"width":272.126,"height":170.079}' \
  --cmd paint.setFill --params '{"color":{"c":60,"m":50,"y":30,"k":100}}' --cmd paint.setStroke --params '{"none":true}' \
  --cmd object.setProps --params '{"name":"Front ground"}' \
  --cmd file.place --params '{"path":"<abs>/<name>-logo.svg","rect":[85.04,34.02,85.04,85.04]}' --cmd object.setProps --params '{"name":"Logo"}' \
  --cmd text.create --params-file "<jobs>/name.json" --cmd text.create --params-file "<jobs>/tag.json" \
  --cmd text.create --params-file "<jobs>/site.json" --cmd text.fonts --params '{}' \
  --cmd file.saveCopy --params '{"path":"<abs>/<name>-business-card.ai","format":"ai"}' \
  --cmd document.exportPdf --params-file "<jobs>/pdf.json"
```

- **Bleed**: anything that runs to the edge (the front ground) runs 3 mm past it, to -8.504. Keep words and the logo at least 4 mm (11.34 pt) inside the trim.
- **Colours are CMYK builds**, written as `{"c":..,"m":..,"y":..,"k":..}`, never left to the tool's RGB conversion: it moves greens (SV's own green turned teal in an early test). A rich black for large dark areas (here 60/50/30/100); plain 100 K for small black text. The placed logo SVG is RGB and is converted on export: look at its colours in the proof.
- **Thai**: Thonburi ships with macOS; any tested Thai face works (see "Thai typography"). The PDF draws all words as outlines.
- **Checks** (all passed 10 Oct): `SVE vector run --cmd document.pdfInfo --params '{"path":"<abs pdf>"}'` gives 2 pages, each with a TrimBox of 255.118 x 153.071 pt (90 x 54 mm) and a BleedBox 8.504 pt larger on every side; `grep -a -c '/Font'` on the PDF is 0; `grep -a -c '/GTS_PDFX'` is not 0; `text.fonts` says `exact` for every face. The proof: `SVE vector run --in "<abs pdf>" --cmd document.export --params '{"path":"<previews>/card.png","format":"png","useArtboards":true,"ppi":300}'` writes one PNG per side with the crop marks; show both in one contact sheet.
- Hand-over: send the PDF (the .ai if they ask); say it is PDF/X-1a, CMYK, 3 mm bleed, crop marks, words outlined. Ask the shop for its paper and whether it wants another bleed. A roll-up banner or a poster is the same recipe at its own size and bleed (for example 850 x 2000 mm with 5 mm), with the words much larger: untested as a recipe.

## SV Blender: scenes for your designs

Blender (free, open source, blender.org) is SV Edits' **scene maker**. It builds a real-world place around a surface (an elevated billboard at dusk, a shophouse lightbox, a mall LED screen, a tote and a takeaway box), renders it like a photograph, and writes down the exact four corners of every surface that will carry a design. PhotoCraft then puts the design there as a smart object (L2), with Blender's light on top. Blender and the Craft tools work together; the person only prompts.

- **Always headless**: `"<blender>" -b --factory-startup --python "<script>" -- <options>`. `-b` is background mode: no window, ever. Never run Blender without `-b`, and never through `open`.
- **No downloads for a scene**: everything is built from code (shapes, materials, Blender's own sky). No asset sites, no Poly Haven, no paid AI.
- **The script is in this file**: the last block, "SV Blender script", written out on demand (below). It is SV Academy's own, MIT.
- Tested on 10 Oct 2026 with **Blender 5.1.2** on macOS 26 (Apple M3 Max, Metal): that is the minimum version SV Blender asks for. Older versions are untested. Windows is untested.

**Status.** The six presets and their renders were made on 9 and 10 Oct 2026 on the test Mac. The fresh-home test on 10 Oct ran the detection step, wrote the script from this file, built one preset and one own .blend, and placed a design in each with L2.

### Find Blender, or install it

**Look first** (read only). On a Mac:

```bash
ls -d /Applications/Blender.app ~/Applications/Blender.app 2>/dev/null
mdfind 'kMDItemCFBundleIdentifier == "org.blenderfoundation.blender"' 2>/dev/null   # other places
B=/Applications/Blender.app/Contents/MacOS/Blender                                  # the path found
"$B" --version | head -1                                                            # 5.1.2 or newer
codesign -dv --verbose=2 "${B%/Contents/MacOS/Blender}" 2>&1 | grep TeamIdentifier   # 68UA947AUU (Stichting Blender Foundation)
```

`--version` prints and quits; it opens no window. A Blender that has already been used often fails `codesign --verify --strict` with "a sealed resource is missing or invalid", where every line it lists is a `__pycache__` file inside the app: Blender's own Python writes them into its bundle (seen 10 Oct on the test Mac). That is not tampering; the Team ID and version checks are the ones that count. SV Blender runs Blender with `PYTHONDONTWRITEBYTECODE=1`, so it adds none. Any other changed file: stop and report.

On Windows (untested): `Get-ChildItem "$env:ProgramFiles\Blender Foundation" -Recurse -Filter blender.exe -ErrorAction SilentlyContinue`, and `winget list --id BlenderFoundation.Blender`.

**Not there? Ask once**, naming the size: "SV Blender needs Blender, the free open source 3D app (about 335 MB to download from blender.org and about 880 MB on disk). It only runs in the background: no window opens. Install it?" (SV Blender ต้องใช้ Blender โปรแกรม 3D ฟรีแบบโอเพนซอร์ส ดาวน์โหลดประมาณ 335 MB จาก blender.org ใช้พื้นที่ราว 880 MB ทำงานเบื้องหลังเท่านั้น ไม่มีหน้าต่างเปิดขึ้นมา ติดตั้งไหม) Then, on a yes (untested as a step: the test Mac already had Blender):

- **macOS, Apple silicon**: the official DMG, pinned. `blender-5.1.2-macos-arm64.dmg`, 335,350,362 bytes, sha256 `f104ffee2ba6aee32328e5c203b7e4608d8a1745f7bbcf2766f3b9777e8fbe17`, from `https://download.blender.org/release/Blender5.1/`, checked against the pin and against `blender-5.1.2.sha256` in the same folder (read 10 Oct 2026). Download it into `~/.sv-edits/blender/`, check size and hash, then copy the app out of it without a Finder window: `hdiutil attach -nobrowse -readonly -mountpoint "<tmp>" "<dmg>"`, `ditto "<tmp>/Blender.app" ~/Applications/Blender.app`, `hdiutil detach "<tmp>"`, delete the DMG. Then `spctl --assess --type execute -vv ~/Applications/Blender.app` must say `accepted`, `Notarized Developer ID` and `68UA947AUU`. `~/Applications` needs no password; never use `sudo`.
- **macOS with Homebrew** (if `brew` is already there): `brew install --cask blender`. It installs the latest release, not the pin: check `--version` afterwards.
- **macOS on Intel**: Blender 5 has no Intel Mac build. SV Blender is untested on the last Intel release (4.5 LTS): say so, and offer L2 with a phone photo instead.
- **Windows** (untested): `winget install --id BlenderFoundation.Blender -e --version 5.1.2`, or the official installer from the same blender.org folder: `blender-5.1.2-windows-x64.msi`, 371,490,816 bytes, `7d1bb468057a3ac8fd19809e90544f6064cc215053b7732cc8a1ccc83b651ab5`; Arm: `blender-5.1.2-windows-arm64.msi`, 231,706,624 bytes, `126170c19e956102fc3e3d12713b9066eb3a33bdd4554612c0ef8b019051de77`. Any Windows prompt during the install is the person's decision. Blender is then `C:\Program Files\Blender Foundation\Blender 5.1\blender.exe`.

If a hash differs, delete the download, stop and report.

### Write the script

The script is the last block of this file, starting with the line `# sv-blender v1:`. Extract it like the helper and check its sha256:

```bash
mkdir -p ~/.sv-edits/blender
tr -d '\r' < "<path to SKILL.md>" | awk 'f && /^```$/ {exit} /^# sv-blender v1:/ {f = 1} f' > ~/.sv-edits/blender/make-scenes-blender.py
shasum -a 256 ~/.sv-edits/blender/make-scenes-blender.py
```

On Windows, from PowerShell (untested): the helper's extraction with the pattern `'(?ms)^# sv-blender v1:.*?\n(?=```$)'`, `$m[0]`, written to `$HOME\.sv-edits\blender\make-scenes-blender.py` as UTF-8 without a byte order mark (the script holds Thai, so not ASCII), then `Get-FileHash`.

It must equal the pin: `make-scenes-blender.py`: `ebe40407aca83318cc612057d82e1bc902c74137670532ddb24d32e6f2b634fd`. If it differs, extract again; never run a script whose hash differs. In the commands below, `SVB` is `~/.sv-edits/blender/make-scenes-blender.py` written out in full, and `BL` is `PYTHONDONTWRITEBYTECODE=1 "<blender path>" -b --factory-startup --python`.

### B1. Build me a billboard (shopfront, LED screen, product shot) scene in Blender and put my design in it

The presets (`BL "<SVB>" -- --list` prints them):

| Preset | The scene | Design faces | Full run on the test Mac (with the step captures) |
| --- | --- | --- | --- |
| `skytrain-billboard` | A 14 x 5.25 m billboard on a skytrain viaduct at blue hour, wet road, shophouses, signs | `billboard` | about 18 min |
| `shopfront-sign` | A shophouse at dusk: a 3.5 m lightbox and an A1 window poster | `lightbox`, `window-poster` | 11 min |
| `mall-led` | A four-level mall atrium with a 12.8 x 7.2 m LED screen | `led-screen` | 9 min |
| `tote-and-box` | Product shot at golden hour: a tote on a peg, a kraft takeaway box | `tote-front`, `box-lid`, `box-front` | 41 s (113 s on a fresh home, without captures: see below) |
| `logo-exploded-3d` | SV Academy's own logo, taken apart in 3D (its part names are SV's) | `tile-face` | 4 min, with 36 frames |
| `print-still-life` | Business cards on a proof table | `card-front`, `card-back` | 56 s |

`logo-exploded-3d` imports the part SVGs from L1 (`--parts <folder>`, SV's part names) and `print-still-life` needs the card and banner pictures (`--card`, `--banner`); the other four need nothing. The look is built in: real-world sizes, a 35 to 50 mm lens, AgX with a contrast look, blue or golden hour, warm practical lights, light haze, glare on emissive signs, and a camera finish (a touch of lens distortion, chromatic aberration, vignette and film grain) applied to the plate, the light pass, the masks and the corners alike, so they still line up.

1. Say the plan: which preset, the render time, that Blender runs in the background, and where the files go (`<project>/scenes/`).
2. Build and render, in the background. For a class or an 8 GB laptop, start with `tote-and-box`. The agent can show a quick look first with `--preview --quick` (half size, the shot only: 2 s for the tote and 3.4 min for the skytrain on the test Mac). The first Cycles render on a computer compiles its GPU kernels once: the tote took 113 s on the fresh home folder and 41 s on a warm one. Then:

```bash
BL "<SVB>" -- --scene tote-and-box --out "<project>/scenes" --no-captures
```

3. It prints `SV_DONE <scene> <seconds>` and writes `<project>/scenes/<scene>/`. Show `plate.png`. Blender also writes `~/.cache` and `~/.thumbnails` in the home folder (its own caches); nothing else outside `--out`.
4. Make one design per face at the face's `aspect` (`corners.json`, `faces[]`; for example a 2400x900 PNG for an 8:3 board), then L2 "From SV Blender" with `plate.png`, `light.png` and each face's `corners_px`. Show the result and a close crop of the surface.
5. Hand over the PSD (each design a smart object, swappable in one call), the PNG, and the scene folder.

`--quick` renders at half size with fewer samples (for a laptop or a first look); `--samples <n>` sets the quality; `--design <png>` also renders `preview-with-design.png`, the design mapped onto every face inside Blender, as a check of the PhotoCraft result. `--captures <folder>` adds step-by-step pictures of how the scene is assembled; it needs `python3` with Pillow and Chrome, so leave it out on a fresh computer.

### B2. Use my own .blend: tell me which object is the design face

The person has a scene of their own, made in Blender or given to them. The file is only read; SV Blender saves its own copy in `--out`. Python scripts stored inside a .blend do not run: `--factory-startup` keeps Blender's auto-run setting off.

1. `keep` the .blend, then list what the camera sees, largest first:

```bash
BL "<SVB>" -- --blend "<abs scene.blend>" --objects
```

Each line `SV_OBJECT {...}` gives a mesh's name, its box on screen in pixels, its share of the frame, its real size in meters, and `flat` (a plane or a thin board). Tell the person which objects look like the design face (flat, facing the camera, a sensible size) and ask which one it is. A design face is one flat object: a plane, or a thin box (its side facing the camera is used). If the surface is part of a bigger mesh, it must be split off in Blender first: say so.

2. Check the face before rendering (fast, no render): `BL "<SVB>" -- --blend "<abs scene.blend>" --face "<object name>" --probe` prints its box in pixels.
3. Render the passes:

```bash
BL "<SVB>" -- --blend "<abs scene.blend>" --face "<object name>" --out "<project>/scenes" --samples 128
```

Several faces: `--face "Poster,Sign"`. A screen or lightbox: `--kind emissive`. Another camera: `--camera "<name>"`. The render uses the file's own camera, lights and resolution, with Cycles and the AgX view. Tested 10 Oct on a small room with a poster board (a thin box) and a sign (a plane): both faces came back with their corners in the right order and their own masks, in 3.3 s at 32 samples; the original .blend's sha256 was unchanged.

4. Then L2 "From SV Blender", as in B1.

**Outputs, for B1 and B2**, in `<out>/<scene>/`:

| File | What it is | How L2 uses it |
| --- | --- | --- |
| `plate.png` | The render, with each design face a neutral grey | The scene |
| `light.png` | Only the design faces, white, lit by the scene; transparent elsewhere | The Light layer (Multiply, clipped) and, by its transparency, the mask |
| `mask.png`, `mask-<face>.png` | Each face white on black; anything in front of it stays black | For a face that another face overlaps |
| `corners.json` | Per face: `corners_px` (top-left, top-right, bottom-right, bottom-left), `size_m`, `aspect`, `suggested_design_px`, the camera | The `quad`, and the design's shape |
| `<scene>.blend` | The scene, saved ready to render | Not needed by L2 |

**Open the .blend in Blender yourself (optional).** Anyone who wants to see how the scene is built can open `<scene>.blend` in Blender, like the optional apps in "Want to see it in the app?". Nothing in SV Edits needs it, and the agent never opens it unless asked.

## Way 3: from a clip you already have

Phone or camera footage, or a reel made earlier. The clip is read where it is and never written to. Every step below is a prompt and a command; none needs the person to open an editor. Tested 8 Oct on 10-bit 4:2:2 camera footage and 9 Oct on 1080p H.264 reels; phone HEVC and HLG originals are untested.

Phone footage, before building anything: a vertical phone clip is often stored as landscape with a rotation flag, so check the probe for a rotation and look at the frames from step 2: if they stand upright while the probe says landscape, build the sequence the way the frames look and say so (untested). HDR (HLG or PQ in the probe's `color.transfer`) to 8-bit H.264 is untested: say so. Phones often record a variable frame rate: use the frame rate the probe reports and look at a frame either side of every cut.

**Step 1. Pre-flight** with the footage tool (ask first: 25 MB on a Mac, 34 MB on Windows). Way 3 alone needs only film; photo adds the contact sheet and covers. `keep` every clip (it only reads it; a big clip takes a few seconds).

**Step 2. Probe and show.**

```bash
SVE film probe "<clip>"            # duration in ticks: 254016000000 per second
SVE frames "<clip>" 6              # six frames and a contact sheet in ~/.sv-edits/previews/
```

`frames` prints the clip's project folder in `~/.sv-edits/projects/`. Every project for that clip goes there; a cut from several clips goes in the first clip's folder name with `-multi` (3f). Show the contact sheet.

**Step 3. Say a cut list back in seconds and wait for a yes**: which parts, in which order, how long the result is, and the size. No shot should end mid-word: ask the person where the words are, because this skill does not transcribe.

**Step 4. Write the edit list and build.** In the project folder, `cut.jsonl` (match width, height and fps to the probe, and with several clips to the first clip's probe, and say so; positions on the timeline are seconds, `sourceIn` and `duration` are ticks):

```json
{"id":"file.newSequence","params":{"name":"Cut","width":1920,"height":1080,"fps":30,"sampleRate":48000}}
{"id":"timeline.place","params":{"item":1,"track":"V1","audioTrack":"A1","seconds":0,"insert":false,"sourceIn":3048192000000,"duration":762048000000}}
{"id":"timeline.place","params":{"item":1,"track":"V1","audioTrack":"A1","seconds":3,"insert":false,"sourceIn":508032000000,"duration":762048000000}}
```

```bash
SVE film --compact --save-as "<project folder>/cut.fcproj" import "<clip>"
SVE film --compact --project "<project folder>/cut.fcproj" --save run "<project folder>/cut.jsonl"
SVE film --compact --project "<project folder>/cut.fcproj" inspect sequence
```

**Step 5. Export with the loudness set.** Write `export.json` and run it through `exec`:

```json
{"path":"<abs path from SVE name>","format":"h264","loudnessLufs":-14,"wait":true}
```

```bash
SVE film --compact --project "<project folder>/cut.fcproj" exec file.exportMedia --json-file "<project folder>/export.json"
```

Use `exec file.exportMedia` for loudness. The shorter `export <out> --settings '{"loudnessLufs":-14}'` exports without changing the loudness (tested 9 Oct: -16.1 LUFS where -14 was asked; through `exec` the same cut measured -14.1).

**A clip with no sound.** When the probe says `"audio": null` (screen recordings, many reels, muted phone clips), export with `"audio":false` and no `loudnessLufs`: `{"path":"<abs path from SVE name>","format":"h264","audio":false,"wait":true}` (tested 9 Oct: the export probed with `"audio": null`). Without it the export gets a silent AAC stereo track, and there is no loudness to set. Several clips where only some have sound: keep the audio and set the loudness, and say which parts are silent.

**Step 6. Check.** `SVE film probe` on the export (duration, size), and render a frame either side of each cut and show them:

```bash
SVE film --project "<project folder>/cut.fcproj" render --seconds 2.9 --out "<abs>/previews/cut-2.9s.png"
```

Then delete empty `FilmCraft Previews` folders in the project folder, `check` the clip, and hand over.

### 3a. Trim (tested)

One `timeline.place` with the range to keep.

### 3b. Cut parts together, or reorder them (tested 9 Oct: two ranges placed in reverse order)

One `timeline.place` per part; the order on the timeline (`seconds`) is the order in the result, whatever the order in the source. Use `file.newSequence` with sizes, or with `fromItem` to match the clip; `file.newSequenceFromClip` fails headless.

### 3c. A vertical version for Reels and TikTok (tested 9 Oct)

**Pick the size from the clip first, so nothing is enlarged.** A 9:16 crop of a landscape clip is as tall as the clip. So:

- A clip 1920 or more pixels tall (4K, or 1440p and up): 1080x1920. `clip.fillFrame` scales it down, which is fine.
- A 1080p landscape clip (1920x1080): the native crop is **608x1080** (1080 × 9 ÷ 16 = 607.5, rounded to the even 608). `clip.fillFrame` then prints `"scale":100.0` (tested 9 Oct, exported and probed at 608x1080). This is the default. Instagram and TikTok accept it and scale it up on the phone (not tested here: say so).
- 1080x1920 from a 1080p clip means enlarging it 1.78 times (`fillFrame` printed `"scale":177.8` on 9 Oct), which breaks "Never enlarge past native pixels" and comes out softer. Make it only when the person asks for exactly 1080x1920 after hearing that, and say it at hand-over.
- In general: height = the clip's height, width = height × 9 ÷ 16 rounded to an even number. Read the `scale` that `fillFrame` prints: over 100 means enlarged, so stop and tell the person.

Then a sequence of that size at the clip's fps, place the clip, select it, and fill the frame (the 1080x1920 lines below are for a clip 1920 or more pixels tall; for 1080p, write 608 and 1080):

```json
{"id":"file.newSequence","params":{"name":"Vertical","width":1080,"height":1920,"fps":30,"sampleRate":48000}}
{"id":"timeline.place","params":{"item":1,"track":"V1","audioTrack":"A1","seconds":0,"insert":false,"sourceIn":0,"duration":1270080000000}}
{"id":"timeline.select","params":{"clips":[10]}}
{"id":"clip.fillFrame","params":{}}
```

The clip id comes from the `timeline.place` result (`"clips":[10,11]`: the first is the video; a clip without sound gives one id). So build it in two passes: first a `place.jsonl` with the `newSequence` and `timeline.place` lines, then read the ids it printed and run a second `fill.jsonl` with `timeline.select` and `clip.fillFrame` (the four lines above show the content; tested 9 Oct in two passes). `clip.fillFrame` without a selection fails with "no clips selected". The crop is centred. To keep a person in frame who is off centre, move the crop by prompt ("move it left a little") with `effects.setParam` on the clip's motion position, and render a frame every second to check (documented; read its params with `SVE film describe effects.setParam`). Tracking a moving subject automatically is out of scope. Never crop a reel that already has words on it: ask SV Motion for that size.

### 3d. Export for sharing, or a master (tested 8 Oct)

H.264 for LINE, IG and TikTok (8-bit 4:2:0, AAC). A ProRes HQ master for an archive: `{"path":"<out>.mov","format":"prores","proresProfile":"hq","wait":true}`. These files are large: ask first.

### 3e. A frame becomes a PSD cover (tested 9 Oct)

Grab the frame next to the clip, then build a cover in the photo tool with live Thai type:

```bash
SVE film --project "<project folder>/frames.fcproj" render --seconds 4 --out "<clip folder>/<clip>-frame-4s.png"
SVE photo run --new-file "<jobs>/cover.json" \
  --cmd file.placeEmbedded --params-file "<jobs>/frame.json" \
  --cmd type.create --params-file "<jobs>/headline-th.json" \
  --cmd layer.layerStyle.stroke --params-file "<jobs>/stroke.json" \
  --out "<clip folder>/<clip>-cover-1280x720.psd"
```

- **Render the frame from `frames.fcproj`**, the project `SVE frames` made in the clip's project folder. Its sequence is the clip's own size, so the frame comes out at native pixels. `render` draws the sequence of the project it is given, and after 3c `cut.fcproj` holds the vertical sequence: a landscape cover from it would get an enlarged vertical frame. For a cover from a moment of the finished cut, render from the cut's project only when its sequence has the cover's shape.
- `cover.json` is `{"width":1280,"height":720,"background":"#000000","name":"cover"}`; `frame.json` is `{"path":"<abs frame png>","fit":true}`; `stroke.json` is `{"size":4,"position":"outside","color":"#000000","opacity":100}`. The stroke goes straight after the `type.create` it belongs to, in the same run, with no `layer` key (see 2a). One `type.create` and one stroke per line.
- Pick a frame with room for the words and no words of its own. A 1080x1920 cover comes from a vertical clip 1920 or more pixels tall, never from enlarging a 16:9 frame; from a vertical 1080p phone clip the cover is 1080x1920 at native size.
- Name the PSD, the JPEG and the frame in one `SVE name` call, export the JPEG at quality 85, and show it.

### 3f. Several clips in one cut (tested 9 Oct: a 16:9 and a 9:16 clip, cut to 1080x1920)

1. `keep` and `SVE frames` each clip, and say the cut list back across all of them.
2. One project for the cut, in `~/.sv-edits/projects/<first clip>-multi-<id>/`. Import every clip in one call; the item ids come back in the order given:

```bash
SVE film --compact --save-as "<project folder>/cut.fcproj" import "<clip 1>" "<clip 2>" "<clip 3>"
# prints {"errors":[],"items":[1,2,3]}: clip 1 is item 1, clip 2 is item 2
```

3. Pass one, `place.jsonl`: one `file.newSequence` (size and fps from the first clip, or for vertical the size 3c picks from the smallest clip, so no clip is enlarged; say which), then one `timeline.place` per part with its `item`, in the order of the result. Run it with `--save run`; each place prints its clip ids.
4. Pass two, for a vertical cut or clips of different shapes: `fill.jsonl` with `timeline.select` of the printed video clip ids and `clip.fillFrame`. Check the `scale` it prints per clip: any clip over 100 is enlarged, so say which and offer the smaller size.
5. Export as in step 5 to `<first clip folder>/<first clip>-cut.mp4` or `-cut-9x16.mp4` (a name from `SVE name`), then step 6: probe, a frame either side of every cut, `check` every clip.

Clips with different frame rates: the sequence takes the first clip's, and the others are conformed to it (untested for mixed rates); say so and look at frames from each clip.

**What way 3 never does:** words, titles or captions on the video (SV Motion); subject tracking; a visual timeline; hand-off files for another editor. If a clip job cannot be finished by prompting alone, say so in one sentence and stop.

## Working inside a document (the two servers)

From the session after `wire`, the agent can also work step by step inside one document. The helper route above stays the default: it works in every session, in both agents, with no registration.

`sv-photo` (rooted at the project folder; paths are relative to it, with forward slashes):

- `doc_open {"path":"sv-edits/post-1080x1350.psd"}`, `doc_inspect {}`, `command_run {"id":"type.edit","params":{...}}`, `command_batch {"steps":[{"id":"...","params":{...}}, ...]}`, `doc_render_preview` (the picture comes straight back into the chat), and `doc_save {"path":"sv-edits/post-v2.psd"}` (all tested 9 Oct).
- **The command key is `id` on sv-photo.** `command_run {"command":...}` is refused with "missing field `id`". sv-vector is the other way round: its `run_command` takes `command`. Do not mix them up.
- Always pass a new `path` to `doc_save`: without one it writes over the file that was opened (seen on 8 Oct).
- It refuses `file.placeEmbedded` and `replaceContents`. Place files with the helper (`SVE photo run ... --cmd file.placeEmbedded`).

`sv-vector` (headless; every path absolute and inside the project):

- `open_file {"path":"<absolute path>"}` opens an existing .ai, SVG, PDF, EPS or image as the active document; its `warnings` say what did not come in as it was. A session starts with a default document open, so open the file first.
- `run_command {"command":"select.all","params":{}}` runs any command id (`command`, not `id`), `draw_shape` with `"stroke":"none"`, `add_text`, `inspect_document`, `export {"path":"<absolute path>"}`, `screenshot {"path":...}` to a file.
- `save_file {"path":"<absolute path, new name>"}`: always with a new path from `SVE name`. Without one it saves over the file that was opened.

Never call the tools that need an app window: photo `ui_inspect`, `ui_screenshot`, `ui_pointer`, `ui_menu_invoke`, `ui_set`, `control_call`; vector `inspect_ui`, `type_text`, `open_panel`, `press_key`, `pointer_gesture`, `select_tool`, `invoke_menu`, and `screenshot` with `window` true.

Codex asks before each MCP call in an interactive session; `codex exec` refuses them (tested 8 Oct). Use the helper route there.

## Rules that keep an edit safe

1. **Never write over an original.** `keep` before, a name from `name`, `check` before every hand-over. If `check` says CHANGED, stop and tell the person.
2. **Params go in files.** JSON, Thai above all, goes to a UTF-8 file and is passed as `--params-file`, `--new-file`, `--settings-file` or `--json-file`. The helper inlines it, because the tools take inline JSON only. Never type Thai or JSON inline on Windows: inline `--params '{...}'` and `--new '{...}'` in this file's examples are for macOS, and on Windows each goes in a params file.
3. **Read ids fresh.** Layer ids change after a save (a run printed layer 6, `info` on the saved PSD said 4), and clip ids come from the `timeline.place` result. So a step that needs a new layer's id (a stroke) goes in the same run as the `type.create`, right after it; a later run takes ids from `info` on the saved file.
4. **Never enlarge past native pixels.**
5. **No window, unless the person asks for one.** Headless commands only; Blender only with `-b`; see "Prompt only" and "Want to see it in the app?".
6. **Ask before anything heavy:** a PSD over about 500 MB, a long 4K export, a ProRes master.
7. **Clip projects live only in `~/.sv-edits/projects/`.**
8. **Files stay on the computer.** No file goes to an editing service, no account, no key. The AI provider sees what the agent reads and the previews, like any chat image. Other network use: each tool's one-time download, macOS's notarization check, `doctor`'s reachability check, and, only after a yes, a font, desktop app or Blender download (in Codex, also the one fetch of this file).
9. **When a check fails, stop.** Keep the exact output and report it. Never route around a hash, signature or permission check.

## Checks before handing over

- Report every warning word for word: import warnings, font `substitute`, `layers flattened`.
- `info` on every PSD and .ai you made: size, layers, the live type lines, fonts.
- Logos: the `.pdf` and `.ai` object counts match the source; the outlined `.svg` has no `Type` objects and looks the same; the PDF has no `/Font` (2d).
- Fonts: every font in a PSD or .ai you touched is listed by `type.fonts`, or the hand-over says which face replaced it.
- Open every preview in the chat, and zoom on the Thai marks.
- Clips: `probe` the export (duration, size, audio), frames either side of every cut. With sound, say the loudness was set to -14 LUFS at export. With no sound in the source, say the export has no audio track and skip loudness. A vertical version: give its size and the `fillFrame` scale, and say so when it was enlarged.
- Tool warnings that point to an app feature (Find Font, a panel, a menu): report them word for word, then give the prompt-only fix (see "Prompt only").
- Mockups: `App screen` (and any swapped layer) is a Smart Object, clipped; the screenshot, logo and photo are the person's own.
- Stickers: layers `Art` and `CutContour` only, the cut line 0.25 pt in the spot colour, TrimBox the stated size and BleedBox 2 mm larger, no `/Font` and at least one `CutContour` in the PDF (V3 gives the `grep` and the PowerShell `Select-String` forms).
- Icons: each SVG holds only its own shapes, and the person has approved the drawings.
- Pairs (PSD and JPEG, .ai with SVG and PDF) carry the same name and `-vN`.
- `check` says unchanged for every original.
- Vector logo (L1): every part named, the stack right (the background group at the back), the PDF with no `/Font`, and the hand-over says it is a redraw made by the agent.
- Scenes (L2): `Design: <face>` is a Smart Object, `Light: <face>` is Multiply and clipped; a close crop of each corner; a swap only with a design of the same pixel size.
- Print (L3): `document.pdfInfo` gives the TrimBox at the stated size and the BleedBox the bleed larger; no `/Font`; `/GTS_PDFX` present; colours given as CMYK builds.
- SV Blender: Blender ran with `-b` only; the script's sha256 equals its pin; `corners.json` says `in_frame` true for every face used; a person's own .blend is unchanged (`check`).
- List each output as a clickable link with its pixel size and KB, and name the outputs that came from untested steps.

## When it fails

| Symptom | Cause and fix |
| --- | --- |
| Checksum differs from the pin or `SHA256SUMS.txt` | The helper deletes the download and stops. Try once more, on another network if possible. If it fails again, do not run anything; stop and report the exact output to the person. |
| macOS: Team ID is not `DJ6XS33FX8`, `codesign --strict` fails, or `spctl` does not say `Notarized Developer ID` | Stop. Never remove quarantine or bypass Gatekeeper. `spctl` needs the internet: retry online. |
| Windows: signature Status is not `Valid`, or the subject is not Learning Machines | Stop. An expired certificate with a timestamp is expected and fine. |
| `--version` shows another version | Wrong asset or architecture. Stop. |
| `not installed` | Ask, then `SVE install <tool>`. |
| Download fails in Codex | No network in the sandbox: approve the command. |
| PowerShell says the params are bad JSON | Quotes were stripped: use a params file. |
| `wire` says too wide | Give it the project folder, not home, Desktop, Documents or Downloads. |
| The servers' tools are missing | They appear in the next session. Keep going with the helper. |
| HEIC will not open | `sips` to JPEG first (macOS), or ask for a JPEG. |
| Thai shows as boxes or broken marks in a PSD or .ai | The face lacks Thai. List the fonts and use Sukhumvit Set or Thonburi (macOS), Leelawadee UI (Windows). |
| Thai boxes on video | The footage tool cannot draw Thai. Words on video belong to SV Motion. |
| .ai opens as a placeholder page | Saved without PDF compatibility. Ask for PDF, SVG or EPS. |
| `convert` to .ai refused | Use `run` with `file.saveCopy`. |
| `info` says `substitute` or `missing` for a font | Use an installed face and say so, or outline the text (`--outline-text`). |
| Black prints grey | 100% K exports as `#343334` here. Send the RGB PDF and let the printer convert. |
| A thin black outline on every shape | Set the stroke to none. |
| Text stroke invisible | Pass `opacity` 100. |
| New colour ignored in `type.edit` | Colour and size go in `runs`. |
| Layer not found after a save ("no such layer LayerId(6)") | Ids changed: run `info` on the saved file and use its ids, or put the step in the same run as the `type.create` it belongs to. |
| sv-photo: "failed to deserialize parameters: missing field `id`" | `command_run` and `command_batch` steps take `id`, not `command`. |
| Edited words came out in another face, no warning | The font is not installed: see "Fonts this computer does not have". |
| `fillFrame` printed a `scale` over 100 | The clip was enlarged: use the native size from 3c, or say so. |
| A silent AAC track on a clip that had no sound | Export with `"audio":false` (step 5). |
| `wire` FAIL row | Its detail ends with the agent's own message: report it word for word. |
| Small photos came out soft | They were enlarged. Skip photos under the target size. |
| WebP bigger than the JPEG | WebP is lossless only here. Use JPEG. |
| A big PSD crashes | Over about 500 MB. Ask for a smaller export of it. |
| An icon's SVG holds the other icons' paths | Select first: `select.allOnArtboard`, then `document.export` with `selectedOnly` true (V2). |
| `replaceContents` swapped nothing, or another layer | Put `layer.select` with the layer's id first, in the same run (M2). |
| A die-cut PDF without a cut line the shop can see | The cut line must be on its own layer, stroked with the `CutContour` spot swatch, no fill (V3). Check the PDF with `grep -a -c 'CutContour'` (PowerShell: `(Select-String -Path "<pdf>" -Pattern 'CutContour' -SimpleMatch).Count`). |
| An app DMG or MSI hash differs from the pin | Delete it, stop and report. Never open it. |
| `clip.fillFrame`: no clips selected | `timeline.select` the clip first. |
| `newSequenceFromClip` fails | Use `file.newSequence` with `fromItem` or sizes. |
| Export loudness not -14 | `--settings` ignores it: export through `exec file.exportMedia`. |
| A `FilmCraft Previews` folder appeared | Delete the empty ones in the project folder. |
| Export slow, fans loud | About 6 cores in use. Close apps, export once. |
| Codex says a tool call requires approval | `codex exec` refuses MCP calls. Use the helper route. |
| A password prompt, Smart App Control or AppLocker | Stop. SV Edits never needs any of them. |
| L1: the letters vanished after grouping | A group covers them: `object.arrange.sendToBack` on the background group (L1 step 5). |
| L1: a new shape has a thin black outline | `paint.setStroke {"none":true}` after its fill. |
| L2: the design is stretched or spills past the surface | The design is not in the surface's shape, or `rect` is not where it landed: `rect` is the design's box centred on the scene, at 100%. |
| L2: the swap spills over the surface | The new design is another pixel size. Make it the same size as the first. |
| L2: the design looks flat and too bright on a photo | The light layer's `inWhite` is too high: read the surface with `document.pixel` again. |
| Blender: `codesign --strict` says a sealed resource is missing | Blender's own Python wrote `__pycache__` files into the app. Check the Team ID and version instead; run with `PYTHONDONTWRITEBYTECODE=1`. Anything else changed: stop. |
| Blender: the first render is slow | Cycles compiles its GPU kernels once per computer (the tote took 113 s on a fresh home, 41 s once warm). |
| Blender: `SV_ERROR no camera` or `no mesh object named` | Use `--objects` to list the names, `--camera` to pick a camera. |
| Blender: the shopfront screens are plain dark | No `python3` with Pillow (or no command line tools on a Mac): the code picture was skipped, nothing else changes. |
| Blender: a Metal error aborts a render | Run the same command once more (seen twice in look-dev, 9 Oct). |

## Fix it in words

Read the file and its params first. Change only what was asked, run that one job again into a new name, show it, `check`. Leave every other output alone.

- The PSD keeps live type and the logo layer; the .ai keeps paths and live text; the clip project keeps the timeline.
- "Make the Thai line bigger": one `size` in the params file, `type.edit`, convert, show.
- "White logo instead": one `recolor.apply`, export, show.
- "Start the cut a second later": one `sourceIn` in `cut.jsonl`, build, export, show.

## Want to see it in the app? (optional)

อยากเปิดดูในโปรแกรม (ไม่บังคับ)

Prompting stays the default, and nothing in SV Edits needs an app. A class or a demo never depends on one being open. But anyone who wants to **see** a file the agent made, or move something by hand, can use the free open source desktop apps of the same two tools, at the same pinned versions:

- **PhotoCraft 0.5.0** opens the PSD files (and PNG, JPEG, TIFF, WebP).
- **VectorCraft 0.5.0** opens the .ai (PDF-compatible), SVG and PDF files.
- Clips have no app path here: SV Edits teaches no timeline editing.

**Only to look, on a Mac: nothing to install.** Quick Look (select the file in Finder and press Space) or Preview shows a flat view of a PSD and of a PDF-compatible .ai (tested 9 Oct with `qlmanage -t` thumbnails of the M1 PSD and the V3 .ai: both drawn right, Thai included). Offer that first. PhotoCraft or VectorCraft is for seeing the layers or changing something by hand.

**Rules for the agent.**

- **Look for an app that is already there first**, before any question about a download. On a Mac: `ls -d /Applications/PhotoCraft.app ~/Applications/PhotoCraft.app 2>/dev/null` (and the same for VectorCraft), then the step 3 checks on the path found. If they pass at the pinned version, nothing is downloaded: say where it is. On Windows: the search in "On Windows" below.
- Install an app **only when the person asks for it**, after one question that names the app, the download size and where it goes. Never as part of Pre-flight, a preset or a fix.
- **Never launch it unless asked.** When the person asks the agent to open a file in the app: `open -a PhotoCraft "<file.psd>"` or `open -a VectorCraft "<file.ai>"` on a Mac (untested), `Start-Process "<path to the app exe>" -ArgumentList '"<file>"'` on Windows (untested). Otherwise say where the file is and let the person open it.
- The helper does none of this: it stays command-line only, and its checks are unchanged. These are separate steps, run by hand from this section.
- Every download is checked like the tools: the size and sha256 must equal the pins below and the line in that release's `SHA256SUMS.txt`. If either differs, delete the download, stop and report the exact output.

Pins (GitHub asset digests, checked against each release's `SHA256SUMS.txt` on 9 Oct 2026; PhotoCraft 0.5.0 on 10 Oct). URL form as for the tools: `https://github.com/storytold/<app>/releases/download/v<version>/<asset>`.

| App | macOS (DMG, universal) | Windows x64 (MSI) | Windows arm64 (MSI) | Windows x86 (MSI) |
| --- | --- | --- | --- | --- |
| PhotoCraft 0.5.0 | `photocraft-0.5.0-macos-universal.dmg`, 74,849,198, `dff8c8105d5938d46fa4ba29559d3efc0f1ea195a5392d36cf62bc14ea2de5e7` | `photocraft-0.5.0-windows-x64.msi`, 55,783,424, `ad4e1a1e574bae0b0eb0ea9dd7538da70a6e5d94d441488d94e1ccc32830be38` | `photocraft-0.5.0-windows-arm64.msi`, 52,805,632, `d1e81892c837d38b394401def73aac87b8bf59d4df0fa8fa43f73341dd397b88` | `photocraft-0.5.0-windows-x86.msi`, 54,583,296, `48f0bf5657aa6b62c1c6c14ca665aea4d7ae0eef5fc3ec8416920897055ca487` |
| VectorCraft 0.5.0 | `vectorcraft-0.5.0-macos-universal.dmg`, 83,864,169, `871c6099353574719478819b63ec7319c4f3f71cd61b0053d4c8e2da0820d0e2` | `vectorcraft-0.5.0-windows-x64.msi`, 61,227,008, `64395a763c106280fdde7b95d3cb0e97a7fb1a9a963bf4b6008bad3e11e7b733` | `vectorcraft-0.5.0-windows-arm64.msi`, 56,516,608, `190f9a6963eb654abc98fc23bdc62c140b0df5d19f9f01a83bed0faea9305ab9` | `vectorcraft-0.5.0-windows-x86.msi`, 59,367,424, `998534e6c1f29b10418fdd047b6176048e5eef080a3d8972edbea590b94de97b` |

The Windows portable zips are the ones already pinned under "Pin the versions"; each holds the app as `<app>.exe` in its folder.

### On a Mac

1. **Download and check**, after the person's yes (shown for PhotoCraft; VectorCraft is the same with its own name, version and pin). The DMG goes to `~/Downloads`, where the person can see it (Finder hides `~/.sv-edits`):

```bash
curl -fL --proto '=https' -o ~/Downloads/photocraft-0.5.0-macos-universal.dmg \
  https://github.com/storytold/photocraft/releases/download/v0.5.0/photocraft-0.5.0-macos-universal.dmg
stat -f %z ~/Downloads/photocraft-0.5.0-macos-universal.dmg          # must be 74849198
shasum -a 256 ~/Downloads/photocraft-0.5.0-macos-universal.dmg       # must equal the pin
curl -fsSL https://github.com/storytold/photocraft/releases/download/v0.5.0/SHA256SUMS.txt | grep 'macos-universal.dmg'   # the same hash
```

If a file of that name is already in Downloads, check it the same way before downloading again.

2. **The person drags it in.** Give them a clickable link to the DMG (`file:///Users/<name>/Downloads/photocraft-0.5.0-macos-universal.dmg`, and the folder `file:///Users/<name>/Downloads/`). They double-click it and drag PhotoCraft to Applications. This is the one step done by hand. The agent never runs `open` on the DMG, and never mounts it, unless the person asks for exactly that.
   - **If macOS asks for a password** (a standard, non-admin account), the person clicks Cancel and drags PhotoCraft to the `Applications` folder inside their home folder instead (`~/Applications`; the agent may create it with `mkdir -p ~/Applications` and give its `file://` link). No password is ever needed.
3. **Check what landed**, on whichever path holds the app, `/Applications/PhotoCraft.app` or `~/Applications/PhotoCraft.app` (tested 9 Oct on this release's apps in /Applications on the test Mac, read only, nothing opened):

```bash
APP=/Applications/PhotoCraft.app; [ -d "$APP" ] || APP=~/Applications/PhotoCraft.app
spctl --assess --type execute -vv "$APP"      # accepted, source=Notarized Developer ID, Learning Machines LLC (DJ6XS33FX8)
/usr/libexec/PlistBuddy -c 'Print :CFBundleShortVersionString' "$APP/Contents/Info.plist"   # 0.5.0
```

If `spctl` says anything else, stop and report it. Never remove quarantine or bypass Gatekeeper.

4. **Open it.** The app is notarized and `spctl` has just accepted it, so it opens like any app. (A file fetched with `curl` carries no "downloaded from the internet" mark, so macOS does not ask its usual first-open question; the `spctl` check in step 3 is the proof instead.) Then File, Open, and pick the PSD or .ai the agent made, or drag the file onto the app's icon.
5. Once the app is in Applications, the DMG can go: the agent deletes `~/Downloads/<asset>` after a yes. To remove the app later, the person drags it to the Trash.

### On Windows (untested)

**Already installed?** Look first, read only:

```powershell
Get-ChildItem "$env:ProgramFiles", "${env:ProgramFiles(x86)}", "$env:LOCALAPPDATA\Programs", "$HOME\.sv-edits\apps" -Recurse -Depth 3 -Filter photocraft.exe -ErrorAction SilentlyContinue | Select-Object -ExpandProperty FullName
```

A path found there is `<path to the app exe>` for `Start-Process`; check its signature (`Get-AuthenticodeSignature`, Status `Valid`, a Learning Machines subject) and its version (`(Get-Item "<path>").VersionInfo.ProductVersion`, 0.5.0) before using it. The same search with `vectorcraft.exe` finds VectorCraft.

Otherwise pick one:

- **The MSI installer** for the computer's CPU (the helper's `doctor` names it: x64, arm64 or x86). Download it to `%USERPROFILE%\.sv-edits\apps\` with `curl.exe -fL -o`, check `(Get-Item <file>).Length` and `(Get-FileHash -Algorithm SHA256 <file>).Hash.ToLower()` against the pin and `SHA256SUMS.txt`, and check `Get-AuthenticodeSignature <file>` (Status `Valid`, a Learning Machines subject). Give the person a clickable link to the MSI; they double-click it. Where it installs is not documented for the pinned release: after the install, run the search above to find the app. **Any Windows prompt during the install, a User Account Control Yes/No or a password, is the person's decision**: the agent never answers it and never asks the person to click Yes. If they would rather not, they cancel and use the portable zip.
- **The portable zip** (no installer, no admin): the same download and checks with the zip from "Pin the versions", then `Expand-Archive` into `%USERPROFILE%\.sv-edits\apps\<app>-<version>\`. The app is `<app>.exe` in the folder `<app>-<version>-windows-<arch>-portable\`; check its signature the same way. This downloads the same zip the helper fetched for the command-line tool (65 MB for photo on x64) a second time, because the helper keeps only the `-cli.exe` tool and deleted the rest unrun: say so in the question. This copy is separate from the helper's, and the helper never runs it.

If SmartScreen, Smart App Control or a password prompt appears, stop and let the person decide; the agent never clicks past it. Then File, Open the PSD or .ai the agent made. To remove: Settings, Apps for the MSI, or delete the folder for the portable copy.

### After a change by hand

The app saves under a new name (File, Save As), never over the agent's file. Then the person tells the agent the new file's name, and it is treated like any file they bring (way 2): `keep`, `info`, `look`, and the font check before any type edit. Prompting carries on from there.

## Codex and other agents

This skill is plain markdown in one file. An agent without skill support can be told:

```text
Read https://raw.githubusercontent.com/sva-admin/sv-edits/v1.1.0/SKILL.md and follow it for everything we do in this session. Then get this computer ready for it.
```

The URL names the release tag `v1.1.0`, never `main`: `main` moves, and the helper pins live inside this file, so they cannot catch a file that changed between two fetches. Pre-flight step 2 saves the file from this same URL. A new release gets a new tag, and this line and step 2 change together. Nothing else needs to be fetched: the helpers are inside this file. Notes for Codex:

- **Where the skill file goes**, so the next session still has it: Claude Code installs it as `~/.claude/skills/sv-edits/SKILL.md`. In Codex, copy the file Pre-flight step 2 saved (`~/.sv-edits/SKILL.md`, from the tagged URL) to `~/.codex/skills/sv-edits/SKILL.md` (untested), or paste the raw-URL line at the start of each session.
- Start Codex in the project folder. Its sandbox writes only inside the workspace. Prefer one approved write to `~/.sv-edits`: the tools then install once per computer, and the at-home pre-install works.
- Only if that write is refused: use `<project>/.sv-edits/` and that path in the helper line (`bash <project>/.sv-edits/edits.sh`, or the PowerShell line with that path). Everything then lives there, jobs included (`<project>/.sv-edits/jobs/`), and the consent question names that folder. The tools are downloaded again for every project, and the folder sits inside sv-photo's root. `install` adds `.sv-edits/` to `<project>/.git/info/exclude` (local only, no tracked file changes), because the binaries are over 100 MB and must never be committed: say so. Do not confuse it with way 1's `sv-edits/` output folder beside it.
- Approve network access for the first install, and approve `install` outside the sandbox: `spctl` needs the system's policy service and the network. Expect one approval prompt.
- `wire` is optional in Codex and has its own question (Pre-flight step 5): `codex mcp add` writes `~/.codex/config.toml`, which every Codex session reads, and needs an approval outside the sandbox. Interactive Codex asks before each MCP call; `codex exec` refuses them. The helper route covers every job without the servers. Codex driving the helper is untested until the cold test.
- Show every result with the image viewer, never by describing file names.
- Trigger phrases are Claude Code mechanics. In Codex, the raw-URL line above is the trigger.

## License and credit

SV Edits is SV Academy's own skill, in our own words, MIT. It copies no code or documentation from anyone; command ids and parameters are facts read from the tools.

Credit: the three tools are separate open source editors from storytold, [PhotoCraft](https://github.com/storytold/photocraft), [VectorCraft](https://github.com/storytold/vectorcraft) and [FilmCraft](https://github.com/storytold/filmcraft) (Apache-2.0 or MIT). They are downloaded from their own releases, run unmodified and never redistributed.

More from Silicon Valley Academy: <https://github.com/sva-admin/claude-skills> and <https://sv-academy.org>

## Helper scripts

Two helper scripts and the SV Blender script, all SV Academy's own, MIT. Pre-flight extracts the helper for this OS from this file (never retyped), checks its sha256 against the pin in step 2, then runs it. "SV Blender" does the same for its script, the last block. Line 1 is the stamp; the pin table sits at the top. Exit codes: 0 all checks passed, 1 a check failed, 2 a usage error. The last output of every verb is a PASS/FAIL table.

| Verb | What it does |
| --- | --- |
| `doctor` | Read only: OS, CPU, shell, disk, RAM, network, helper stamp, each tool's version, sha256 and signature, the agent commands on PATH, the Thai fonts the photo tool sees |
| `install photo\|vector\|film\|all [--from <folder>]` | The only verb that downloads: the checks under "Pre-flight" |
| `wire claude\|codex <project folder> [--dry-run]` | Registers `sv-photo` (rooted at the project) and `sv-vector` (headless) |
| `photo\|vector\|film <arguments>` | Runs the pinned tool. Reads `--params-file`, `--new-file`, `--settings-file` and `--json-file` from UTF-8 files. Refuses `--in-place`, `--bridge`, `--connect`, `--control-token`, `--demo` and servers |
| `keep <file>...` | Fingerprints originals before any edit |
| `check [<file>...]` | Proves every kept original is unchanged |
| `name <wanted path>...` | Prints one free name per path: `-v2`, `-v3` on a clash, the same `-vN` for every path in the call, never an original |
| `look <file>...` | Previews at most 1600 px wide in `previews/` for PSD, AI, SVG, PDF, EPS and images, plus pixel size and KB |
| `frames <clip> [n]` | Probe, a project in `projects/`, n frames and a contact sheet, empty preview folders removed |
| `where` | The install folder and the lines that remove SV Edits. The agent runs them only when the person asks to remove SV Edits, after confirming once |

Status: the macOS helper's `doctor`, `wire --dry-run`, `photo`, `vector`, `film`, `keep`, `check`, `name`, `look` and `frames` were tested on 9 Oct 2026 against the pinned builds; `install` is untested end to end on a fresh Mac (its checks mirror an installer that passed on 8 Oct), and `wire` has run only as `--dry-run`, plus a failing add against a stand-in `claude` command, which showed the agent's message in the FAIL row (9 Oct, in a sandbox). `name` with several paths was tested on 9 Oct. The Windows helper is untested.

**Helper pins** are under Pre-flight step 2, where they are used. Change them there whenever a block below changes.

### ~/.sv-edits/edits.sh (macOS)

Runs on macOS's bash 3.2 (no associative arrays, no `mapfile`; pins are looked up with `case`).

```bash
# sv-edits helper v1: photo 0.5.0, vector 0.5.0, film 0.2.1
# SV Edits helper for macOS (Linux is not pinned). SV Academy's own script, MIT.
# The agent writes this file from SKILL.md to ~/.sv-edits/edits.sh and runs it with bash:
#   bash ~/.sv-edits/edits.sh <verb> [arguments]
# Verbs: doctor, install, wire, photo, vector, film, keep, check, name, look, frames, where.
# Exit codes: 0 all checks passed, 1 a check failed, 2 usage error.
# Needs only what macOS ships: bash 3.2, curl, shasum, ditto, codesign, spctl, sips.
# Installs command-line tools only: never an app, a DMG or an installer. No sudo, no PATH or
# profile edits, no quarantine or Gatekeeper changes, ever.

STAMP='sv-edits helper v1: photo 0.5.0, vector 0.5.0, film 0.2.1'
TEAM_ID='DJ6XS33FX8'
# Home: the folder this script sits in (~/.sv-edits), unless SV_EDITS_HOME names another one.
SVE_HOME="${SV_EDITS_HOME:-$(cd "$(dirname "$0")" && pwd)}"
SELF="$SVE_HOME/edits.sh"
PREV="$SVE_HOME/previews"
PROJ="$SVE_HOME/projects"
KEPT="$SVE_HOME/kept.sha256"

# ---- pin table (macOS universal CLI zips; GitHub asset digests, re-read 9 Oct 2026, photo 0.5.0 on 10 Oct) ----
app_of() {
  case "$1" in
    photo) echo photocraft ;;
    vector) echo vectorcraft ;;
    film) echo filmcraft ;;
    *) return 1 ;;
  esac
}
ver_of() {
  case "$1" in
    photocraft) echo 0.5.0 ;;
    vectorcraft) echo 0.5.0 ;;
    filmcraft) echo 0.2.1 ;;
  esac
}
pin_of() {
  case "$1" in
    photocraft) echo "photocraft-cli-0.5.0-macos-universal.zip 57316934 8af20fa1254a17f75cd99ccf1d25b6c9ad5cfb51c6a47ff7463c228ef25ad01c" ;;
    vectorcraft) echo "vectorcraft-cli-0.5.0-macos-universal.zip 64607865 64307411bc17988827dd5ce417798b3b5c20450a1ce5a4c7738202b24030b3d0" ;;
    filmcraft) echo "filmcraft-cli-0.2.1-macos-universal.zip 24675100 18535c3c82e4de9156251d73cf4d8f3626638ee3979063dcf4c966e811b666f6" ;;
  esac
}
bin_of() { echo "$SVE_HOME/$1-$(ver_of "$1")/$1-cli"; }

# ---- result table: every verb ends with it ----
RESULTS=''
FAILED=0
row() {
  RESULTS="${RESULTS}$(printf '%-5s  %-28s  %s' "$1" "$2" "$3")
"
  if [ "$1" = FAIL ]; then FAILED=1; fi
}
table() {
  printf '\n%-5s  %-28s  %s\n' RESULT CHECK DETAIL
  printf '%s' "$RESULTS"
  if [ "$FAILED" -eq 0 ]; then echo "PASS: $1"; else echo "FAIL: $1"; fi
}
usage() {
  cat >&2 <<EOF
usage: bash "$SELF" <verb>
  doctor                              read only: OS, CPU, disk, RAM, network, tools, Thai fonts
  install photo|vector|film|all [--from <folder>]
  wire claude|codex <project folder> [--dry-run]
                                      register the photo and vector MCP servers (sv-photo, sv-vector)
  photo|vector|film <cli arguments>   run the pinned tool; --params-file, --new-file,
                                      --settings-file and --json-file are read from UTF-8 files
  keep <file>...                      fingerprint originals before any edit
  check [<file>...]                   prove every kept original is unchanged
  name <wanted output path>...        free names next to the original (adds -v2, -v3; one -vN for all)
  look <file>...                      preview images (at most 1600 px wide) to open in the chat
  frames <clip> [n]                   n evenly spaced frames and a contact sheet (default 6)
  where                               install folder and the lines that remove it
EOF
  exit 2
}
kb() { echo $(( ($(wc -c < "$1" | tr -d ' ') + 1023) / 1024 )); }
lower() { printf '%s' "$1" | tr 'A-Z' 'a-z'; }
abspath() { (cd "$(dirname "$1")" 2>/dev/null && printf '%s/%s' "$(pwd)" "$(basename "$1")"); }
sha() { shasum -a 256 "$1" | awk '{print $1}'; }
need_cmds() {
  for c in "$@"; do
    command -v "$c" >/dev/null 2>&1 || { row FAIL "command $c" "missing: this helper needs what macOS ships"; return 1; }
  done
}
team_of() { codesign -dv --verbose=2 "$1" 2>&1 | sed -n 's/^TeamIdentifier=//p'; }
need_tool() {
  b=$(bin_of "$1")
  [ -x "$b" ] || { echo "SV Edits: the $2 tool is not installed. Ask the person first, then run: bash \"$SELF\" install $2" >&2; exit 1; }
}

# ---- doctor (read only) ----
check_installed() {
  app=$1; ver=$(ver_of "$app"); bin=$(bin_of "$app"); rec="$SVE_HOME/$app-$ver/.sv-edits-install"
  if [ ! -x "$bin" ]; then
    row INFO "$app $ver" "not installed (fine if this way does not need it)"
    return 0
  fi
  out=$("$bin" --version 2>&1 | head -1)
  case "$out" in
    "$app-cli $ver"|"$app-cli $ver "*) row PASS "$app version" "$out" ;;
    *) row FAIL "$app version" "expected $ver, got: $out" ;;
  esac
  zip_sha=$(set -- $(pin_of "$app"); echo "$3")
  if [ -f "$rec" ] && grep -q "^zip_sha256=$zip_sha\$" "$rec"; then
    if grep -q "^cli_sha256=$(sha "$bin")\$" "$rec"; then row PASS "$app sha256" "zip matched the pin at install; binary unchanged since"
    else row FAIL "$app sha256" "binary changed since install: delete $SVE_HOME/$app-$ver and install again"; fi
  else
    row FAIL "$app sha256" "no install record: delete $SVE_HOME/$app-$ver and install again"
  fi
  if codesign --verify --strict "$bin" >/dev/null 2>&1 && [ "$(team_of "$bin")" = "$TEAM_ID" ]; then
    row PASS "$app signature" "Developer ID, Team ID $TEAM_ID"
  else
    row FAIL "$app signature" "codesign --strict or Team ID failed: stop and report this output"
  fi
}
doctor() {
  if [ "$(uname -s)" = Darwin ]; then
    v=$(sw_vers -productVersion); major=${v%%.*}
    if [ "$major" -ge 11 ] 2>/dev/null; then row PASS "macOS" "$v ($(uname -m), universal build)"; else row FAIL "macOS" "$v: needs 11 or newer"; fi
    mem=$(( $(sysctl -n hw.memsize) / 1073741824 ))
    if [ "$mem" -le 8 ]; then row WARN "RAM" "$mem GB: one job at a time, clips only short 1080p"; else row PASS "RAM" "$mem GB"; fi
  else
    row FAIL "OS" "$(uname -s): this helper is for macOS. On Windows use edits.ps1"
  fi
  row INFO "shell" "bash ${BASH_VERSION:-unknown}"
  need_cmds curl shasum ditto codesign spctl sips && row PASS "built-in commands" "curl shasum ditto codesign spctl sips"
  free=$(df -k "$HOME" | awk 'NR==2 {printf "%d", $4/1048576}')
  if [ "${free:-0}" -ge 1 ]; then row PASS "disk free" "$free GB"; else row FAIL "disk free" "${free:-0} GB: free about 1 GB"; fi
  if curl -sS -o /dev/null -I --max-time 8 https://github.com 2>/dev/null; then row PASS "network" "github.com reachable"
  else row WARN "network" "github.com not reachable: installs and the macOS notarization check need it"; fi
  if [ "$(head -1 "$0")" = "# $STAMP" ]; then row PASS "helper" "$STAMP"; else row FAIL "helper" "stamp differs from SKILL.md: write the helper again"; fi
  row INFO "helper sha256" "$(tr -d '\r' < "$0" | shasum -a 256 | awk '{print $1}') (must equal the pin in SKILL.md)"
  for app in photocraft vectorcraft filmcraft; do check_installed "$app"; done
  for a in claude codex; do
    if command -v "$a" >/dev/null 2>&1; then row INFO "agent command" "$a found (wire can register the servers)"; fi
  done
  pbin=$(bin_of photocraft)
  if [ -x "$pbin" ]; then
    thai=$("$pbin" run --new '{"width":64,"height":64}' --cmd type.fonts 2>/dev/null \
      | grep -o -E '"(Thonburi|Sukhumvit Set|Sarabun|Prompt|Kanit|Noto Sans Thai|Noto Serif Thai|IBM Plex Sans Thai|IBM Plex Sans Thai Looped|Ayuthaya|Silom|Krungthep)"' \
      | tr -d '"' | sort -u | paste -sd ',' - | sed 's/,/, /g')
    if [ -n "$thai" ]; then row PASS "Thai fonts (photo tool)" "$thai"; else row WARN "Thai fonts (photo tool)" "none of the known Thai faces found"; fi
  fi
  table "doctor ($SVE_HOME)"
  exit "$FAILED"
}

# ---- install (the only verb that downloads) ----
cleanup_tmp() { [ -n "$TMPD" ] && rm -rf "$TMPD"; [ -n "$STAGED" ] && rm -rf "$STAGED"; TMPD=''; STAGED=''; }
fail_install() { row FAIL "$1" "$2"; cleanup_tmp; table "install stopped, nothing was installed for this tool"; exit 1; }
install_one() {
  app=$1; ver=$(ver_of "$app"); set -- $(pin_of "$app"); asset=$1; bytes=$2; pinsha=$3
  dest="$SVE_HOME/$app-$ver"
  if [ -x "$dest/$app-cli" ] && [ -f "$dest/.sv-edits-install" ]; then
    row PASS "$app $ver" "already installed (doctor re-checks it)"; return 0
  fi
  mkdir -p "$SVE_HOME" || fail_install "$app folder" "cannot create $SVE_HOME"
  TMPD=$(mktemp -d "$SVE_HOME/.download.XXXXXX") || fail_install "$app download" "cannot create a temporary folder"
  if [ -n "$FROM" ]; then
    src="$FROM/$app-$ver"
    cp "$src/$asset" "$TMPD/$asset" 2>/dev/null && cp "$src/SHA256SUMS.txt" "$TMPD/SHA256SUMS.txt" 2>/dev/null \
      || fail_install "$app copy" "expected $src/$asset and $src/SHA256SUMS.txt"
  else
    base="https://github.com/storytold/$app/releases/download/v$ver"
    echo "Downloading $asset ($(( bytes / 1000000 )) MB) from github.com/storytold/$app ..."
    curl -fL --proto '=https' --proto-redir '=https' --retry 2 -o "$TMPD/$asset" "$base/$asset" \
      || fail_install "$app download" "curl failed for $base/$asset"
    curl -fsSL --proto '=https' --proto-redir '=https' --retry 2 -o "$TMPD/SHA256SUMS.txt" "$base/SHA256SUMS.txt" \
      || fail_install "$app download" "curl failed for $base/SHA256SUMS.txt"
  fi
  size=$(wc -c < "$TMPD/$asset" | tr -d ' ')
  [ "$size" = "$bytes" ] || fail_install "$app size" "expected $bytes bytes, got $size"
  got=$(sha "$TMPD/$asset")
  [ "$got" = "$pinsha" ] || fail_install "$app sha256 (pin)" "expected $pinsha, got $got"
  listed=$(awk -v n="$asset" '{f=$2; sub(/^\*/, "", f); sub(/^\.\//, "", f); if (f == n) print tolower($1)}' "$TMPD/SHA256SUMS.txt")
  [ -n "$listed" ] || fail_install "$app sha256 (SHA256SUMS)" "$asset is not listed in SHA256SUMS.txt"
  [ "$(printf '%s\n' "$listed" | sort -u)" = "$pinsha" ] || fail_install "$app sha256 (SHA256SUMS)" "SHA256SUMS.txt says $listed"
  row PASS "$app sha256" "matches the pin and SHA256SUMS.txt"
  ditto -x -k "$TMPD/$asset" "$TMPD/x" || fail_install "$app unpack" "ditto could not unpack $asset"
  found=$(find "$TMPD/x" -type f -name "$app-cli" ! -path '*.app/*')
  [ -n "$found" ] && [ "$(printf '%s\n' "$found" | wc -l | tr -d ' ')" = 1 ] || fail_install "$app unpack" "expected exactly one $app-cli in the zip"
  codesign --verify --strict "$found" >/dev/null 2>&1 || fail_install "$app signature" "codesign --verify --strict failed"
  team=$(team_of "$found")
  [ "$team" = "$TEAM_ID" ] || fail_install "$app signature" "TeamIdentifier is '$team', expected $TEAM_ID"
  sp=$(spctl --assess --type install -vv "$found" 2>&1)
  case "$sp" in
    *accepted*"Notarized Developer ID"*) row PASS "$app signature" "Team ID $TEAM_ID, notarized" ;;
    *) fail_install "$app notarization" "spctl did not say 'accepted' and 'Notarized Developer ID' (it needs the internet): $(printf '%s' "$sp" | tr '\n' ' ')" ;;
  esac
  vout=$("$found" --version 2>&1 | head -1)
  case "$vout" in
    "$app-cli $ver"|"$app-cli $ver "*) row PASS "$app version" "$vout" ;;
    *) fail_install "$app version" "expected $ver, got: $vout" ;;
  esac
  STAGED="$SVE_HOME/.$app-$ver.new"
  rm -rf "$STAGED"
  ditto "$(dirname "$found")" "$STAGED" || fail_install "$app copy" "ditto into $SVE_HOME failed"
  codesign --verify --strict "$STAGED/$app-cli" >/dev/null 2>&1 && [ "$(team_of "$STAGED/$app-cli")" = "$TEAM_ID" ] \
    || fail_install "$app signature (copy)" "the copied binary failed codesign --strict"
  {
    echo "app=$app"; echo "version=$ver"; echo "asset=$asset"; echo "zip_sha256=$pinsha"
    echo "cli_sha256=$(sha "$STAGED/$app-cli")"
    echo "team_id=$TEAM_ID"; echo "installed_at=$(date -u +%Y-%m-%dT%H:%M:%SZ)"
  } > "$STAGED/.sv-edits-install"
  rm -rf "$dest"
  mv "$STAGED" "$dest" || fail_install "$app copy" "could not move the checked copy into place"
  STAGED=''
  cleanup_tmp
  row PASS "$app $ver" "installed in $dest"
}
write_installed_json() {
  f="$SVE_HOME/installed.json"; first=1
  { printf '{\n  "helper": "%s",\n  "tools": [' "$STAMP"
    for app in photocraft vectorcraft filmcraft; do
      m="$SVE_HOME/$app-$(ver_of "$app")/.sv-edits-install"
      [ -f "$m" ] || continue
      [ $first -eq 1 ] || printf ','
      first=0
      printf '\n    {"app": "%s", "version": "%s", "zip_sha256": "%s", "installed_at": "%s"}' \
        "$app" "$(sed -n 's/^version=//p' "$m")" "$(sed -n 's/^zip_sha256=//p' "$m")" "$(sed -n 's/^installed_at=//p' "$m")"
    done
    printf '\n  ]\n}\n'
  } > "$f"
}
install_verb() {
  [ "$(uname -s)" = Darwin ] || { echo "SV Edits: this helper installs macOS builds only. On Windows use edits.ps1." >&2; exit 2; }
  what=$1; shift
  FROM=''
  if [ "$1" = --from ]; then FROM=$2; [ -d "$FROM" ] || usage; fi
  case "$what" in
    photo|vector|film) apps=$(app_of "$what") ;;
    all) apps="photocraft vectorcraft filmcraft" ;;
    *) usage ;;
  esac
  need_cmds curl shasum ditto codesign spctl || { table "install"; exit 1; }
  trap cleanup_tmp EXIT
  # The helper folder inside a git repository (Codex's project-local .sv-edits): keep it out of git,
  # locally only, so 100+ MB binaries are never committed. Changes no tracked file.
  parent=$(dirname "$SVE_HOME")
  if [ -d "$parent/.git" ]; then
    mkdir -p "$parent/.git/info"
    grep -qx "$(basename "$SVE_HOME")/" "$parent/.git/info/exclude" 2>/dev/null \
      || printf '%s/\n' "$(basename "$SVE_HOME")" >> "$parent/.git/info/exclude"
    row INFO "git" "$(basename "$SVE_HOME")/ is in $parent/.git/info/exclude (local only, nothing committed)"
  fi
  for app in $apps; do install_one "$app"; done
  write_installed_json
  table "install ($SVE_HOME)"
  exit "$FAILED"
}

# ---- wire: register the MCP servers (a standing change: only after the person's yes) ----
wire() {
  agent=$1; dir=$2; dry=$3
  case "$agent" in claude|codex) ;; *) usage ;; esac
  [ -n "$dir" ] && [ -d "$dir" ] || { echo "SV Edits: wire needs the person's project folder" >&2; exit 2; }
  dir=$(cd "$dir" && pwd)
  case "$dir" in "$HOME"|/|/Users|/Users/*/Desktop|/Users/*/Documents|/Users/*/Downloads)
    echo "SV Edits: $dir is too wide. Root the photo server at one project folder." >&2; exit 2 ;; esac
  if [ "$dry" != --dry-run ]; then
    command -v "$agent" >/dev/null 2>&1 || { row FAIL "$agent" "the $agent command is not on PATH: run wire from a shell that has it"; table "wire"; exit 1; }
  fi
  pbin=$(bin_of photocraft); vbin=$(bin_of vectorcraft)
  # Claude Code: local scope, run inside the project folder, so the servers exist only there.
  # The agent's own output is kept, so a failed add shows its reason in the table.
  WIRE_OUT=''
  run_or_print() {
    if [ "$dry" = --dry-run ]; then printf 'would run (in %s):' "$dir"; printf ' %q' "$@"; printf '\n'; return 0; fi
    WIRE_OUT=$(cd "$dir" && "$@" 2>&1)
  }
  why() { printf '%s' "$WIRE_OUT" | tr '\n' ' ' | cut -c1-300; }
  if [ "$agent" = claude ]; then rm_cmd="claude mcp remove -s local"; add_cmd="claude mcp add -s local"; else rm_cmd="codex mcp remove"; add_cmd="codex mcp add"; fi
  if [ -x "$pbin" ]; then
    run_or_print $rm_cmd sv-photo || true
    if run_or_print $add_cmd sv-photo -- "$pbin" mcp --automation-read-root "$dir" --automation-write-root "$dir"; then
      row PASS "sv-photo ($agent)" "reads and writes only inside $dir"
    else row FAIL "sv-photo ($agent)" "$add_cmd failed: $(why)"; fi
  else row INFO "sv-photo" "photo tool not installed: not registered"; fi
  if [ -x "$vbin" ]; then
    run_or_print $rm_cmd sv-vector || true
    if run_or_print $add_cmd sv-vector -- "$vbin" mcp --headless; then
      row PASS "sv-vector ($agent)" "headless, no window"
    else row FAIL "sv-vector ($agent)" "$add_cmd failed: $(why)"; fi
  else row INFO "sv-vector" "vector tool not installed: not registered"; fi
  row INFO "next session" "the servers' tools appear in the next $agent session; the CLI route works now"
  table "wire $agent"
  exit "$FAILED"
}

# ---- run a pinned tool ----
read_json() {
  [ -f "$1" ] || { echo "SV Edits: no such file: $1" >&2; exit 2; }
  c=$(cat "$1")
  bom=$(printf '\357\273\277')
  c=${c#"$bom"}
  case "$(printf '%s' "$c" | tr -d ' \t\r\n' | cut -c1)" in
    '{'|'[') printf '%s' "$c" ;;
    *) echo "SV Edits: $1 does not start with { or [, so it is not JSON" >&2; exit 2 ;;
  esac
}
run_tool() {
  tool=$1; shift
  app=$(app_of "$tool"); need_tool "$app" "$tool"; bin=$(bin_of "$app")
  for x in "$@"; do
    case "$x" in
      mcp|serve) echo "SV Edits: servers are registered only with: bash \"$SELF\" wire" >&2; exit 2 ;;
    esac
  done
  args=()
  while [ $# -gt 0 ]; do
    case "$1" in
      --params-file|--new-file|--settings-file)
        [ $# -ge 2 ] || usage
        j=$(read_json "$2") || exit 2
        args+=("${1%-file}" "$j"); shift 2 ;;
      --json-file)
        [ $# -ge 2 ] || usage
        j=$(read_json "$2") || exit 2
        args+=("$j"); shift 2 ;;
      --in-place|--bridge|--bridge=*|--connect|--connect=*|--control-token|--control-token-file|--demo)
        echo "SV Edits: $1 is not allowed (originals never change, never an app window)" >&2; exit 2 ;;
      *) args+=("$1"); shift ;;
    esac
  done
  "$bin" "${args[@]}"
  exit $?
}

# ---- keep, check, name: the original never changes ----
keep() {
  [ $# -ge 1 ] || usage
  mkdir -p "$SVE_HOME"; touch "$KEPT"
  for f in "$@"; do
    if [ ! -f "$f" ]; then row FAIL "$f" "not found"; continue; fi
    a=$(abspath "$f"); h=$(sha "$a")
    awk -v p="$a" '{ l = $0; sub(/^[0-9a-f]+  /, "", l); if (l != p) print }' "$KEPT" > "$KEPT.tmp"; mv "$KEPT.tmp" "$KEPT"
    printf '%s  %s\n' "$h" "$a" >> "$KEPT"
    size=$(wc -c < "$a" | tr -d ' ')
    if [ "$size" -gt 524288000 ]; then row WARN "$(basename "$a")" "kept; over about 500 MB: ask before opening it in the photo tool"
    else row PASS "$(basename "$a")" "kept, $(kb "$a") KB, sha256 ${h%"${h#????????????}"}"; fi
  done
  table "keep (fingerprints in $KEPT)"
  exit "$FAILED"
}
check() {
  [ -s "$KEPT" ] || { row FAIL "kept.sha256" "nothing kept yet: run keep on the originals first"; table "check"; exit 1; }
  while IFS= read -r line; do
    h=${line%%  *}; a=${line#*  }
    if [ $# -gt 0 ]; then
      hit=0; for f in "$@"; do [ "$(abspath "$f")" = "$a" ] && hit=1; done
      [ $hit -eq 1 ] || continue
    fi
    if [ ! -f "$a" ]; then row WARN "$(basename "$a")" "no longer at $a"
    elif [ "$(sha "$a")" = "$h" ]; then row PASS "$(basename "$a")" "unchanged"
    else row FAIL "$(basename "$a")" "CHANGED since keep: stop and tell the person"; fi
  done < "$KEPT"
  for f in "$@"; do is_kept "$(abspath "$f")" || row FAIL "$f" "was never kept: run keep before editing"; done
  table "check (every original must say unchanged)"
  exit "$FAILED"
}
is_kept() {
  [ -f "$KEPT" ] || return 1
  awk -v p="$1" '{ l = $0; sub(/^[0-9a-f]+  /, "", l); if (l == p) f = 1 } END { exit !f }' "$KEPT"
}
variant() {
  # variant <path> <k>: k 1 is the path itself, k 2 adds -v2 before the extension, and so on.
  vw=$1; vk=$2
  [ "$vk" -eq 1 ] && { printf '%s' "$vw"; return 0; }
  vd=$(dirname "$vw"); vb=$(basename "$vw"); vs=${vb%.*}; ve=${vb##*.}; [ "$vs" = "$vb" ] && ve=''
  if [ -n "$ve" ]; then printf '%s/%s-v%s.%s' "$vd" "$vs" "$vk" "$ve"; else printf '%s/%s-v%s' "$vd" "$vs" "$vk"; fi
}
name() {
  # One free name per path, all with the same -vN, so a PSD and its JPEG keep matching names.
  [ $# -ge 1 ] || usage
  for want in "$@"; do
    [ -d "$(dirname "$want")" ] || { echo "SV Edits: no such folder: $(dirname "$want")" >&2; exit 2; }
  done
  k=1
  while :; do
    clash=0; out=''
    for want in "$@"; do
      cand=$(variant "$want" "$k")
      if [ -e "$cand" ] || is_kept "$(abspath "$cand")"; then clash=1; break; fi
      out="$out$cand
"
    done
    [ "$clash" -eq 0 ] && { printf '%s' "$out"; return 0; }
    k=$((k + 1))
  done
}

# ---- look: previews at most 1600 px wide, for the agent to open in the chat ----
look_one() {
  f=$(abspath "$1"); base=$(basename "$f"); ext=$(lower "${base##*.}")
  stem=$(printf '%s' "${base%.*}" | tr -c 'A-Za-z0-9._\n-' '-')
  src="$f"
  case "$ext" in
    png|jpg|jpeg|tif|tiff|gif|bmp|heic|webp) ;;
    psd|pcraft|psb)
      need_tool photocraft photo; src="$PREV/.$stem-$ext.flat.png"
      "$(bin_of photocraft)" convert "$f" "$src" >/dev/null 2>&1 || { row WARN "$base" "the photo tool could not render it"; return 0; } ;;
    ai|svg|svgz|pdf|eps|vectorcraft)
      need_tool vectorcraft vector; src="$PREV/.$stem-$ext.render.png"
      "$(bin_of vectorcraft)" convert "$f" "$src" >/dev/null 2>&1 || { row WARN "$base" "the vector tool could not render it"; return 0; } ;;
    mp4|mov|m4v|mxf|avi|mts|mkv|webm) row INFO "$base" "a clip: use frames"; return 0 ;;
    *) row INFO "$base" "not an image, PSD, vector file or clip"; return 0 ;;
  esac
  w=$(sips -g pixelWidth "$src" 2>/dev/null | awk '/pixelWidth/ {print $2}')
  h=$(sips -g pixelHeight "$src" 2>/dev/null | awk '/pixelHeight/ {print $2}')
  [ -n "$w" ] || { row WARN "$base" "could not read it"; return 0; }
  if [ "$ext" = jpg ] || [ "$ext" = jpeg ] || [ "$ext" = heic ]; then fmt=jpeg; pext=jpg; else fmt=png; pext=png; fi
  p="$PREV/$stem-$ext.preview.$pext"
  if [ "$w" -gt 1600 ]; then sips -s format "$fmt" --resampleWidth 1600 "$src" --out "$p" >/dev/null 2>&1
  else sips -s format "$fmt" "$src" --out "$p" >/dev/null 2>&1; fi
  [ "$src" != "$f" ] && rm -f "$src"
  if [ -f "$p" ]; then row PASS "$base" "${w}x${h} px, $(kb "$f") KB, preview: $p"
  else row WARN "$base" "${w}x${h} px, $(kb "$f") KB, no preview written"; fi
}
look() {
  [ $# -ge 1 ] || usage
  mkdir -p "$PREV"
  for f in "$@"; do
    if [ -f "$f" ]; then look_one "$f"; else row FAIL "$f" "not found"; fi
  done
  table "look (open every preview in the chat before handing over)"
  exit "$FAILED"
}

# ---- frames: n evenly spaced frames from a clip ----
frames() {
  [ $# -ge 1 ] || usage
  clip=$1; n=${2:-6}
  case "$n" in ''|*[!0-9]*) usage ;; esac
  [ -f "$clip" ] || { echo "SV Edits: no such clip: $clip" >&2; exit 2; }
  clip=$(abspath "$clip")
  need_tool filmcraft film; fbin=$(bin_of filmcraft)
  stem=$(lower "$(basename "${clip%.*}")" | tr -c 'a-z0-9\n' '-')
  pdir="$PROJ/$stem-$(printf '%s' "$clip" | shasum -a 256 | cut -c1-6)"
  mkdir -p "$pdir" "$PREV"
  probe=$("$fbin" probe "$clip" 2>&1) || { row FAIL "probe" "$(printf '%s' "$probe" | head -1)"; table "frames"; exit 1; }
  dur=$(printf '%s\n' "$probe" | sed -n 's/^  "duration": \([0-9][0-9]*\).*/\1/p' | head -1)
  w=$(printf '%s\n' "$probe" | sed -n 's/^ *"width": \([0-9][0-9]*\).*/\1/p' | head -1)
  h=$(printf '%s\n' "$probe" | sed -n 's/^ *"height": \([0-9][0-9]*\).*/\1/p' | head -1)
  [ -n "$dur" ] || { row FAIL "probe" "no duration found"; table "frames"; exit 1; }
  secs=$(awk -v d="$dur" 'BEGIN {printf "%.2f", d / 254016000000}')
  row PASS "probe" "${w}x${h}, $secs s"
  tag="$stem-frames"; k=2
  while [ -e "$PREV/$tag" ]; do tag="$stem-frames-$k"; k=$((k + 1)); done
  proj="$pdir/frames.fcproj"; fdir="$PREV/$tag"
  mkdir -p "$fdir"
  cd "$pdir" || exit 1
  rm -f "$proj"
  imp=$("$fbin" --compact --save-as "$proj" import "$clip" 2>&1) || { row FAIL "import" "$imp"; table "frames"; exit 1; }
  item=$(printf '%s' "$imp" | sed -n 's/.*"items":\[\([0-9][0-9]*\).*/\1/p' | head -1)
  [ -n "$item" ] || { row FAIL "import" "no item id in: $imp"; table "frames"; exit 1; }
  seq=$("$fbin" --compact --project "$proj" --save exec file.newSequence "{\"name\":\"Frames\",\"fromItem\":$item}" 2>&1) \
    || { row FAIL "sequence" "$seq"; table "frames"; exit 1; }
  i=1
  while [ "$i" -le "$n" ]; do
    t=$(awk -v d="$secs" -v i="$i" -v n="$n" 'BEGIN {printf "%.2f", d * (i - 0.5) / n}')
    p="$fdir/frame-$i-${t}s.png"
    if "$fbin" --project "$proj" render --seconds "$t" --out "$p" >/dev/null 2>&1 && [ -f "$p" ]; then
      pw=$(sips -g pixelWidth "$p" 2>/dev/null | awk '/pixelWidth/ {print $2}')
      [ -n "$pw" ] && [ "$pw" -gt 1600 ] && sips --resampleWidth 1600 "$p" >/dev/null 2>&1
      row PASS "frame $i at $t s" "$p"
    else
      row FAIL "frame $i at $t s" "render failed"
    fi
    i=$((i + 1))
  done
  pbin=$(bin_of photocraft)
  if [ -x "$pbin" ]; then
    sheet="$PREV/$tag-contact-sheet.png"; rows=$(( (n + 2) / 3 ))
    if "$pbin" run --new '{"width":64,"height":64}' --cmd file.automate.contactSheetII \
        --params "{\"input\":\"$fdir\",\"units\":\"pixels\",\"width\":1800,\"height\":$((rows * 600)),\"resolution\":72,\"columns\":3,\"rows\":$rows,\"caption\":true,\"flatten\":true}" \
        --out "$sheet" >/dev/null 2>&1 && [ -f "$sheet" ]; then
      row PASS "contact sheet" "$sheet"
    else
      row INFO "contact sheet" "not made: open the frames one by one"
    fi
  else
    row INFO "contact sheet" "needs the photo tool: open the frames one by one"
  fi
  [ -d "$pdir/FilmCraft Previews" ] && find "$pdir/FilmCraft Previews" -depth -type d -empty -delete
  row INFO "project folder" "$pdir (every project for this clip goes here; the clip is only read)"
  table "frames"
  exit "$FAILED"
}

where() {
  echo "Tools, helper, previews and clip projects: $SVE_HOME"
  for app in photocraft vectorcraft filmcraft; do
    d="$SVE_HOME/$app-$(ver_of "$app")"
    if [ -d "$d" ]; then echo "  $app $(ver_of "$app"): $d ($(du -sh "$d" | awk '{print $1}'))"; else echo "  $app: not installed"; fi
  done
  echo "Your files are never inside this folder, and removing it never touches them."
  echo "To remove SV Edits (the agent runs these only when the person asks, after confirming once):"
  echo "  in the project folder: claude mcp remove -s local sv-photo; claude mcp remove -s local sv-vector   (or: codex mcp remove sv-photo; codex mcp remove sv-vector)"
  echo "  rm -rf \"$SVE_HOME\""
  row PASS "where" "$SVE_HOME"
  table "where"
}

verb=$1
[ $# -gt 0 ] && shift
case "$verb" in
  doctor) doctor ;;
  install) [ $# -ge 1 ] || usage; install_verb "$@" ;;
  wire) wire "$@" ;;
  photo|vector|film) run_tool "$verb" "$@" ;;
  keep) keep "$@" ;;
  check) check "$@" ;;
  name) name "$@" ;;
  look) look "$@" ;;
  frames) frames "$@" ;;
  where) where ;;
  *) usage ;;
esac
```

### %USERPROFILE%\.sv-edits\edits.ps1 (Windows, untested)

Plain ASCII, so Windows PowerShell 5.1 reads it the same way in any locale.

```powershell
# sv-edits helper v1: photo 0.5.0, vector 0.5.0, film 0.2.1
# SV Edits helper for Windows 10 and 11 (UNTESTED on Windows: written from the release files). SV Academy's own script, MIT.
# The agent writes this file from SKILL.md to %USERPROFILE%\.sv-edits\edits.ps1 and runs it with:
#   powershell -NoProfile -ExecutionPolicy Bypass -File "$HOME\.sv-edits\edits.ps1" <verb> [arguments]
# From Git Bash (Claude Code on Windows) the same line works with "$USERPROFILE/.sv-edits/edits.ps1".
# The Bypass applies to that one process only. Never change the machine's execution policy.
# Verbs: doctor, install, wire, photo, vector, film, keep, check, name, look, frames, where.
# Exit codes: 0 all checks passed, 1 a check failed, 2 usage error.
# Needs only what Windows ships: PowerShell 5.1, curl.exe, Get-FileHash, Expand-Archive,
# Get-AuthenticodeSignature, System.Drawing. Portable command-line tools only: no MSI, no app,
# no admin, no PATH change.

$Argv = @($args)
$Stamp = 'sv-edits helper v1: photo 0.5.0, vector 0.5.0, film 0.2.1'
# Home: the folder this script sits in, unless SV_EDITS_HOME names another one.
$SveHome = if ($env:SV_EDITS_HOME) { $env:SV_EDITS_HOME } else { $PSScriptRoot }
$Self = Join-Path $SveHome 'edits.ps1'
$Prev = Join-Path $SveHome 'previews'
$Proj = Join-Path $SveHome 'projects'
$Kept = Join-Path $SveHome 'kept.sha256'
try { [Console]::OutputEncoding = New-Object System.Text.UTF8Encoding $false } catch { }
$Utf8 = New-Object System.Text.UTF8Encoding $false

# ---- pin table (Windows portable zips; GitHub asset digests, re-read 9 Oct 2026, photo 0.5.0 on 10 Oct) ----
# Each zip holds one folder <app>-<version>-windows-<arch>-portable\ with <app>.exe (the desktop
# app, never kept) and <app>-cli.exe (the command-line tool), per packaging/windows/package.ps1.
$Apps = @{ photo = 'photocraft'; vector = 'vectorcraft'; film = 'filmcraft' }
$Vers = @{ photocraft = '0.5.0'; vectorcraft = '0.5.0'; filmcraft = '0.2.1' }
$Pins = @{
  'photocraft:X64'    = @('photocraft-0.5.0-windows-x64-portable.zip', 67717284, 'da0402c19aa1b65f3cde310461cc6e41d391791e6097659a34006e526fb9d9cd')
  'photocraft:Arm64'  = @('photocraft-0.5.0-windows-arm64-portable.zip', 65047499, 'eafcc98083b427fecf50c968e7083f6252878e04518b615355c810d66319ab72')
  'photocraft:X86'    = @('photocraft-0.5.0-windows-x86-portable.zip', 65425226, '3b5b328bef203a7e0f8eb49ae7c34aecabb29c36eb62b64b4b44dacdbe025eab')
  'vectorcraft:X64'   = @('vectorcraft-0.5.0-windows-x64-portable.zip', 77469769, '98929a4a49383a16cc2e12da92281efc0bd2c7dc44112adf11859689b2c2e4c1')
  'vectorcraft:Arm64' = @('vectorcraft-0.5.0-windows-arm64-portable.zip', 73123015, '2ba33f1c3e6a0bca2ac06386bbfb5472d82004522d2e6aceb93a71c9aab28e71')
  'vectorcraft:X86'   = @('vectorcraft-0.5.0-windows-x86-portable.zip', 74188285, '72df3fc76ac2a2db8d5cfee0d67664126e4b59699b668bae69ced845352efb65')
  'filmcraft:X64'     = @('filmcraft-0.2.1-windows-x64-portable.zip', 33724271, '5306251e23de051523895dfa791ad3e3324c9865944df3e6a4ad590521657942')
  'filmcraft:X86'     = @('filmcraft-0.2.1-windows-x86-portable.zip', 32902572, 'e401205d3b4dd124398660b33bdd890364832c90704e1fdc9a5995037f27e1d0')
}

# ---- result table: every verb ends with it ----
$script:Rows = New-Object System.Collections.ArrayList
$script:Failed = 0
function Row([string]$s, [string]$c, [string]$d) {
  [void]$script:Rows.Add(('{0,-5}  {1,-28}  {2}' -f $s, $c, $d))
  if ($s -eq 'FAIL') { $script:Failed = 1 }
}
function Show-Table([string]$title) {
  ''
  '{0,-5}  {1,-28}  {2}' -f 'RESULT', 'CHECK', 'DETAIL'
  $script:Rows | ForEach-Object { $_ }
  if ($script:Failed -eq 0) { "PASS: $title" } else { "FAIL: $title" }
}
function Usage {
  [Console]::Error.WriteLine("usage: powershell -NoProfile -ExecutionPolicy Bypass -File `"$Self`" <verb>")
  [Console]::Error.WriteLine('  doctor | install photo|vector|film|all [--from <folder>] | wire claude|codex <project folder> [--dry-run]')
  [Console]::Error.WriteLine('  photo|vector|film <cli arguments> | keep <file>... | check [<file>...] | name <wanted output path>...')
  [Console]::Error.WriteLine('  look <file>... | frames <clip> [n] | where')
  exit 2
}
function Get-Arch {
  # The real machine, not this process: an x64 PowerShell under emulation on Arm64 still says Arm64.
  try {
    if ((Get-CimInstance Win32_OperatingSystem).OSArchitecture -like '32*') { return 'X86' }
    $cpu = (Get-CimInstance Win32_Processor | Select-Object -First 1).Architecture
    if ($cpu -eq 12) { return 'Arm64' }
    if ($cpu -eq 9) { return 'X64' }
  } catch { }
  $a = if ($env:PROCESSOR_ARCHITEW6432) { $env:PROCESSOR_ARCHITEW6432 } else { $env:PROCESSOR_ARCHITECTURE }
  switch ($a) { 'AMD64' { 'X64' } 'ARM64' { 'Arm64' } default { 'X86' } }
}
function Get-Pin([string]$app) {
  $arch = Get-Arch
  $p = $Pins["${app}:$arch"]
  if (-not $p -and $arch -eq 'Arm64') {
    # No Arm64 build at this version: Windows 11 on Arm runs x64 under emulation, Windows 10 on Arm only x86.
    if ([Environment]::OSVersion.Version.Build -ge 22000) { $p = $Pins["${app}:X64"] } else { $p = $Pins["${app}:X86"] }
  }
  return $p
}
function Exe-Of([string]$app) { Join-Path $SveHome "$app-$($Vers[$app])\$app-cli.exe" }
function Kb([string]$f) { [math]::Ceiling((Get-Item -LiteralPath $f).Length / 1024) }
function Sha([string]$f) { (Get-FileHash -Algorithm SHA256 -LiteralPath $f).Hash.ToLower() }
function Full([string]$p) { $ExecutionContext.SessionState.Path.GetUnresolvedProviderPathFromPSPath($p) }
function Need-Tool([string]$app, [string]$tool) {
  if (-not (Test-Path -LiteralPath (Exe-Of $app))) {
    [Console]::Error.WriteLine("SV Edits: the $tool tool is not installed. Ask the person first, then run: powershell -NoProfile -ExecutionPolicy Bypass -File `"$Self`" install $tool")
    exit 1
  }
}
function Test-Signature([string]$exe) {
  $sig = Get-AuthenticodeSignature -LiteralPath $exe
  if ($sig.Status -ne 'Valid') { return "signature status is $($sig.Status)" }
  if ($sig.SignerCertificate.Subject -notmatch 'Learning Machines') { return "signer is $($sig.SignerCertificate.Subject)" }
  if ($sig.SignerCertificate.Issuer -notlike 'CN=Microsoft ID Verified CS*') { return "issuer is $($sig.SignerCertificate.Issuer)" }
  if (-not $sig.TimeStamperCertificate) { return 'no timestamp on the signature' }
  return ''
}
function Q([string]$s) {
  # PowerShell before 7.3 drops inner double quotes when it calls a program: escape them first.
  if ($PSVersionTable.PSVersion -lt [version]'7.3') { return [regex]::Replace($s, '(\\*)"', '$1$1\"') }
  return $s
}

# ---- doctor (read only) ----
function Doctor {
  $v = [Environment]::OSVersion.Version
  $arch = Get-Arch
  if ($v.Major -ge 10) { Row 'PASS' 'Windows' "$v ($arch)" } else { Row 'FAIL' 'Windows' "$v ($arch): needs Windows 10 or 11" }
  if ($arch -eq 'Arm64') { Row 'INFO' 'footage tool on Arm64' 'no Arm64 build at film 0.2.1: it runs under emulation (untested)' }
  $shell = if ($env:MSYSTEM) { "called from Git Bash ($env:MSYSTEM)" } else { 'PowerShell' }
  Row 'INFO' 'shell' "$shell, PowerShell $($PSVersionTable.PSVersion)"
  if (Get-Command curl.exe -ErrorAction SilentlyContinue) { Row 'PASS' 'curl.exe' 'present' } else { Row 'FAIL' 'curl.exe' 'missing: Windows 10 1803 or newer ships it' }
  try {
    $drive = Get-PSDrive -Name $env:USERPROFILE.Substring(0, 1)
    $free = [math]::Floor($drive.Free / 1GB)
    if ($free -ge 1) { Row 'PASS' 'disk free' "$free GB" } else { Row 'FAIL' 'disk free' "$free GB: free about 1 GB" }
  } catch { Row 'WARN' 'disk free' 'could not read' }
  try {
    $mem = [math]::Round((Get-CimInstance Win32_ComputerSystem).TotalPhysicalMemory / 1GB)
    if ($mem -le 8) { Row 'WARN' 'RAM' "$mem GB: one job at a time, clips only short 1080p" } else { Row 'PASS' 'RAM' "$mem GB" }
  } catch { Row 'WARN' 'RAM' 'could not read' }
  & curl.exe -sS -o NUL -I --max-time 8 https://github.com 2>$null
  if ($LASTEXITCODE -eq 0) { Row 'PASS' 'network' 'github.com reachable' } else { Row 'WARN' 'network' 'github.com not reachable: installs need it' }
  if ((Get-Content -LiteralPath $PSCommandPath -TotalCount 1) -eq "# $Stamp") { Row 'PASS' 'helper' $Stamp } else { Row 'FAIL' 'helper' 'stamp differs from SKILL.md: write the helper again' }
  $hb = [Text.Encoding]::ASCII.GetBytes(([IO.File]::ReadAllText($PSCommandPath) -replace "`r", ''))
  $hs = (([Security.Cryptography.SHA256]::Create().ComputeHash($hb)) | ForEach-Object { $_.ToString('x2') }) -join ''
  Row 'INFO' 'helper sha256' "$hs (must equal the pin in SKILL.md)"
  foreach ($app in 'photocraft', 'vectorcraft', 'filmcraft') {
    $ver = $Vers[$app]; $exe = Exe-Of $app; $rec = Join-Path $SveHome "$app-$ver\.sv-edits-install"
    if (-not (Test-Path -LiteralPath $exe)) { Row 'INFO' "$app $ver" 'not installed (fine if this way does not need it)'; continue }
    $out = (& $exe --version 2>&1 | Select-Object -First 1) -as [string]
    if ($out -like "$app-cli $ver*") { Row 'PASS' "$app version" $out } else { Row 'FAIL' "$app version" "expected $ver, got: $out" }
    if ((Test-Path -LiteralPath $rec) -and ((Get-Content -LiteralPath $rec) -contains "cli_sha256=$(Sha $exe)")) { Row 'PASS' "$app sha256" 'zip matched the pin at install; binary unchanged since' }
    else { Row 'FAIL' "$app sha256" "binary changed or no install record: delete $(Split-Path $exe) and install again" }
    $why = Test-Signature $exe
    if ($why -eq '') { Row 'PASS' "$app signature" 'Valid, Learning Machines, timestamped' } else { Row 'FAIL' "$app signature" $why }
  }
  foreach ($a in 'claude', 'codex') { if (Get-Command $a -ErrorAction SilentlyContinue) { Row 'INFO' 'agent command' "$a found (wire can register the servers)" } }
  $pexe = Exe-Of 'photocraft'
  if (Test-Path -LiteralPath $pexe) {
    $fonts = (& $pexe run --new (Q '{"width":64,"height":64}') --cmd type.fonts 2>$null) -join ''
    $thai = @('Leelawadee UI', 'Leelawadee', 'Tahoma', 'Sarabun', 'Prompt', 'Kanit', 'Noto Sans Thai', 'IBM Plex Sans Thai', 'IBM Plex Sans Thai Looped') | Where-Object { $fonts -match ('"' + [regex]::Escape($_) + '"') }
    if ($thai) { Row 'PASS' 'Thai fonts (photo tool)' ($thai -join ', ') } else { Row 'WARN' 'Thai fonts (photo tool)' 'none of the known Thai faces found' }
  }
  Show-Table "doctor ($SveHome)"
  exit $script:Failed
}

# ---- install (the only verb that downloads) ----
$script:Tmp = ''; $script:Staged = ''
function Clean-Tmp {
  if ($script:Tmp -and (Test-Path -LiteralPath $script:Tmp)) { Remove-Item -LiteralPath $script:Tmp -Recurse -Force -ErrorAction SilentlyContinue }
  if ($script:Staged -and (Test-Path -LiteralPath $script:Staged)) { Remove-Item -LiteralPath $script:Staged -Recurse -Force -ErrorAction SilentlyContinue }
  $script:Tmp = ''; $script:Staged = ''
}
function Fail-Install([string]$c, [string]$d) { Row 'FAIL' $c $d; Clean-Tmp; Show-Table 'install stopped, nothing was installed for this tool'; exit 1 }
function Install-One([string]$app, [string]$from) {
  $ver = $Vers[$app]; $pin = Get-Pin $app
  if (-not $pin) { Fail-Install "$app" "no build for $(Get-Arch) at this version" }
  $asset = $pin[0]; $bytes = $pin[1]; $pinsha = $pin[2]
  if ((Get-Arch) -eq 'Arm64' -and $asset -notlike '*-arm64-*') { Row 'WARN' "$app" "no Arm64 build at this version: using $asset under emulation (untested)" }
  $dest = Join-Path $SveHome "$app-$ver"
  if ((Test-Path -LiteralPath (Join-Path $dest "$app-cli.exe")) -and (Test-Path -LiteralPath (Join-Path $dest '.sv-edits-install'))) { Row 'PASS' "$app $ver" 'already installed (doctor re-checks it)'; return }
  New-Item -ItemType Directory -Force -Path $SveHome | Out-Null
  $script:Tmp = Join-Path $SveHome ('.download.' + [guid]::NewGuid().ToString('N').Substring(0, 8))
  New-Item -ItemType Directory -Force -Path $script:Tmp | Out-Null
  $zip = Join-Path $script:Tmp $asset; $sums = Join-Path $script:Tmp 'SHA256SUMS.txt'
  if ($from) {
    try { Copy-Item -LiteralPath (Join-Path $from "$app-$ver\$asset") $zip -ErrorAction Stop; Copy-Item -LiteralPath (Join-Path $from "$app-$ver\SHA256SUMS.txt") $sums -ErrorAction Stop }
    catch { Fail-Install "$app copy" "expected $from\$app-$ver\$asset and SHA256SUMS.txt" }
  } else {
    $base = "https://github.com/storytold/$app/releases/download/v$ver"
    "Downloading $asset ($([math]::Round($bytes / 1000000)) MB) from github.com/storytold/$app ..."
    & curl.exe -fL --proto '=https' --proto-redir '=https' --retry 2 -o $zip "$base/$asset"
    if ($LASTEXITCODE -ne 0) { Fail-Install "$app download" "curl.exe failed for $base/$asset" }
    & curl.exe -fsSL --proto '=https' --proto-redir '=https' --retry 2 -o $sums "$base/SHA256SUMS.txt"
    if ($LASTEXITCODE -ne 0) { Fail-Install "$app download" "curl.exe failed for $base/SHA256SUMS.txt" }
  }
  $size = (Get-Item -LiteralPath $zip).Length
  if ($size -ne $bytes) { Fail-Install "$app size" "expected $bytes bytes, got $size" }
  $got = Sha $zip
  if ($got -ne $pinsha) { Fail-Install "$app sha256 (pin)" "expected $pinsha, got $got" }
  $listed = @(Get-Content -LiteralPath $sums | ForEach-Object {
      if ($_ -match '^([0-9a-fA-F]{64})\s+\*?(?:\./)?(.+?)\s*$' -and $Matches[2] -eq $asset) { $Matches[1].ToLower() } } | Select-Object -Unique)
  if ($listed.Count -ne 1 -or $listed[0] -ne $pinsha) { Fail-Install "$app sha256 (SHA256SUMS)" "SHA256SUMS.txt does not list $asset with the pinned hash" }
  Row 'PASS' "$app sha256" 'matches the pin and SHA256SUMS.txt'
  try { Expand-Archive -LiteralPath $zip -DestinationPath (Join-Path $script:Tmp 'x') -ErrorAction Stop } catch { Fail-Install "$app unpack" 'Expand-Archive failed' }
  $found = @(Get-ChildItem -LiteralPath (Join-Path $script:Tmp 'x') -Recurse -File -Filter "$app-cli.exe")
  if ($found.Count -ne 1) { Fail-Install "$app unpack" "expected exactly one $app-cli.exe in the zip" }
  $why = Test-Signature $found[0].FullName
  if ($why -ne '') { Fail-Install "$app signature" $why }
  Row 'PASS' "$app signature" 'Valid, Learning Machines, timestamped'
  $vout = (& $found[0].FullName --version 2>&1 | Select-Object -First 1) -as [string]
  if ($vout -notlike "$app-cli $ver*") { Fail-Install "$app version" "expected $ver, got: $vout" }
  Row 'PASS' "$app version" $vout
  $script:Staged = Join-Path $SveHome ".$app-$ver.new"
  if (Test-Path -LiteralPath $script:Staged) { Remove-Item -LiteralPath $script:Staged -Recurse -Force }
  New-Item -ItemType Directory -Force -Path $script:Staged | Out-Null
  # Keep the command-line tool, its licences and portable.txt (which keeps settings beside the tool
  # instead of %APPDATA%, if the tool honours it: untested). The desktop app (<app>.exe) is never kept.
  Get-ChildItem -LiteralPath $found[0].DirectoryName -File | Where-Object { $_.Name -eq "$app-cli.exe" -or $_.Name -like 'LICENSE*' -or $_.Name -like 'OFL-*' -or $_.Name -eq 'README.md' -or $_.Name -eq 'portable.txt' } |
    ForEach-Object { Copy-Item -LiteralPath $_.FullName (Join-Path $script:Staged $_.Name) }
  $landed = Join-Path $script:Staged "$app-cli.exe"
  $why = Test-Signature $landed
  if ($why -ne '') { Fail-Install "$app signature (copy)" $why }
  $rec = @("app=$app", "version=$ver", "asset=$asset", "zip_sha256=$pinsha", "cli_sha256=$(Sha $landed)", "installed_at=$((Get-Date).ToUniversalTime().ToString('yyyy-MM-ddTHH:mm:ssZ', [Globalization.CultureInfo]::InvariantCulture))")
  [System.IO.File]::WriteAllLines((Join-Path $script:Staged '.sv-edits-install'), $rec, $Utf8)
  if (Test-Path -LiteralPath $dest) { Remove-Item -LiteralPath $dest -Recurse -Force }
  Rename-Item -LiteralPath $script:Staged -NewName "$app-$ver"
  $script:Staged = ''
  Clean-Tmp
  Row 'PASS' "$app $ver" "installed in $dest"
}
function Write-InstalledJson {
  $tools = @()
  foreach ($app in 'photocraft', 'vectorcraft', 'filmcraft') {
    $m = Join-Path $SveHome "$app-$($Vers[$app])\.sv-edits-install"
    if (-not (Test-Path -LiteralPath $m)) { continue }
    $kv = @{}; Get-Content -LiteralPath $m | ForEach-Object { $p = $_.Split('=', 2); $kv[$p[0]] = $p[1] }
    $tools += '    {"app": "' + $app + '", "version": "' + $kv['version'] + '", "asset": "' + $kv['asset'] + '", "zip_sha256": "' + $kv['zip_sha256'] + '", "installed_at": "' + $kv['installed_at'] + '"}'
  }
  $json = "{`n  `"helper`": `"$Stamp`",`n  `"tools`": [`n" + ($tools -join ",`n") + "`n  ]`n}`n"
  [System.IO.File]::WriteAllText((Join-Path $SveHome 'installed.json'), $json, $Utf8)
}
function Install-Verb([string[]]$a) {
  if ($a.Count -lt 1) { Usage }
  $from = ''
  if ($a.Count -ge 3 -and $a[1] -eq '--from') { $from = $a[2]; if (-not (Test-Path -LiteralPath $from)) { Usage } }
  switch ($a[0]) {
    'all' { $list = @('photocraft', 'vectorcraft', 'filmcraft') }
    default { if ($Apps.ContainsKey($a[0])) { $list = @($Apps[$a[0]]) } else { Usage } }
  }
  if (-not (Get-Command curl.exe -ErrorAction SilentlyContinue)) { Row 'FAIL' 'curl.exe' 'missing'; Show-Table 'install'; exit 1 }
  # The helper folder inside a git repository (Codex's project-local .sv-edits): keep it out of git, locally only.
  $parent = Split-Path -Parent $SveHome; $leaf = Split-Path -Leaf $SveHome
  if (Test-Path -LiteralPath (Join-Path $parent '.git') -PathType Container) {
    $info = Join-Path $parent '.git\info'; New-Item -ItemType Directory -Force -Path $info | Out-Null
    $ex = Join-Path $info 'exclude'
    if (-not ((Test-Path -LiteralPath $ex) -and ((Get-Content -LiteralPath $ex) -contains "$leaf/"))) { [System.IO.File]::AppendAllText($ex, "$leaf/`n", $Utf8) }
    Row 'INFO' 'git' "$leaf/ is in $ex (local only, nothing committed)"
  }
  try { foreach ($app in $list) { Install-One $app $from } } finally { Clean-Tmp }
  Write-InstalledJson
  Show-Table "install ($SveHome)"
  exit $script:Failed
}

# ---- wire: register the MCP servers (a standing change: only after the person's yes) ----
function Wire([string[]]$a) {
  if ($a.Count -lt 2 -or ($a[0] -ne 'claude' -and $a[0] -ne 'codex')) { Usage }
  $agent = $a[0]; $dry = ($a.Count -ge 3 -and $a[2] -eq '--dry-run')
  if (-not (Test-Path -LiteralPath $a[1] -PathType Container)) { [Console]::Error.WriteLine('SV Edits: wire needs the person''s project folder'); exit 2 }
  $dir = (Resolve-Path -LiteralPath $a[1]).ProviderPath.TrimEnd('\')
  $wide = @($env:USERPROFILE, (Join-Path $env:USERPROFILE 'Desktop'), (Join-Path $env:USERPROFILE 'Documents'), (Join-Path $env:USERPROFILE 'Downloads'),
    [Environment]::GetFolderPath('Desktop'), [Environment]::GetFolderPath('MyDocuments'))
  foreach ($od in @($env:OneDrive, $env:OneDriveConsumer, $env:OneDriveCommercial)) {
    if ($od) { $wide += @($od, (Join-Path $od 'Desktop'), (Join-Path $od 'Documents')) }
  }
  $wide = @($wide | Where-Object { $_ } | ForEach-Object { $_.TrimEnd('\') })
  if ($dir.Length -le 3 -or ($wide | Where-Object { $_ -ieq $dir })) { [Console]::Error.WriteLine("SV Edits: $dir is too wide. Root the photo server at one project folder."); exit 2 }
  # The agent's own program (.cmd or .exe), never a .ps1 shim, which could swallow the bare -- below.
  $agentExe = $agent
  if (-not $dry) {
    $cmd = Get-Command $agent -CommandType Application -ErrorAction SilentlyContinue | Select-Object -First 1
    if (-not $cmd) { Row 'FAIL' $agent "no $agent program (.cmd or .exe) on PATH"; Show-Table 'wire'; exit 1 }
    $agentExe = $cmd.Path
  }
  # Runs inside the project folder: Claude Code's local scope belongs to the folder it is run in.
  # The agent's own output is kept, so a failed add shows its reason in the table.
  $script:WireOut = ''
  function Run-Or-Print([string[]]$argv) {
    if ($dry) { Write-Host ("would run (in $dir): " + $agent + ' ' + ($argv -join ' ')); return $true }
    Push-Location -LiteralPath $dir
    try {
      $o = (& $agentExe @argv 2>&1 | Out-String)
      $script:WireOut = ($o -replace '\s+', ' ').Trim()
      if ($script:WireOut.Length -gt 300) { $script:WireOut = $script:WireOut.Substring(0, 300) }
      return ($LASTEXITCODE -eq 0)
    } finally { Pop-Location }
  }
  # Claude Code: add-json with local scope, so nothing depends on a bare -- reaching claude.
  # Codex has no add-json: it gets the -- form, through codex.cmd or codex.exe (untested on PowerShell 5.1).
  function Server-Args([string]$name, [string]$exe, [string[]]$sargs) {
    if ($agent -eq 'claude') {
      $json = @{ type = 'stdio'; command = $exe; args = $sargs } | ConvertTo-Json -Compress
      return @('mcp', 'add-json', '-s', 'local', $name, (Q $json))
    }
    return @('mcp', 'add', $name, '--', $exe) + $sargs
  }
  if ($agent -eq 'claude') { $rm = @('mcp', 'remove', '-s', 'local') } else { $rm = @('mcp', 'remove') }
  $pexe = (Exe-Of 'photocraft').Replace('\', '/'); $vexe = (Exe-Of 'vectorcraft').Replace('\', '/')
  if (Test-Path -LiteralPath $pexe) {
    [void](Run-Or-Print ($rm + @('sv-photo')))
    if (Run-Or-Print (Server-Args 'sv-photo' $pexe @('mcp', '--automation-read-root', $dir, '--automation-write-root', $dir))) { Row 'PASS' "sv-photo ($agent)" "reads and writes only inside $dir" }
    else { Row 'FAIL' "sv-photo ($agent)" "mcp add failed: $script:WireOut" }
  } else { Row 'INFO' 'sv-photo' 'photo tool not installed: not registered' }
  if (Test-Path -LiteralPath $vexe) {
    [void](Run-Or-Print ($rm + @('sv-vector')))
    if (Run-Or-Print (Server-Args 'sv-vector' $vexe @('mcp', '--headless'))) { Row 'PASS' "sv-vector ($agent)" 'headless, no window' }
    else { Row 'FAIL' "sv-vector ($agent)" "mcp add failed: $script:WireOut" }
  } else { Row 'INFO' 'sv-vector' 'vector tool not installed: not registered' }
  Row 'INFO' 'next session' "the servers' tools appear in the next $agent session; the CLI route works now"
  Show-Table "wire $agent"
  exit $script:Failed
}

# ---- run a pinned tool ----
function Read-Json([string]$p) {
  if (-not (Test-Path -LiteralPath $p)) { [Console]::Error.WriteLine("SV Edits: no such file: $p"); exit 2 }
  $c = [System.IO.File]::ReadAllText((Resolve-Path -LiteralPath $p).ProviderPath, [System.Text.Encoding]::UTF8)
  $t = $c.TrimStart([char]0xFEFF, ' ', "`t", "`r", "`n")
  if (-not ($t.StartsWith('{') -or $t.StartsWith('['))) { [Console]::Error.WriteLine("SV Edits: $p does not start with { or [, so it is not JSON"); exit 2 }
  return (Q $t.Trim())
}
function Run-Tool([string]$tool, [string[]]$a) {
  $app = $Apps[$tool]; Need-Tool $app $tool; $exe = Exe-Of $app
  if ($a | Where-Object { $_ -eq 'mcp' -or $_ -eq 'serve' }) { [Console]::Error.WriteLine("SV Edits: servers are registered only with the wire verb"); exit 2 }
  $out = New-Object System.Collections.ArrayList
  for ($i = 0; $i -lt $a.Count; $i++) {
    $x = $a[$i]
    if ($x -in @('--params-file', '--new-file', '--settings-file')) {
      if ($i + 1 -ge $a.Count) { Usage }
      [void]$out.Add($x.Substring(0, $x.Length - 5)); [void]$out.Add((Read-Json $a[$i + 1])); $i++
    } elseif ($x -eq '--json-file') {
      if ($i + 1 -ge $a.Count) { Usage }
      [void]$out.Add((Read-Json $a[$i + 1])); $i++
    } elseif ($x -in @('--in-place', '--bridge', '--connect', '--control-token', '--control-token-file', '--demo') -or $x -like '--bridge=*' -or $x -like '--connect=*') {
      [Console]::Error.WriteLine("SV Edits: $x is not allowed (originals never change, never an app window)"); exit 2
    } else { [void]$out.Add((Q $x)) }
  }
  $argList = [string[]]$out.ToArray()
  & $exe @argList
  exit $LASTEXITCODE
}

# ---- keep, check, name: the original never changes ----
function Read-Kept {
  $list = New-Object System.Collections.ArrayList
  if (Test-Path -LiteralPath $Kept) {
    foreach ($line in [System.IO.File]::ReadAllLines($Kept, [System.Text.Encoding]::UTF8)) {
      if ($line -match '^([0-9a-f]{64})  (.+)$') { [void]$list.Add(@($Matches[1], $Matches[2])) }
    }
  }
  return ,$list
}
function Is-Kept([string]$p) { foreach ($k in (Read-Kept)) { if ($k[1] -ieq $p) { return $true } }; return $false }
function Keep([string[]]$a) {
  if ($a.Count -lt 1) { Usage }
  New-Item -ItemType Directory -Force -Path $SveHome | Out-Null
  foreach ($f in $a) {
    if (-not (Test-Path -LiteralPath $f -PathType Leaf)) { Row 'FAIL' $f 'not found'; continue }
    $p = (Resolve-Path -LiteralPath $f).ProviderPath; $h = Sha $p
    $lines = @(foreach ($k in (Read-Kept)) { if ($k[1] -ine $p) { "$($k[0])  $($k[1])" } }) + "$h  $p"
    [System.IO.File]::WriteAllLines($Kept, [string[]]$lines, $Utf8)
    if ((Get-Item -LiteralPath $p).Length -gt 524288000) { Row 'WARN' (Split-Path -Leaf $p) 'kept; over about 500 MB: ask before opening it in the photo tool' }
    else { Row 'PASS' (Split-Path -Leaf $p) "kept, $(Kb $p) KB, sha256 $($h.Substring(0, 12))" }
  }
  Show-Table "keep (fingerprints in $Kept)"
  exit $script:Failed
}
function Check([string[]]$a) {
  $kept = Read-Kept
  if ($kept.Count -eq 0) { Row 'FAIL' 'kept.sha256' 'nothing kept yet: run keep on the originals first'; Show-Table 'check'; exit 1 }
  $want = @($a | ForEach-Object { Full $_ })
  foreach ($k in $kept) {
    if ($want.Count -gt 0 -and -not ($want | Where-Object { $_ -ieq $k[1] })) { continue }
    $name = Split-Path -Leaf $k[1]
    if (-not (Test-Path -LiteralPath $k[1])) { Row 'WARN' $name "no longer at $($k[1])" }
    elseif ((Sha $k[1]) -eq $k[0]) { Row 'PASS' $name 'unchanged' }
    else { Row 'FAIL' $name 'CHANGED since keep: stop and tell the person' }
  }
  foreach ($w in $want) { if (-not (Is-Kept $w)) { Row 'FAIL' $w 'was never kept: run keep before editing' } }
  Show-Table 'check (every original must say unchanged)'
  exit $script:Failed
}
function Variant([string]$want, [int]$k) {
  # k 1 is the path itself, k 2 adds -v2 before the extension, and so on.
  if ($k -eq 1) { return $want }
  $dir = Split-Path -Parent $want
  return (Join-Path $dir ([IO.Path]::GetFileNameWithoutExtension($want) + "-v$k" + [IO.Path]::GetExtension($want)))
}
function Name-Verb([string[]]$a) {
  # One free name per path, all with the same -vN, so a PSD and its JPEG keep matching names.
  if ($a.Count -lt 1) { Usage }
  $wants = @($a | ForEach-Object { Full $_ })
  foreach ($w in $wants) {
    $dir = Split-Path -Parent $w
    if (-not (Test-Path -LiteralPath $dir -PathType Container)) { [Console]::Error.WriteLine("SV Edits: no such folder: $dir"); exit 2 }
  }
  for ($k = 1; ; $k++) {
    $c = @($wants | ForEach-Object { Variant $_ $k })
    if (-not ($c | Where-Object { (Test-Path -LiteralPath $_) -or (Is-Kept $_) })) {
      # Forward slashes, so the paths can go straight into JSON (C:/Users/...).
      $c | ForEach-Object { $_.Replace('\', '/') }
      return
    }
  }
}

# ---- look: previews at most 1600 px wide, for the agent to open in the chat ----
function Save-Preview([string]$src, [string]$dst, [int]$maxW) {
  Add-Type -AssemblyName System.Drawing
  $img = [System.Drawing.Image]::FromFile($src)
  try {
    $w = $img.Width; $h = $img.Height
    if ($w -gt $maxW) { $nw = $maxW; $nh = [int][math]::Round($h * $maxW / $w) } else { $nw = $w; $nh = $h }
    $bmp = New-Object System.Drawing.Bitmap $nw, $nh
    $g = [System.Drawing.Graphics]::FromImage($bmp)
    $g.InterpolationMode = [System.Drawing.Drawing2D.InterpolationMode]::HighQualityBicubic
    $g.DrawImage($img, 0, 0, $nw, $nh); $g.Dispose()
    if ($dst.EndsWith('.png')) { $bmp.Save($dst, [System.Drawing.Imaging.ImageFormat]::Png) } else { $bmp.Save($dst, [System.Drawing.Imaging.ImageFormat]::Jpeg) }
    $bmp.Dispose()
    return @($w, $h)
  } finally { $img.Dispose() }
}
function Look-One([string]$f) {
  $p = (Resolve-Path -LiteralPath $f).ProviderPath; $name = Split-Path -Leaf $p
  $ext = [IO.Path]::GetExtension($p).TrimStart('.').ToLower()
  $stem = ([IO.Path]::GetFileNameWithoutExtension($p) -replace '[^A-Za-z0-9._-]', '-')
  $src = $p
  if ($ext -in @('psd', 'psb', 'pcraft')) {
    Need-Tool 'photocraft' 'photo'; $src = Join-Path $Prev ".$stem-$ext.flat.png"
    & (Exe-Of 'photocraft') convert $p $src *> $null
    if ($LASTEXITCODE -ne 0) { Row 'WARN' $name 'the photo tool could not render it'; return }
  } elseif ($ext -in @('ai', 'svg', 'svgz', 'pdf', 'eps', 'vectorcraft')) {
    Need-Tool 'vectorcraft' 'vector'; $src = Join-Path $Prev ".$stem-$ext.render.png"
    & (Exe-Of 'vectorcraft') convert $p $src *> $null
    if ($LASTEXITCODE -ne 0) { Row 'WARN' $name 'the vector tool could not render it'; return }
  } elseif ($ext -in @('mp4', 'mov', 'm4v', 'mxf', 'avi', 'mts', 'mkv', 'webm')) { Row 'INFO' $name 'a clip: use frames'; return }
  elseif ($ext -notin @('png', 'jpg', 'jpeg', 'tif', 'tiff', 'gif', 'bmp')) { Row 'INFO' $name 'not readable here: make a PNG or JPEG of it first'; return }
  $pext = if ($ext -in @('jpg', 'jpeg')) { 'jpg' } else { 'png' }
  $dst = Join-Path $Prev "$stem-$ext.preview.$pext"
  try { $wh = Save-Preview $src $dst 1600; Row 'PASS' $name "$($wh[0])x$($wh[1]) px, $(Kb $p) KB, preview: $dst" }
  catch { Row 'WARN' $name 'could not read it' }
  if ($src -ne $p) { Remove-Item -LiteralPath $src -Force -ErrorAction SilentlyContinue }
}
function Look([string[]]$a) {
  if ($a.Count -lt 1) { Usage }
  New-Item -ItemType Directory -Force -Path $Prev | Out-Null
  foreach ($f in $a) { if (Test-Path -LiteralPath $f -PathType Leaf) { Look-One $f } else { Row 'FAIL' $f 'not found' } }
  Show-Table 'look (open every preview in the chat before handing over)'
  exit $script:Failed
}

# ---- frames: n evenly spaced frames from a clip ----
function Frames([string[]]$a) {
  if ($a.Count -lt 1) { Usage }
  if (-not (Test-Path -LiteralPath $a[0] -PathType Leaf)) { [Console]::Error.WriteLine("SV Edits: no such clip: $($a[0])"); exit 2 }
  $clip = (Resolve-Path -LiteralPath $a[0]).ProviderPath
  $n = 6; if ($a.Count -ge 2) { $n = [int]$a[1] }
  Need-Tool 'filmcraft' 'film'; $fexe = Exe-Of 'filmcraft'
  $stem = ([IO.Path]::GetFileNameWithoutExtension($clip).ToLower() -replace '[^a-z0-9]', '-')
  $sha = [System.Security.Cryptography.SHA256]::Create()
  $tag6 = (($sha.ComputeHash($Utf8.GetBytes($clip)) | ForEach-Object { $_.ToString('x2') }) -join '').Substring(0, 6)
  $pdir = Join-Path $Proj "$stem-$tag6"
  New-Item -ItemType Directory -Force -Path $pdir, $Prev | Out-Null
  $probe = (& $fexe probe $clip 2>&1) -join "`n"
  if ($LASTEXITCODE -ne 0) { Row 'FAIL' 'probe' $probe; Show-Table 'frames'; exit 1 }
  try { $pj = $probe | ConvertFrom-Json } catch { Row 'FAIL' 'probe' 'not JSON'; Show-Table 'frames'; exit 1 }
  $secs = [double]$pj.duration / 254016000000
  Row 'PASS' 'probe' ("{0}x{1}, {2:N2} s" -f $pj.video.width, $pj.video.height, $secs)
  $tag = "$stem-frames"; $k = 2
  while (Test-Path -LiteralPath (Join-Path $Prev $tag)) { $tag = "$stem-frames-$k"; $k++ }
  $proj = Join-Path $pdir 'frames.fcproj'; $fdir = Join-Path $Prev $tag
  New-Item -ItemType Directory -Force -Path $fdir | Out-Null
  if (Test-Path -LiteralPath $proj) { Remove-Item -LiteralPath $proj -Force }
  Push-Location $pdir
  try {
    $imp = (& $fexe --compact --save-as $proj import $clip 2>&1) -join ''
    if ($imp -notmatch '"items":\[(\d+)') { Row 'FAIL' 'import' $imp; Show-Table 'frames'; exit 1 }
    $item = $Matches[1]
    & $fexe --compact --project $proj --save exec file.newSequence (Q ('{"name":"Frames","fromItem":' + $item + '}')) | Out-Null
    if ($LASTEXITCODE -ne 0) { Row 'FAIL' 'sequence' 'file.newSequence failed'; Show-Table 'frames'; exit 1 }
    for ($i = 1; $i -le $n; $i++) {
      $t = ($secs * ($i - 0.5) / $n).ToString('F2', [Globalization.CultureInfo]::InvariantCulture)
      $p = Join-Path $fdir "frame-$i-${t}s.png"
      & $fexe --project $proj render --seconds $t --out $p | Out-Null
      if ($LASTEXITCODE -eq 0 -and (Test-Path -LiteralPath $p)) { Row 'PASS' "frame $i at $t s" $p } else { Row 'FAIL' "frame $i at $t s" 'render failed' }
    }
  } finally { Pop-Location }
  $pexe = Exe-Of 'photocraft'
  if (Test-Path -LiteralPath $pexe) {
    $rows = [math]::Ceiling($n / 3); $sheet = Join-Path $Prev "$tag-contact-sheet.png"
    $params = '{"input":"' + $fdir.Replace('\', '\\') + '","units":"pixels","width":1800,"height":' + ($rows * 600) + ',"resolution":72,"columns":3,"rows":' + $rows + ',"caption":true,"flatten":true}'
    & $pexe run --new (Q '{"width":64,"height":64}') --cmd file.automate.contactSheetII --params (Q $params) --out $sheet | Out-Null
    if ($LASTEXITCODE -eq 0 -and (Test-Path -LiteralPath $sheet)) { Row 'PASS' 'contact sheet' $sheet } else { Row 'INFO' 'contact sheet' 'not made: open the frames one by one' }
  } else { Row 'INFO' 'contact sheet' 'needs the photo tool: open the frames one by one' }
  $fp = Join-Path $pdir 'FilmCraft Previews'
  if (Test-Path -LiteralPath $fp) {
    Get-ChildItem -LiteralPath $fp -Recurse -Directory | Sort-Object { $_.FullName.Length } -Descending | Where-Object { -not (Get-ChildItem -LiteralPath $_.FullName -Force) } | Remove-Item -Force
    if (-not (Get-ChildItem -LiteralPath $fp -Force)) { Remove-Item -LiteralPath $fp -Force }
  }
  Row 'INFO' 'project folder' "$pdir (every project for this clip goes here; the clip is only read)"
  Show-Table 'frames'
  exit $script:Failed
}

function Where-Verb {
  "Tools, helper, previews and clip projects: $SveHome"
  foreach ($app in 'photocraft', 'vectorcraft', 'filmcraft') {
    $d = Join-Path $SveHome "$app-$($Vers[$app])"
    if (Test-Path -LiteralPath $d) { "  $app $($Vers[$app]): $d" } else { "  ${app}: not installed" }
  }
  'Your files are never inside this folder, and removing it never touches them.'
  $ad = Join-Path $env:APPDATA 'Photocraft'
  if (Test-Path -LiteralPath $ad) { "  The photo tool also wrote settings in: $ad" }
  'To remove SV Edits (the agent runs these only when the person asks, after confirming once):'
  '  in the project folder: claude mcp remove -s local sv-photo; claude mcp remove -s local sv-vector   (or: codex mcp remove sv-photo; codex mcp remove sv-vector)'
  "  Remove-Item -Recurse -Force `"$SveHome`""
  if (Test-Path -LiteralPath $ad) { "  Remove-Item -Recurse -Force `"$ad`"" }
  Row 'PASS' 'where' $SveHome
  Show-Table 'where'
}

if ($Argv.Count -eq 0) { Usage }
$verb = $Argv[0]
$rest = [string[]]@($Argv | Select-Object -Skip 1)
switch ($verb) {
  'doctor' { Doctor }
  'install' { Install-Verb $rest }
  'wire' { Wire $rest }
  { $_ -in @('photo', 'vector', 'film') } { Run-Tool $verb $rest }
  'keep' { Keep $rest }
  'check' { Check $rest }
  'name' { Name-Verb $rest }
  'look' { Look $rest }
  'frames' { Frames $rest }
  'where' { Where-Verb }
  default { Usage }
}
```

### ~/.sv-edits/blender/make-scenes-blender.py (SV Blender script)

SV Academy's own, MIT. Written out on demand by "SV Blender" (Write the script), which holds its sha256 pin. It is code for Blender to run: there is no need to read it.

```python
# sv-blender v1: make-scenes-blender.py, SV Academy's own script, MIT. Tested with Blender 5.1.2 (macOS, Metal).
"""
SV Blender: the scene maker inside SV Edits.

Blender (free, open source) builds and renders a real-world scene, then records
the exact four corners of every "design face" (a billboard, a sign, a screen,
a bag front). PhotoCraft then places the design there as a swappable smart
object, with Blender's own light multiplied on top.

Everything runs headless. Blender never opens a window:

  BLENDER=/Applications/Blender.app/Contents/MacOS/Blender
  $BLENDER -b --factory-startup --python make-scenes-blender.py -- --list
  $BLENDER -b --factory-startup --python make-scenes-blender.py -- \\
      --scene skytrain-billboard --out ../scenes --captures ../example/sv/launch/blender

  --scene        a preset name, a comma list, or "all"
  --out          folder for <scene>/ outputs (default: ../scenes next to tools/)
  --captures     folder for the step-by-step assembly captures (optional)
  --samples      override Cycles samples for the final plate
  --quick        fast preview quality (low samples, half resolution)
  --preview      only render the shot camera once (fast look-dev), nothing else
  --no-captures  skip the 01..07 assembly captures
  --design PNG   also render preview-with-design.png with the image mapped on
                 every design face (a ground-truth check for the PhotoCraft composite)
  --parts DIR    logo-exploded-3d: folder of named part SVGs
  --card PNG / --banner PNG   print-still-life: the print files used as textures

Your own .blend (the file is only read; a copy with the SV passes is saved in --out):
  $BLENDER -b --factory-startup --python make-scenes-blender.py -- --blend room.blend --objects
      lists every mesh the scene camera sees, largest first, with its size and whether it is flat
  $BLENDER -b --factory-startup --python make-scenes-blender.py -- --blend room.blend --face Poster \\
      --out scenes [--camera Camera] [--kind emissive] [--samples 128] [--probe]
      the named object(s) become design faces: plate, light, mask and corners.json as for a preset.
      A face is a flat object: a plane, or a thin box (its camera-side face is used).

Outputs per scene, in <out>/<scene>/:
  <scene>.blend   the scene; open it in Blender yourself if you like (optional)
  plate.png       the final render, design faces in neutral mid grey
  light.png       ONLY the design faces in pure white, carrying the scene's light
                  and shading (RGBA, transparent elsewhere): use it as a Multiply layer
  mask.png        design faces white on black (mask-<face>.png per face when >1)
  corners.json    per face: four corners in image pixels (TL, TR, BR, BL),
                  computed with bpy_extras.object_utils.world_to_camera_view from
                  the face's real vertices, plus size in meters and aspect ratio
  stages/         clean, unlabelled stage renders (blockout, materials, light, camera)

The camera finish (bloom, a touch of barrel distortion, chromatic aberration,
vignette and film grain) is applied to plate.png. The same distortion is applied
to light.png and mask.png and to the corner coordinates, so all four line up.

Second mode (plain python3 with Pillow, no Blender): lays out the captures.
  python3 make-scenes-blender.py annotate --scene-dir scenes/<scene> --captures <dir>
The Blender run calls this automatically at the end when --captures is given.

A new scene is a function that builds geometry with the helpers below and calls
design_face(...) for every surface that will carry a design. Add it to PRESETS.
"""
import json
import math
import zlib
import os
import random
import subprocess
import sys
import time

HERE = os.path.dirname(os.path.abspath(__file__))
DEFAULT_OUT = os.path.normpath(os.path.join(HERE, "..", "scenes"))
LAUNCH = os.path.normpath(os.path.join(HERE, "..", "example", "sv", "launch"))

try:
    import bpy  # noqa: F401
    IN_BLENDER = True
except ImportError:
    IN_BLENDER = False

# Copy for the captures (EN plus a short Thai line). No em or en dashes.
SCENE_TITLES = {
    "skytrain-billboard": ("Skytrain billboard, blue hour", "บิลบอร์ดใต้รางรถไฟฟ้า ช่วงฟ้าเริ่มมืด"),
    "shopfront-sign": ("Shophouse lightbox and window poster", "ป้ายไฟหน้าร้านตึกแถว กับโปสเตอร์ติดกระจก"),
    "mall-led": ("Mall atrium LED screen", "จอ LED กลางโถงห้าง"),
    "tote-and-box": ("Woven tote patch and kraft takeaway box", "ถุงผ้าแขวนผนัง กับกล่องอาหารกระดาษคราฟท์"),
    "logo-exploded-3d": ("The logo, taken apart", "โลโก้ แยกออกทีละชิ้น"),
    "print-still-life": ("Business cards on the proof table", "นามบัตรบนโต๊ะตรวจงานพิมพ์"),
}
STEPS = [
    ("01", "Blockout", "Shapes at real size, in meters", "ขึ้นรูปทรงตามขนาดจริง หน่วยเป็นเมตร"),
    ("02", "Materials", "Concrete, glass, metal, fabric, all written as code", "ใส่วัสดุ คอนกรีต กระจก โลหะ ผ้า เขียนด้วยโค้ดทั้งหมด"),
    ("03", "Light", "Real sky model, lamps switched on", "เปิดท้องฟ้าจำลองและไฟในฉาก"),
    ("04", "Camera", "Frame the shot, mark the design face", "ตั้งกล้อง จัดเฟรม กำหนดพื้นที่วางงาน"),
    ("05", "Render", "Cycles on the GPU, design face left grey", "เรนเดอร์ด้วย Cycles เว้นพื้นที่งานเป็นสีเทา"),
    ("06", "Passes", "Plate, light and mask for PhotoCraft", "แยกภาพฉาก แสง และมาสก์ ส่งให้ PhotoCraft"),
    ("07", "Corners", "Four corners in pixels, read from the 3D face", "พิกัดสี่มุมเป็นพิกเซล อ่านจากหน้าจริงในฉาก 3D"),
]

FONT_SIGN = ["/System/Library/Fonts/Supplemental/Krungthep.ttf",
             os.path.expanduser("~/Library/Fonts/Prompt-Black.ttf")]
FONT_SIGN_TH = [os.path.join(os.path.dirname(os.path.abspath(__file__)), "..", "example", "sv", "fonts", "Sarabun-Bold.ttf"),
                "/System/Library/Fonts/Supplemental/Krungthep.ttf"]
FONT_SIGN2 = ["/System/Library/Fonts/Supplemental/Silom.ttf",
              "/System/Library/Fonts/Supplemental/Krungthep.ttf"]

# =============================================================================
# Blender side
# =============================================================================
if IN_BLENDER:
    import bmesh
    import numpy as np
    from bpy_extras.object_utils import world_to_camera_view
    from mathutils import Matrix, Vector

    C = {}            # current build state
    FACES = []        # registered design faces
    W, H = 1920, 1080
    ACCENT = (0x22 / 255, 0xC5 / 255, 0x5E / 255)
    PROXY_ORANGE = (1.0, 0.42, 0.05)

    # ---------------------------------------------------------------- colour
    def lin(c):
        """sRGB 0..1 tuple (or hex string) to linear RGB."""
        if isinstance(c, str):
            c = c.lstrip("#")
            c = tuple(int(c[i:i + 2], 16) / 255 for i in (0, 2, 4))
        out = []
        for v in c[:3]:
            out.append(v / 12.92 if v <= 0.04045 else ((v + 0.055) / 1.055) ** 2.4)
        return tuple(out)

    def kelvin(t):
        """Colour temperature to linear RGB (Tanner Helland fit), normalised."""
        t = t / 100.0
        r = 255 if t <= 66 else 329.698727446 * ((t - 60) ** -0.1332047592)
        g = 99.4708025861 * math.log(t) - 161.1195681661 if t <= 66 else 288.1221695283 * ((t - 60) ** -0.0755148492)
        b = 255 if t >= 66 else (0 if t <= 19 else 138.5177312231 * math.log(t - 10) - 305.0447927307)
        c = [max(0, min(255, x)) / 255 for x in (r, g, b)]
        lc = lin(c)
        m = max(lc)
        return tuple(x / m for x in lc)

    def rgba(c):
        return (c[0], c[1], c[2], 1.0)

    def L(c):
        return lin(c) if isinstance(c, str) else tuple(c)

    # ---------------------------------------------------------------- nodes
    class G:
        """Tiny node graph helper for shader trees."""

        def __init__(self, nt):
            self.nt = nt

        def n(self, typ, ins=None, **props):
            node = self.nt.nodes.new(typ)
            for k, v in props.items():
                setattr(node, k, v)
            for k, v in (ins or {}).items():
                self.set(self.sock(node.inputs, k), v)
            return node

        @staticmethod
        def sock(coll_, key):
            if isinstance(key, int):
                return coll_[key]
            for s in coll_:
                if s.identifier == key:
                    return s
            return coll_[key]

        def set(self, sock, v):
            if isinstance(v, bpy.types.NodeSocket):
                self.nt.links.new(v, sock)
            elif isinstance(v, bpy.types.Node):
                self.nt.links.new(v.outputs[0], sock)
            else:
                if isinstance(v, (tuple, list)) and len(v) == 3 and hasattr(sock, "default_value") \
                        and hasattr(sock.default_value, "__len__") and len(sock.default_value) == 4:
                    v = rgba(v)
                sock.default_value = v

        def math(self, op, a, b=0.0, clamp=False):
            node = self.n("ShaderNodeMath", operation=op, use_clamp=clamp)
            self.set(node.inputs[0], a)
            self.set(node.inputs[1], b)
            return node.outputs[0]

        def mix(self, fac, a, b, blend="MIX"):
            node = self.n("ShaderNodeMix", data_type="RGBA", blend_type=blend)
            self.set(self.sock(node.inputs, "Factor_Float"), fac)
            self.set(self.sock(node.inputs, "A_Color"), a)
            self.set(self.sock(node.inputs, "B_Color"), b)
            return self.sock(node.outputs, "Result_Color")

        def coords(self, kind="Object", scale=(1, 1, 1), loc=(0, 0, 0)):
            tc = self.n("ShaderNodeTexCoord")
            mp = self.n("ShaderNodeMapping")
            self.nt.links.new(tc.outputs[kind], mp.inputs["Vector"])
            mp.inputs["Scale"].default_value = scale
            mp.inputs["Location"].default_value = loc
            return mp.outputs[0]

        def noise(self, vec, scale, detail=6.0, rough=0.55, out="Fac"):
            node = self.n("ShaderNodeTexNoise", {"Vector": vec, "Scale": scale, "Detail": detail, "Roughness": rough})
            return node.outputs[out]

        def maprange(self, v, a=0.0, b=1.0, c=0.0, d=1.0, clamp=True):
            node = self.n("ShaderNodeMapRange", {"Value": v, "From Min": a, "From Max": b, "To Min": c, "To Max": d})
            node.clamp = clamp
            return node.outputs[0]

        def ramp(self, fac, a, b, lo=0.3, hi=0.7):
            return self.mix(self.maprange(fac, lo, hi), a, b)

        def bump(self, height, strength=0.2, distance=0.02, normal=None):
            ins = {"Height": height, "Strength": strength, "Distance": distance}
            if normal is not None:
                ins["Normal"] = normal
            return self.n("ShaderNodeBump", ins).outputs["Normal"]

        def grey(self, v):
            cc = self.n("ShaderNodeCombineColor")
            for i in range(3):
                self.set(cc.inputs[i], v)
            return cc.outputs[0]

    def new_mat(name):
        m = bpy.data.materials.new(name)
        try:
            m.use_nodes = True
        except Exception:
            pass
        nt = m.node_tree
        for node in list(nt.nodes):
            nt.nodes.remove(node)
        g = G(nt)
        out = g.n("ShaderNodeOutputMaterial")
        return m, g, out

    def principled(g, base, rough=0.5, metal=0.0, spec=0.5, normal=None, coat=0.0, coat_rough=0.05,
                   emit=None, emit_strength=0.0, transmission=0.0, ior=1.45, alpha=1.0, aniso=0.0,
                   sheen=0.0, subsurface=0.0):
        ins = {"Base Color": base, "Roughness": rough, "Metallic": metal, "Specular IOR Level": spec}
        if normal is not None:
            ins["Normal"] = normal
            if coat:
                ins["Coat Normal"] = normal if False else normal
        if coat:
            ins["Coat Weight"] = coat
            ins["Coat Roughness"] = coat_rough
        if emit is not None:
            ins["Emission Color"] = emit
            ins["Emission Strength"] = emit_strength
        if transmission:
            ins["Transmission Weight"] = transmission
            ins["IOR"] = ior
        if alpha < 1:
            ins["Alpha"] = alpha
        if aniso:
            ins["Anisotropic"] = aniso
        if sheen:
            ins["Sheen Weight"] = sheen
        if subsurface:
            ins["Subsurface Weight"] = subsurface
        return g.n("ShaderNodeBsdfPrincipled", ins)

    def finish(m, g, out, shader, view=None, volume=None):
        if shader is not None:
            g.set(out.inputs["Surface"], shader)
        if volume is not None:
            g.set(out.inputs["Volume"], volume)
        if view is not None:
            m.diffuse_color = rgba(view)
        return m

    MATS = {}

    def cached(fn):
        def wrap(name, *a, **k):
            if name in MATS:
                return MATS[name]
            MATS[name] = fn(name, *a, **k)
            return MATS[name]
        return wrap

    @cached
    def mat_plain(name, color, rough=0.5, metal=0.0, spec=0.5, coat=0.0):
        m, g, out = new_mat(name)
        c = L(color)
        return finish(m, g, out, principled(g, c, rough, metal, spec, coat=coat), c)

    @cached
    def mat_surface(name, c1, c2, scale=2.0, rough=(0.7, 0.9), bump=0.15, metal=0.0,
                    stretch=(1, 1, 1), streaks=0.0, fine=40.0, spec=0.5, coat=0.0, coords="Object",
                    grime=0.0, patches=0.0):
        """Weathered surface: two-colour noise mix, roughness variation, fine bump,
        optional vertical water streaks and grime toward the ground. Concrete,
        plaster, stone, kraft, wood, fabric all come from this."""
        m, g, out = new_mat(name)
        a, b = L(c1), L(c2)
        vec = g.coords(coords, stretch)
        base_noise = g.noise(vec, scale, 8.0, 0.6)
        col = g.ramp(base_noise, a, b, 0.35, 0.68)
        dark = 1.0
        if streaks:
            st = g.noise(g.coords(coords, (2.5, 2.5, 0.08)), 2.2, 5.0, 0.55)
            mr = g.maprange(st, 0.42, 0.78, 1.0, 1.0 - streaks)
            col = g.mix(1.0, col, g.grey(mr), "MULTIPLY")
        if patches:
            pn = g.noise(g.coords(coords, (1, 1, 1)), 0.35, 3.0, 0.5)
            pm = g.maprange(pn, 0.48, 0.52, 1.0 - patches, 1.0 + patches * 0.4)
            col = g.mix(1.0, col, g.grey(pm), "MULTIPLY")
        if grime:
            tc = g.n("ShaderNodeTexCoord")
            sep = g.n("ShaderNodeSeparateXYZ")
            g.set(sep.inputs[0], tc.outputs["Object"])
            gz = g.maprange(sep.outputs[2], 0.0, 1.2, 1.0 - grime, 1.0)
            col = g.mix(1.0, col, g.grey(gz), "MULTIPLY")
        rr = g.maprange(base_noise, 0.2, 0.8, rough[0], rough[1])
        nrm = None
        if bump:
            fine_n = g.noise(g.coords(coords, stretch), fine, 10.0, 0.7)
            nrm = g.bump(fine_n, bump, 0.01)
        sh = principled(g, col, rr, metal, spec, normal=nrm, coat=coat)
        return finish(m, g, out, sh, a)

    @cached
    def mat_wet_asphalt(name, c1="#2a2a2b", c2="#161617", puddle=0.45, puddle_scale=0.16, wet=0.5):
        """Asphalt after rain: aggregate texture, damp low-gloss surface and
        mirror-like puddles where the noise dips (reflections sell a night street)."""
        m, g, out = new_mat(name)
        vec = g.coords("Object")
        agg = g.noise(vec, 60.0, 12.0, 0.75)
        big = g.noise(vec, 1.2, 6.0, 0.6)
        col = g.ramp(big, L(c1), L(c2), 0.3, 0.7)
        col = g.mix(g.maprange(agg, 0.4, 0.75, 0.0, 0.35), col, (0.08, 0.08, 0.08))
        pn = g.noise(g.coords("Object", (1, 1.6, 1)), puddle_scale, 8.0, 0.62)
        p = g.maprange(pn, puddle - 0.02, puddle + 0.03, 1.0, 0.0)          # 1 in a puddle
        rough_dry = g.maprange(agg, 0.3, 0.7, 0.35 * (1.6 - wet), 0.75 * (1.4 - wet))
        rough = g.mix(p, g.grey(rough_dry), (0.035, 0.035, 0.035))
        rr = g.n("ShaderNodeSeparateColor")
        g.set(rr.inputs[0], rough)
        col = g.mix(p, col, g.mix(1.0, col, (0.55, 0.55, 0.55), "MULTIPLY"))
        bm = g.bump(agg, 0.35, 0.006)
        ripple = g.bump(g.noise(vec, 7.0, 3.0, 0.5), 0.03, 0.002)
        nrm = g.n("ShaderNodeMix", data_type="VECTOR")
        g.set(g.sock(nrm.inputs, "Factor_Float"), p)
        g.set(g.sock(nrm.inputs, "A_Vector"), bm)
        g.set(g.sock(nrm.inputs, "B_Vector"), ripple)
        sh = principled(g, col, rr.outputs[0], 0.0, 0.6, normal=g.sock(nrm.outputs, "Result_Vector"))
        return finish(m, g, out, sh, L(c1))

    @cached
    def mat_wet_road(name, c1="#3a3a3a", c2="#262627", puddle=0.47, puddle_scale=0.2, edge=0.012, dip=0.004):
        """Asphalt after rain, built the way the street really is: the whole road is damp, its roughness driven by
        one noise (features about 0.4 m) between 0.05 and 0.35, so the lights smear in patches; the puddles are a
        separate mask with a soft edge (a ColorRamp, a few centimetres wide, not a hard step), sit a few millimetres
        low, and keep the asphalt's own colour darkened by 30 percent: the reflections lie on visible ground."""
        m, g, out = new_mat(name)
        vec = g.coords("Object")
        agg = g.noise(vec, 60.0, 12.0, 0.75)
        big = g.noise(vec, 1.2, 6.0, 0.6)
        col = g.ramp(big, L(c1), L(c2), 0.3, 0.7)
        col = g.mix(g.maprange(agg, 0.4, 0.75, 0.0, 0.35), col, (0.07, 0.07, 0.07))
        # damp layer: one noise at 0.4 m drives roughness 0.05 to 0.35
        wetn = g.noise(vec, 2.5, 3.0, 0.5)
        r_damp = g.maprange(wetn, 0.2, 0.8, 0.05, 0.35)
        # puddles: low-frequency noise through a ColorRamp with a soft edge
        pn = g.noise(g.coords("Object", (1, 1.6, 1)), puddle_scale, 2.5, 0.45)
        cr = g.n("ShaderNodeValToRGB")
        g.set(cr.inputs["Fac"], pn)
        el = cr.color_ramp.elements
        el[0].position, el[0].color = puddle - edge, (1, 1, 1, 1)
        el[1].position, el[1].color = puddle + edge, (0, 0, 0, 1)
        cr.color_ramp.interpolation = "EASE"
        p = cr.outputs["Color"]
        pr = g.n("ShaderNodeSeparateColor")
        g.set(pr.inputs[0], p)
        p = pr.outputs[0]
        rough = g.mix(p, g.grey(r_damp), (0.045, 0.045, 0.045))
        rr = g.n("ShaderNodeSeparateColor")
        g.set(rr.inputs[0], rough)
        col = g.mix(p, col, g.mix(1.0, col, (0.7, 0.7, 0.7), "MULTIPLY"))
        # height: aggregate texture on the dry road, filled flat by water in the puddle, which sits a little low
        h_dry = g.math("MULTIPLY", agg, g.math("SUBTRACT", 1.0, p))
        h = g.math("SUBTRACT", g.math("MULTIPLY", h_dry, 0.0025), g.math("MULTIPLY", p, dip))
        bm = g.n("ShaderNodeBump", {"Height": h, "Strength": 1.0, "Distance": 1.0}).outputs["Normal"]
        ripple = g.bump(g.math("MULTIPLY", g.noise(vec, 9.0, 3.0, 0.5), g.math("ADD", 0.4, p)), 0.04, 0.002, normal=bm)
        spec_ = g.maprange(p, 0.0, 1.0, 0.55, 0.32)       # water over asphalt reflects at IOR 1.33, not 1.5
        sh = principled(g, col, rr.outputs[0], 0.0, spec_, normal=ripple)
        return finish(m, g, out, sh, L(c1))

    @cached
    def mat_street_wet(name, c1="#383839", c2="#2c2c2d", puddles=(), noise_puddle=0.44, puddle_scale=0.2, edge=0.05):
        """Asphalt after rain for the skytrain street (10 Oct): a dark base (albedo about 0.04), damp roughness 0.25 to
        0.45 broken up by two noises, a fine aggregate bump, and puddles as soft-masked glossy zones (roughness 0.02,
        the asphalt's own colour, no holes). Placed puddles are ellipses in world metres (x, y, rx, ry) set where the
        lens sees the board and the shop signs mirrored; a few small noise puddles break up the rest."""
        m, g, out = new_mat(name)
        vec = g.coords("Object")
        agg = g.noise(vec, 70.0, 12.0, 0.75)
        big = g.noise(vec, 0.9, 6.0, 0.6)
        col = g.ramp(big, L(c1), L(c2), 0.3, 0.7)
        col = g.mix(g.maprange(agg, 0.45, 0.8, 0.0, 0.25), col, L("#4a4a4b"))
        # damp: roughness 0.25 to 0.45 from a mid noise, nudged by a slow one so it never repeats
        wet1 = g.noise(vec, 1.6, 4.0, 0.55)
        wet2 = g.noise(vec, 0.25, 2.0, 0.5)
        r_damp = g.maprange(g.math("ADD", g.math("MULTIPLY", wet1, 0.7), g.math("MULTIPLY", wet2, 0.3)), 0.3, 0.7, 0.25, 0.45)
        # puddles: noise ones, small and few
        pn = g.noise(g.coords("Object", (1, 1.6, 1)), puddle_scale, 2.5, 0.45)
        p = g.maprange(pn, noise_puddle - edge * 0.4, noise_puddle + edge * 0.4, 1.0, 0.0)
        # placed ones: soft ellipses with a wobbly rim
        tc = g.n("ShaderNodeTexCoord")
        sep = g.n("ShaderNodeSeparateXYZ")
        g.set(sep.inputs[0], tc.outputs["Object"])
        wob = g.noise(vec, 0.9, 3.0, 0.5)
        for (px, py, rx, ry) in puddles:
            dx = g.math("DIVIDE", g.math("SUBTRACT", sep.outputs[0], px), rx)
            dy = g.math("DIVIDE", g.math("SUBTRACT", sep.outputs[1], py), ry)
            d2 = g.math("ADD", g.math("MULTIPLY", dx, dx), g.math("MULTIPLY", dy, dy))
            d2 = g.math("ADD", d2, g.math("MULTIPLY", g.math("SUBTRACT", wob, 0.5), 0.9))
            e = g.maprange(d2, 1.0 - edge * 4, 1.0 + edge * 4, 1.0, 0.0)
            p = g.math("MAXIMUM", p, e)
        rough = g.maprange(p, 0.0, 1.0, 0.0, 1.0)
        rmix = g.n("ShaderNodeMix", data_type="FLOAT")
        g.set(g.sock(rmix.inputs, "Factor_Float"), p)
        g.set(g.sock(rmix.inputs, "A_Float"), r_damp)
        g.set(g.sock(rmix.inputs, "B_Float"), 0.02)
        # water over asphalt: the same colour a touch deeper, so the reflections lie on visible ground
        col = g.mix(p, col, g.mix(1.0, col, (0.85, 0.85, 0.85), "MULTIPLY"))
        h_dry = g.math("MULTIPLY", agg, g.math("SUBTRACT", 1.0, p))
        h = g.math("SUBTRACT", g.math("MULTIPLY", h_dry, 0.0022), g.math("MULTIPLY", p, 0.003))
        bm = g.n("ShaderNodeBump", {"Height": h, "Strength": 1.0, "Distance": 1.0}).outputs["Normal"]
        ripple = g.bump(g.math("MULTIPLY", g.noise(vec, 11.0, 3.0, 0.5), g.math("ADD", 0.25, p)), 0.025, 0.0015, normal=bm)
        spec_ = g.maprange(p, 0.0, 1.0, 0.5, 0.32)
        sh = principled(g, col, g.sock(rmix.outputs, "Result_Float"), 0.0, spec_, normal=ripple)
        return finish(m, g, out, sh, L(c1))

    @cached
    def mat_brushed_brass(name, color="#c4ad82"):
        """Brushed brass, not yellow plastic: a desaturated brass, roughness 0.25, anisotropy 0.6 with the tangent
        along the part's length (local X), and fine brushing lines in the bump."""
        m, g, out = new_mat(name)
        c = L(color)
        vt = g.n("ShaderNodeVectorTransform", vector_type="VECTOR", convert_from="OBJECT", convert_to="WORLD")
        vt.inputs[0].default_value = (1.0, 0.0, 0.0)
        lines = g.noise(g.coords("Object", (0.02, 1.0, 1.0)), 2200.0, 2.0, 0.5)
        nrm = g.bump(lines, 0.08, 0.0002)
        rr = g.maprange(g.noise(g.coords("Object", (0.05, 1, 1)), 300.0, 3.0, 0.5), 0.3, 0.7, 0.21, 0.29)
        sh = principled(g, c, rr, 1.0, normal=nrm, aniso=0.6)
        g.set(g.sock(sh.inputs, "Tangent"), vt.outputs[0])
        return finish(m, g, out, sh, c)

    @cached
    def mat_travertine(name):
        """Honed travertine: a warm cream with soft horizontal bedding bands and real pitting (Voronoi cells pressed
        into the surface, darker inside), honed to a low sheen."""
        m, g, out = new_mat(name)
        vec = g.coords("Object")
        band = g.noise(g.coords("Object", (1.0, 1.0, 9.0)), 2.2, 6.0, 0.6)
        col = g.ramp(band, lin("#e8dcc4"), lin("#d4c2a1"), 0.3, 0.75)
        vor = g.n("ShaderNodeTexVoronoi", {"Vector": g.coords("Object", (1.0, 1.0, 2.2)), "Scale": 34.0})
        vor2 = g.n("ShaderNodeTexVoronoi", {"Vector": vec, "Scale": 120.0})
        pit = g.math("MAXIMUM", g.maprange(vor.outputs["Distance"], 0.16, 0.07, 0.0, 1.0),
                     g.math("MULTIPLY", g.maprange(vor2.outputs["Distance"], 0.14, 0.06, 0.0, 1.0), 0.7))
        col = g.mix(g.math("MULTIPLY", pit, 0.55), col, lin("#9b8768"))
        h = g.math("SUBTRACT", g.math("MULTIPLY", g.noise(vec, 60.0, 6.0, 0.6), 0.2), pit)
        nrm = g.bump(h, 0.6, 0.0015)
        rr = g.mix(pit, g.grey(g.maprange(band, 0.3, 0.7, 0.32, 0.45)), (0.85, 0.85, 0.85))
        rs = g.n("ShaderNodeSeparateColor")
        g.set(rs.inputs[0], rr)
        return finish(m, g, out, principled(g, col, rs.outputs[0], 0.0, 0.5, normal=nrm), lin("#e2d5bc"))

    @cached
    def mat_kraft(name, color="#a57f56"):
        """Kraft board: an even brown (no blotches), a fibre bump about 0.3 mm, roughness 0.7 to 0.85."""
        m, g, out = new_mat(name)
        vec = g.coords("Object")
        fib = g.noise(g.coords("Object", (1.0, 5.0, 1.0)), 900.0, 4.0, 0.6)
        fib2 = g.noise(g.coords("Object", (5.0, 1.0, 1.0)), 1400.0, 3.0, 0.6)
        col = g.mix(1.0, L(color), g.grey(g.maprange(fib, 0.3, 0.7, 0.96, 1.04)), "MULTIPLY")
        h = g.math("ADD", fib, g.math("MULTIPLY", fib2, 0.6))
        nrm = g.bump(h, 0.5, 0.0003)
        rr = g.maprange(g.noise(vec, 80.0, 4.0, 0.5), 0.3, 0.7, 0.7, 0.85)
        return finish(m, g, out, principled(g, col, rr, 0.0, 0.45, normal=nrm), L(color))

    @cached
    def mat_tiles(name, c1, c2, mortar, size=0.4, mortar_size=0.006, rough=0.5, bump=0.3, coat=0.0, offset=0.5,
                  wet=0.0, row=None):
        m, g, out = new_mat(name)
        vec = g.coords("Object")
        br = g.n("ShaderNodeTexBrick", {"Vector": vec, "Color1": L(c1), "Color2": L(c2),
                                        "Mortar": L(mortar), "Scale": 1.0, "Mortar Size": mortar_size,
                                        "Bias": 0.0, "Brick Width": size, "Row Height": row or size,
                                        "Mortar Smooth": 0.1}, offset=offset)
        nz = g.noise(vec, 3.0, 6.0, 0.6)
        col = g.mix(1.0, br.outputs["Color"], g.grey(g.maprange(nz, 0, 1, 0.86, 1.06)), "MULTIPLY")
        inv = g.math("SUBTRACT", 1.0, br.outputs["Fac"])
        nrm = g.bump(inv, bump, 0.004)
        r0 = rough
        if wet:
            pn = g.noise(g.coords("Object", (1, 1, 1)), 0.5, 6.0, 0.6)
            r0 = g.maprange(pn, 0.4, 0.6, rough * (1 - wet), rough)
        rr = g.n("ShaderNodeMapRange", {"Value": br.outputs["Fac"], "To Min": r0, "To Max": 0.9})
        sh = principled(g, col, rr.outputs[0], normal=nrm, coat=coat)
        return finish(m, g, out, sh, L(c1))

    @cached
    def mat_emit(name, color, strength, temp=None, noise=0.0):
        m, g, out = new_mat(name)
        c = kelvin(temp) if temp else L(color)
        s = strength
        if noise:
            s = g.maprange(g.noise(g.coords("Object"), 3.0, 3.0, 0.5), 0.3, 0.7, strength * (1 - noise), strength)
        em = g.n("ShaderNodeEmission", {"Color": c, "Strength": s})
        return finish(m, g, out, em, c)

    @cached
    def mat_lightbox(name, color, strength, rough=0.25):
        """Backlit acrylic: emission with a soft hot spot plus a glossy skin."""
        m, g, out = new_mat(name)
        c = L(color)
        sh = principled(g, c, rough, 0.0, 0.6, emit=c, emit_strength=strength, coat=0.4)
        return finish(m, g, out, sh, c)

    @cached
    def mat_glass(name, tint=(0.9, 0.95, 0.95), reflect=0.06, rough=0.02):
        """Architectural glass: transparent plus a Fresnel reflection, no refraction."""
        m, g, out = new_mat(name)
        tr = g.n("ShaderNodeBsdfTransparent", {"Color": tint})
        gl = g.n("ShaderNodeBsdfGlossy", {"Color": (1, 1, 1), "Roughness": rough})
        lw = g.n("ShaderNodeLayerWeight", {"Blend": 0.18})
        fac = g.math("MAXIMUM", lw.outputs["Fresnel"], reflect)
        # light passes architectural glass: shadow rays see it as fully transparent (lamps outside light the room)
        lp = g.n("ShaderNodeLightPath")
        fac = g.math("MULTIPLY", fac, g.math("SUBTRACT", 1.0, lp.outputs["Is Shadow Ray"]))
        mx = g.n("ShaderNodeMixShader")
        g.set(mx.inputs[0], fac)
        g.set(mx.inputs[1], tr)
        g.set(mx.inputs[2], gl)
        m.diffuse_color = (0.6, 0.7, 0.75, 0.3)
        return finish(m, g, out, mx.outputs[0])

    @cached
    def mat_window(name, glass=(0.02, 0.025, 0.03), lit=None, strength=1.5):
        """Reflective dark glass, optionally with a warm room glow behind it."""
        m, g, out = new_mat(name)
        if lit is not None:
            k = g.maprange(g.noise(g.coords("Object"), 1.2, 3.0, 0.5), 0.3, 0.7, 0.45 * strength, strength)
            sh = principled(g, glass, 0.04, 0.0, 0.8, emit=lit, emit_strength=k)
        else:
            sh = principled(g, glass, 0.05, 0.0, 0.9)
        return finish(m, g, out, sh, glass)

    @cached
    def mat_train_window(name, strength=1.3):
        """A lit carriage seen through tinted glass: brighter toward the ceiling lights."""
        m, g, out = new_mat(name)
        tc = g.n("ShaderNodeTexCoord")
        sep = g.n("ShaderNodeSeparateXYZ")
        g.set(sep.inputs[0], tc.outputs["Generated"])
        grad = g.maprange(sep.outputs[2], 0.0, 1.0, 0.35, 1.0)
        # what you see through the glass: a bright ceiling strip, standing riders as soft
        # dark shapes, vertical grab poles and darker seat backs along the bottom, so the
        # windows stop reading as flat white cards at a 100 percent crop
        riders = g.noise(g.coords("Object", (1.0, 1.0, 0.35)), 1.6, 3.0, 0.5)
        rider = g.maprange(riders, 0.48, 0.56, 1.0, 0.18)
        mid = g.maprange(sep.outputs[2], 0.12, 0.3, 0.0, 1.0)
        top = g.maprange(sep.outputs[2], 0.78, 0.9, 1.0, 0.0)
        rider = g.maprange(g.math("MULTIPLY", g.math("SUBTRACT", 1.0, rider), g.math("MULTIPLY", mid, top)),
                           0.0, 1.0, 1.0, 0.0)
        wv = g.n("ShaderNodeTexWave", {"Vector": g.coords("Object"), "Scale": 0.21, "Distortion": 0.0})
        wv.wave_type = "BANDS"
        wv.bands_direction = "X"
        pole = g.maprange(wv.outputs["Fac"], 0.0, 0.008, 0.25, 1.0)
        seat = g.maprange(sep.outputs[2], 0.18, 0.26, 0.32, 1.0)
        ceil = g.maprange(sep.outputs[2], 0.86, 0.92, 1.0, 2.4)
        k = g.math("MULTIPLY", g.math("MULTIPLY", rider, pole), g.math("MULTIPLY", seat, ceil))
        grad = g.math("MULTIPLY", grad, k)
        sh = principled(g, (0.02, 0.025, 0.03), 0.05, 0.0, 0.8, emit=kelvin(5200),
                        emit_strength=g.math("MULTIPLY", grad, strength))
        return finish(m, g, out, sh, (0.6, 0.7, 0.75))

    @cached
    def mat_facade(name, wall, bay=3.0, floor_h=3.3, win_w=2.2, win_h=1.9, lit_ratio=0.35,
                   lit_temp=3200, lit_strength=2.5, cool_ratio=0.3, glass=(0.025, 0.03, 0.04), seed=0.0,
                   band=False, axis="XY"):
        """Building facade by code: a window grid on any axis-aligned box. Each
        window gets a random state (dark, warm lit, cool lit) from White Noise,
        with blinds (vertical brightness falloff) on some."""
        m, g, out = new_mat(name)
        tc = g.n("ShaderNodeTexCoord")
        sep = g.n("ShaderNodeSeparateXYZ")
        g.set(sep.inputs[0], tc.outputs["Object"])
        u = g.math("ADD", sep.outputs[0], sep.outputs[1])
        z = sep.outputs[2]
        cu = g.math("DIVIDE", u, bay)
        cz = g.math("DIVIDE", z, floor_h)
        fu = g.math("FRACT", cu)
        fz = g.math("FRACT", cz)
        iu = g.math("FLOOR", cu)
        iz = g.math("FLOOR", cz)
        a0 = (1 - win_w / bay) / 2
        a1 = 1 - a0
        if band:
            a0, a1 = -1, 2
        b0 = 0.16
        b1 = b0 + win_h / floor_h
        mu = g.math("MULTIPLY", g.math("GREATER_THAN", fu, a0), g.math("LESS_THAN", fu, a1))
        mz = g.math("MULTIPLY", g.math("GREATER_THAN", fz, b0), g.math("LESS_THAN", fz, b1))
        win = g.math("MULTIPLY", mu, mz)
        cx = g.n("ShaderNodeCombineXYZ")
        g.set(cx.inputs[0], iu)
        g.set(cx.inputs[1], iz)
        g.set(cx.inputs[2], seed)
        wn = g.n("ShaderNodeTexWhiteNoise", {"Vector": cx.outputs[0]})
        wn.noise_dimensions = "3D"
        r = wn.outputs["Value"]
        lit_warm = g.math("LESS_THAN", r, lit_ratio)
        lit_cool = g.math("MULTIPLY", g.math("GREATER_THAN", r, lit_ratio),
                          g.math("LESS_THAN", r, lit_ratio + cool_ratio))
        emc = g.mix(lit_cool, kelvin(lit_temp), kelvin(5200))
        wn2 = g.n("ShaderNodeTexWhiteNoise", {"Vector": cx.outputs[0], "W": 3.7})
        wn2.noise_dimensions = "4D"
        br = g.maprange(wn2.outputs["Value"], 0, 1, 0.25, 1.0)
        # blinds: brighter at the bottom of some windows
        blind = g.maprange(fz, b0, b1, 1.0, 0.35)
        on = g.math("MULTIPLY", g.math("ADD", lit_warm, lit_cool), win)
        strength = g.math("MULTIPLY", g.math("MULTIPLY", g.math("MULTIPLY", on, br), blind), lit_strength)
        wl = L(wall)
        nz = g.noise(g.coords("Object", (1, 1, 1)), 0.6, 6.0, 0.6)
        wallc = g.ramp(nz, tuple(x * 0.75 for x in wl), wl, 0.3, 0.7)
        col = g.mix(win, wallc, glass)
        rough = g.maprange(win, 0, 1, 0.85, 0.06)
        sh = principled(g, col, rough, 0.0, 0.5, emit=emc, emit_strength=strength)
        return finish(m, g, out, sh, wl)

    @cached
    def mat_metal(name, color, rough=0.3, brushed=False, axis_scale=(1, 1, 60), aniso=0.0):
        m, g, out = new_mat(name)
        c = L(color)
        nrm = None
        rr = rough
        if brushed:
            nz = g.noise(g.coords("Object", axis_scale), 40, 4, 0.5)
            nrm = g.bump(nz, 0.04, 0.002)
            rr = g.maprange(g.noise(g.coords("Object"), 6, 4, 0.5), 0.3, 0.7, rough * 0.8, rough * 1.25)
        else:
            rr = g.maprange(g.noise(g.coords("Object"), 4, 6, 0.6), 0.3, 0.7, rough * 0.7, rough * 1.3)
        return finish(m, g, out, principled(g, c, rr, 1.0, normal=nrm, aniso=aniso), c)

    @cached
    def mat_paint(name, color, rough=0.35, coat=0.8, flakes=False):
        """Car paint or powder coat: base colour under a clear coat."""
        m, g, out = new_mat(name)
        c = L(color)
        return finish(m, g, out, principled(g, c, rough, 0.3 if flakes else 0.0, 0.5, coat=coat, coat_rough=0.03), c)

    @cached
    def mat_corrugated(name, color, pitch=0.06, rough=0.45, axis="Z", rust=0.0, metal=0.6):
        m, g, out = new_mat(name)
        c = L(color)
        wv = g.n("ShaderNodeTexWave", {"Vector": g.coords("Object"), "Scale": 1.0 / pitch / 6.0},
                 wave_type="BANDS", bands_direction=axis)
        nrm = g.bump(wv.outputs["Fac"], 0.6, 0.01)
        nz = g.noise(g.coords("Object", (1, 1, 0.2)), 3, 5, 0.6)
        col = g.ramp(nz, tuple(x * 0.6 for x in c), c, 0.35, 0.7)
        if rust:
            rn = g.noise(g.coords("Object"), 2.0, 8, 0.7)
            col = g.mix(g.maprange(rn, 0.55, 0.7, 0, rust), col, lin("#6b3d22"))
        return finish(m, g, out, principled(g, col, rough, metal, normal=nrm), c)

    @cached
    def mat_pack(name, color, kind="box"):
        """Retail packaging by code: a base colour, a printed label band with lines of 'type', a logo blob and a
        darker cap or lid; roughness differs between the board and the varnished label. Used for shop stock, so a
        shelf reads as goods, not as coloured cubes."""
        m, g, out = new_mat(name)
        c = L(color)
        tc = g.n("ShaderNodeTexCoord")
        sep = g.n("ShaderNodeSeparateXYZ")
        g.set(sep.inputs[0], tc.outputs["Generated"])
        z = sep.outputs[2]
        u = g.math("ADD", sep.outputs[0], sep.outputs[1])
        lo, hi = (0.30, 0.72) if kind == "box" else (0.22, 0.62)
        band = g.math("MULTIPLY", g.math("GREATER_THAN", z, lo), g.math("LESS_THAN", z, hi))
        label = lin("#f3eee2") if sum(c) < 1.2 else tuple(x * 0.25 for x in c)
        col = g.mix(band, c, label)
        wv = g.n("ShaderNodeTexWave", {"Vector": tc.outputs["Generated"], "Scale": 9.0}, wave_type="BANDS", bands_direction="Z")
        lines = g.math("MULTIPLY", g.math("GREATER_THAN", wv.outputs["Fac"], 0.78),
                       g.math("MULTIPLY", g.math("GREATER_THAN", z, lo + 0.04), g.math("LESS_THAN", z, lo + 0.2)))
        ink = tuple(x * 0.35 for x in c) if sum(c) > 1.2 else lin("#2a2522")
        col = g.mix(g.math("MULTIPLY", lines, 0.85), col, ink)
        blob = g.noise(g.coords("Generated", (1, 1, 1)), 2.2, 2.0, 0.5)
        logo = g.math("MULTIPLY", g.math("GREATER_THAN", blob, 0.58),
                      g.math("MULTIPLY", g.math("GREATER_THAN", z, hi - 0.17), g.math("LESS_THAN", z, hi - 0.03)))
        col = g.mix(logo, col, c)
        cap = g.math("GREATER_THAN", z, 0.88 if kind != "box" else 1.1)
        col = g.mix(cap, col, tuple(x * 0.45 for x in c))
        rough = g.maprange(band, 0, 1, 0.65 if kind == "box" else 0.3, 0.25)
        return finish(m, g, out, principled(g, col, rough, 0.0, 0.5), c)

    @cached
    def mat_haze(name, density=0.004, color=(0.8, 0.85, 0.95), aniso=0.55):
        m, g, out = new_mat(name)
        vol = g.n("ShaderNodeVolumePrincipled", {"Color": color, "Density": density, "Anisotropy": aniso})
        return finish(m, g, out, None, volume=vol)

    # ---------------------------------------------------------------- geometry
    def coll(name):
        c = bpy.data.collections.get(name)
        if c is None:
            c = bpy.data.collections.new(name)
            bpy.context.scene.collection.children.link(c)
        return c

    def link(ob, collection=None):
        (collection or C["coll"]).objects.link(ob)
        ob.color = (0.78, 0.78, 0.78, 1.0)
        return ob

    def mesh_obj(name, verts, faces, mat=None, collection=None, smooth=False, edges=()):
        me = bpy.data.meshes.new(name)
        me.from_pydata([tuple(v) for v in verts], list(edges), [tuple(f) for f in faces])
        me.update()
        if smooth:
            for p in me.polygons:
                p.use_smooth = True
        ob = bpy.data.objects.new(name, me)
        link(ob, collection)
        if mat is not None:
            for mm in (mat if isinstance(mat, (list, tuple)) else [mat]):
                me.materials.append(mm)
        return ob

    def add_bevel(ob, width, segments=2):
        md = ob.modifiers.new("bevel", "BEVEL")
        md.width = width
        md.segments = segments
        md.limit_method = "ANGLE"
        try:
            md.harden_normals = False
        except Exception:
            pass
        return md

    def box(name, x0, x1, y0, y1, z0, z1, mat=None, bevel=0.0, collection=None, parent=None):
        x0, x1 = min(x0, x1), max(x0, x1)
        y0, y1 = min(y0, y1), max(y0, y1)
        z0, z1 = min(z0, z1), max(z0, z1)
        v = [(x0, y0, z0), (x1, y0, z0), (x1, y1, z0), (x0, y1, z0),
             (x0, y0, z1), (x1, y0, z1), (x1, y1, z1), (x0, y1, z1)]
        f = [(0, 3, 2, 1), (4, 5, 6, 7), (0, 1, 5, 4), (1, 2, 6, 5), (2, 3, 7, 6), (3, 0, 4, 7)]
        ob = mesh_obj(name, v, f, mat, collection)
        if bevel:
            add_bevel(ob, bevel)
        if parent is not None:
            ob.parent = parent
        return ob

    def boxc(name, center, size, mat=None, rot_z=0.0, bevel=0.0, collection=None, rot=None, parent=None):
        sx, sy, sz = size
        ob = box(name, -sx / 2, sx / 2, -sy / 2, sy / 2, -sz / 2, sz / 2, mat, bevel, collection, parent)
        ob.location = center
        ob.rotation_euler = rot if rot is not None else (0, 0, math.radians(rot_z))
        return ob

    def cyl(name, r, h, center, mat=None, segs=24, r2=None, rot=(0, 0, 0), collection=None, smooth=True, parent=None):
        me = bpy.data.meshes.new(name)
        bm = bmesh.new()
        bmesh.ops.create_cone(bm, cap_ends=True, cap_tris=False, segments=segs, radius1=r,
                              radius2=r if r2 is None else r2, depth=h)
        bm.to_mesh(me)
        bm.free()
        if smooth:
            for p in me.polygons:
                p.use_smooth = len(p.vertices) == 4
        ob = bpy.data.objects.new(name, me)
        link(ob, collection)
        if mat is not None:
            me.materials.append(mat)
        ob.location = center
        ob.rotation_euler = rot
        if parent is not None:
            ob.parent = parent
        return ob

    def sphere(name, r, center, mat=None, segs=24, collection=None, scale=(1, 1, 1), parent=None):
        me = bpy.data.meshes.new(name)
        bm = bmesh.new()
        bmesh.ops.create_uvsphere(bm, u_segments=segs, v_segments=max(6, segs // 2), radius=r)
        bm.to_mesh(me)
        bm.free()
        for p in me.polygons:
            p.use_smooth = True
        ob = bpy.data.objects.new(name, me)
        link(ob, collection)
        if mat is not None:
            me.materials.append(mat)
        ob.location = center
        ob.scale = scale
        if parent is not None:
            ob.parent = parent
        return ob

    def beam(name, p0, p1, r, mat=None, segs=8, collection=None, parent=None):
        p0, p1 = Vector(p0), Vector(p1)
        d = p1 - p0
        ob = cyl(name, r, d.length, (p0 + p1) / 2, mat, segs, collection=collection, parent=parent)
        ob.rotation_mode = "QUATERNION"
        ob.rotation_quaternion = d.to_track_quat("Z", "Y")
        return ob

    def recalc(ob):
        me = ob.data
        bm = bmesh.new()
        bm.from_mesh(me)
        bmesh.ops.recalc_face_normals(bm, faces=bm.faces)
        bm.to_mesh(me)
        bm.free()

    def prism_x(name, profile_yz, x0, x1, mat=None, collection=None, parent=None, bevel=0.0):
        """Extrude a closed (y, z) profile along X: decks, parapets, girders."""
        n = len(profile_yz)
        v = [(x0, y, z) for y, z in profile_yz] + [(x1, y, z) for y, z in profile_yz]
        f = [tuple(reversed(range(n))), tuple(range(n, 2 * n))]
        for i in range(n):
            j = (i + 1) % n
            f.append((i, j, n + j, n + i))
        ob = mesh_obj(name, v, f, mat, collection)
        recalc(ob)
        if bevel:
            add_bevel(ob, bevel)
        if parent is not None:
            ob.parent = parent
        return ob

    def prism_y(name, profile_xz, y0, y1, mat=None, collection=None, parent=None, bevel=0.0):
        ob = prism_x(name, list(profile_xz), y0, y1, mat, collection, None, 0.0)
        for vtx in ob.data.vertices:
            x, y, z = vtx.co
            vtx.co = (y, x, z)
        recalc(ob)
        if bevel:
            add_bevel(ob, bevel)
        if parent is not None:
            ob.parent = parent
        return ob

    def cable(name, p0, p1, sag, r=0.012, mat=None, n=18, collection=None, parent=None):
        cu = bpy.data.curves.new(name, "CURVE")
        cu.dimensions = "3D"
        cu.bevel_depth = r
        cu.bevel_resolution = 2
        sp = cu.splines.new("POLY")
        sp.points.add(n - 1)
        p0, p1 = Vector(p0), Vector(p1)
        for i in range(n):
            t = i / (n - 1)
            p = p0.lerp(p1, t)
            p.z -= sag * 4 * t * (1 - t)
            sp.points[i].co = (p.x, p.y, p.z, 1)
        ob = bpy.data.objects.new(name, cu)
        link(ob, collection)
        if mat is not None:
            cu.materials.append(mat)
        if parent is not None:
            ob.parent = parent
        return ob

    def ribbon(name, pts, half_w, thick, mat=None, collection=None):
        cu = bpy.data.curves.new(name, "CURVE")
        cu.dimensions = "3D"
        cu.extrude = half_w
        cu.bevel_depth = thick
        cu.bevel_resolution = 1
        sp = cu.splines.new("POLY")
        sp.points.add(len(pts) - 1)
        for i, p in enumerate(pts):
            sp.points[i].co = (p[0], p[1], p[2], 1)
        ob = bpy.data.objects.new(name, cu)
        link(ob, collection)
        if mat is not None:
            cu.materials.append(mat)
        return ob

    def group(name, loc=(0, 0, 0), rot_z=0.0, parent=None):
        e = bpy.data.objects.new(name, None)
        link(e)
        e.location = loc
        e.rotation_euler = (0, 0, math.radians(rot_z))
        if parent is not None:
            e.parent = parent
        return e

    FONTS = {}

    def load_font(cands):
        for p in cands:
            if p in FONTS:
                return FONTS[p]
            if os.path.exists(p):
                try:
                    FONTS[p] = bpy.data.fonts.load(p, check_existing=True)
                    return FONTS[p]
                except Exception:
                    continue
        return None

    def text3d(name, body, size, loc, rot=(90, 0, 0), mat=None, extrude=0.0, font=None, align="CENTER",
               parent=None, valign="CENTER"):
        """Real type as geometry (sign lettering). Rotation in degrees; default stands
        the text up facing -Y."""
        f = font or load_font(FONT_SIGN)
        if f is None:
            return None
        cu = bpy.data.curves.new(name, "FONT")
        cu.body = body
        cu.font = f
        cu.size = size
        cu.extrude = extrude
        cu.align_x = align
        try:
            cu.align_y = valign
        except Exception:
            pass
        ob = bpy.data.objects.new(name, cu)
        link(ob)
        if mat is not None:
            cu.materials.append(mat)
        ob.location = loc
        ob.rotation_euler = tuple(math.radians(a) for a in rot)
        if parent is not None:
            ob.parent = parent
        return ob

    # ---------------------------------------------------------------- lights
    def light(name, kind, loc, energy, color=(1, 1, 1), target=None, size=1.0, size_y=None,
              spot_deg=60, blend=0.3, temp=None, shadow_soft=0.05, collection=None, parent=None,
              spread=None):
        ld = bpy.data.lights.new(name, kind)
        ld.energy = energy
        ld.color = kelvin(temp) if temp else color
        if kind == "AREA":
            ld.shape = "RECTANGLE"
            ld.size = size
            ld.size_y = size if size_y is None else size_y
            if spread is not None:
                ld.spread = math.radians(spread)
        elif kind == "SPOT":
            ld.spot_size = math.radians(spot_deg)
            ld.spot_blend = blend
            ld.shadow_soft_size = shadow_soft
        elif kind == "POINT":
            ld.shadow_soft_size = shadow_soft
        ob = bpy.data.objects.new(name, ld)
        link(ob, collection or coll("SV_lights"))
        ob.location = loc
        if target is not None:
            d = Vector(target) - Vector(loc)
            ob.rotation_euler = d.to_track_quat("-Z", "Y").to_euler()
        if parent is not None:
            ob.parent = parent
        return ob

    # ---------------------------------------------------------------- world
    def world_sky(elev_deg, rot_deg, strength=1.0, sun_disc=False, air=1.0, aerosol=1.0, ozone=1.0,
                  altitude=0.0, kind="MULTIPLE_SCATTERING", tint=None):
        sc = bpy.context.scene
        w = bpy.data.worlds.new("SV_sky")
        sc.world = w
        try:
            w.use_nodes = True
        except Exception:
            pass
        nt = w.node_tree
        for node in list(nt.nodes):
            nt.nodes.remove(node)
        g = G(nt)
        sky = nt.nodes.new("ShaderNodeTexSky")
        chosen = None
        for t in (kind, "MULTIPLE_SCATTERING", "NISHITA", "SINGLE_SCATTERING", "HOSEK_WILKIE"):
            try:
                sky.sky_type = t
                chosen = t
                break
            except TypeError:
                continue
        sky.sun_elevation = math.radians(elev_deg)
        sky.sun_rotation = math.radians(rot_deg)
        for attr, val in (("sun_disc", sun_disc), ("air_density", air), ("aerosol_density", aerosol),
                          ("dust_density", aerosol), ("ozone_density", ozone), ("altitude", altitude)):
            if hasattr(sky, attr):
                try:
                    setattr(sky, attr, val)
                except Exception:
                    pass
        col = sky.outputs[0]
        if tint is not None:
            col = g.mix(1.0, col, tint, "MULTIPLY")
        bg = g.n("ShaderNodeBackground", {"Color": col, "Strength": strength})
        out = g.n("ShaderNodeOutputWorld")
        g.set(out.inputs["Surface"], bg)
        C["sky_model"] = chosen
        C["world_bg"] = bg
        return w

    def world_color(color, strength=1.0, name="SV_world"):
        sc = bpy.context.scene
        w = bpy.data.worlds.new(name)
        sc.world = w
        try:
            w.use_nodes = True
        except Exception:
            pass
        nt = w.node_tree
        for node in list(nt.nodes):
            nt.nodes.remove(node)
        bg = nt.nodes.new("ShaderNodeBackground")
        bg.inputs[0].default_value = rgba(color)
        bg.inputs[1].default_value = strength
        out = nt.nodes.new("ShaderNodeOutputWorld")
        nt.links.new(bg.outputs[0], out.inputs[0])
        C["world_bg"] = bg
        C["sky_model"] = "studio colour"
        return w

    def haze(density, color=(0.8, 0.85, 0.95), center=(0, 0, 0), size=(400, 400, 80), aniso=0.55):
        """Light volumetric haze: one big box of thin scattering medium."""
        ob = boxc("SV_haze", center, size, mat_haze("haze", density, color, aniso), collection=coll("SV_atmosphere"))
        ob["sv_mask_ignore"] = True
        ob.visible_shadow = False
        ob.display_type = "BOUNDS"
        ob.hide_viewport = False
        return ob

    # ---------------------------------------------------------------- camera
    def camera(name, loc, target, lens=35.0, shift_x=0.0, shift_y=0.0, roll=0.0, fstop=None, focus=None,
               level=False):
        """level=True keeps the camera's horizon flat and verticals vertical
        (architectural): it looks horizontally toward the target and uses lens
        shift to frame up or down."""
        cd = bpy.data.cameras.new(name)
        cd.lens = lens
        cd.sensor_width = 36.0
        cd.sensor_fit = "HORIZONTAL"
        cd.shift_x = shift_x
        cd.shift_y = shift_y
        cd.clip_start = 0.02
        cd.clip_end = 3000
        if fstop:
            cd.dof.use_dof = True
            cd.dof.aperture_fstop = fstop
            cd.dof.focus_distance = focus if focus else (Vector(target) - Vector(loc)).length
        ob = bpy.data.objects.new(name, cd)
        link(ob, coll("SV_cameras"))
        ob.location = loc
        d = Vector(target) - Vector(loc)
        if level:
            yaw = math.atan2(d.x, d.y)
            pitch = math.degrees(math.atan2(d.z, math.hypot(d.x, d.y))) if level == "aim" else 0.0
            ob.rotation_euler = (math.radians(90 + pitch), math.radians(roll), -yaw)
        else:
            ob.rotation_euler = d.to_track_quat("-Z", "Y").to_euler()
            if roll:
                ob.rotation_euler.rotate_axis("Z", math.radians(roll))
        return ob

    # ---------------------------------------------------------------- design faces
    FACE_SURFACES = {
        # surface: (rough, bump strength, bump scale)
        "paper": (0.55, 0.02, 300.0),
        "card": (0.45, 0.015, 400.0),
        "vinyl": (0.38, 0.03, 120.0),
        "canvas": (0.95, 0.30, 0.0),
        "kraft": (0.85, 0.10, 180.0),
        "lightbox": (0.25, 0.0, 0.0),
        "led": (0.2, 0.0, 0.0),
        "fabric": (0.9, 0.08, 200.0),
        "woven": (0.6, 1.0, 0.0),     # a woven label: fine warp and weft, a sheen on the threads
    }

    def design_face(name, center, width, height, yaw=0.0, tilt=0.0, kind="printed", surface="paper",
                    label=None, bulge=0.0, subdiv=1, parent=None, roll=0.0, emit_strength=6.0, round_r=0.0, **extra):
        """The generic SV design face: a rectangle that will carry a design.

        center          world (or parent-space) centre in meters
        width, height   real size in meters (the aspect ratio comes from these)
        yaw             degrees around Z; at 0 the front faces -Y (a viewer standing at -Y)
        tilt            degrees leaning back around the face's own horizontal axis (90 = lying flat, face up)
        kind            "printed" (lit by the scene) or "emissive" (screen or lightbox)
        surface         paper, card, vinyl, canvas, kraft, fabric, lightbox, led
        bulge           meters of pillow bulge toward the viewer at the centre (fabric)
        Vertex order 0..3 is top-left, top-right, bottom-right, bottom-left,
        stored on the object so the corners survive subdivision."""
        w2, h2 = width / 2, height / 2
        n = max(1, subdiv)
        verts, faces = [], []
        if round_r:
            # a rounded rectangle (one n-gon): the mask follows the real outline, while the four corners stored for
            # PhotoCraft stay the corners of the bounding rectangle, so the art maps 1:1 onto the face
            r_ = min(round_r, w2, h2)
            pts = []
            for (cx_, cz_, a0) in ((w2 - r_, h2 - r_, 0), (-w2 + r_, h2 - r_, 90), (-w2 + r_, -h2 + r_, 180),
                                   (w2 - r_, -h2 + r_, 270)):
                for k in range(17):
                    a = math.radians(a0 + 90 * k / 16)
                    pts.append((cx_ + r_ * math.cos(a), 0.0, cz_ + r_ * math.sin(a)))
            ob = mesh_obj("DESIGN_" + name, pts, [tuple(range(len(pts)))], None, coll("SV_design_faces"))   # CCW seen from -Y: normal toward the viewer
            uv = ob.data.uv_layers.new(name="UVMap")
            for poly in ob.data.polygons:
                for li in poly.loop_indices:
                    x, _, z = pts[ob.data.loops[li].vertex_index]
                    uv.data[li].uv = ((x + w2) / width, (z + h2) / height)
            if parent is not None:
                ob.parent = parent
            ob.location = center
            ob.rotation_euler = (math.radians(-tilt), math.radians(roll), math.radians(yaw))
            ob["sv_corners_local"] = [[-w2, 0, h2], [w2, 0, h2], [w2, 0, -h2], [-w2, 0, -h2]]
            ob["sv_kind"] = kind
            ob["sv_surface"] = surface
            ob["sv_size_m"] = [width, height]
            ob["sv_round_r"] = r_
            ob.color = rgba(ACCENT)
            FACES.append({"name": name, "obj": ob, "kind": kind, "surface": surface, "emit": emit_strength,
                          "label": label or name.replace("-", " "), "size": (width, height), "round_r": r_})
            return ob
        for j in range(n + 1):           # rows from top to bottom
            v = j / n
            for i in range(n + 1):       # columns from left to right
                u = i / n
                x = -w2 + u * width
                z = h2 - v * height
                y = -bulge * (math.sin(math.pi * u) ** 0.8) * (math.sin(math.pi * v) ** 0.8) if bulge else 0.0
                verts.append((x, y, z))
        for j in range(n):
            for i in range(n):
                a = j * (n + 1) + i
                b, c, d = a + 1, a + n + 2, a + n + 1
                faces.append((d, c, b, a))   # normal toward -Y
        ob = mesh_obj("DESIGN_" + name, verts, faces, None, coll("SV_design_faces"))
        uv = ob.data.uv_layers.new(name="UVMap")
        for poly in ob.data.polygons:
            for li in poly.loop_indices:
                vi = ob.data.loops[li].vertex_index
                x, _, z = verts[vi]
                uv.data[li].uv = ((x + w2) / width, (z + h2) / height)
        if parent is not None:
            ob.parent = parent
        ob.location = center
        ob.rotation_euler = (math.radians(-tilt), math.radians(roll), math.radians(yaw))
        ob["sv_corners_local"] = [list(verts[0]), list(verts[n]), list(verts[-1]), list(verts[-(n + 1)])]
        ob["sv_kind"] = kind
        ob["sv_surface"] = surface
        ob["sv_size_m"] = [width, height]
        ob.color = rgba(ACCENT)
        FACES.append({"name": name, "obj": ob, "kind": kind, "surface": surface, "emit": emit_strength,
                      "label": label or name.replace("-", " "), "size": (width, height), **extra})
        return ob

    def face_material(face, mode, design_image=None):
        """mode: plate (neutral mid grey), light (pure white), design (preview with an image)."""
        rough, bstr, bscale = FACE_SURFACES.get(face["surface"], (0.5, 0.0, 0.0))
        m, g, out = new_mat("FACE_%s_%s" % (face["name"], mode))
        nrm = None
        if bstr and face["surface"] in ("canvas", "woven"):
            vec = g.coords("UV", (1, 1, 1))
            sz = face["size"]
            k_ = 180 if face["surface"] == "canvas" else 300      # a woven label's weave, about 3 mm: it reads at 100 percent
            wx = g.n("ShaderNodeTexWave", {"Vector": vec, "Scale": sz[0] * k_}, wave_type="BANDS", bands_direction="X")
            wy = g.n("ShaderNodeTexWave", {"Vector": vec, "Scale": sz[1] * k_}, wave_type="BANDS", bands_direction="Y")
            h = g.math("MULTIPLY", wx.outputs["Fac"], wy.outputs["Fac"])
            nz = g.noise(g.coords("Object"), 25, 6, 0.6)
            h = g.math("ADD", h, g.math("MULTIPLY", nz, 0.6))
            nrm = g.bump(h, bstr, 0.002)
        elif bstr:
            nz = g.noise(g.coords("Object"), bscale, 8, 0.7)
            nrm = g.bump(nz, bstr, 0.002)
        tex = None
        if mode == "design" and design_image:
            img = bpy.data.images.load(design_image, check_existing=True)
            tex = g.n("ShaderNodeTexImage")
            tex.image = img
            uvv = g.n("ShaderNodeTexCoord").outputs["UV"]
            bf = C.get("design_bleed", 0.0)
            if bf:
                # the art carries a bleed on every edge: map only its trim onto the face
                mp = g.n("ShaderNodeMapping")
                g.set(mp.inputs["Vector"], uvv)
                mp.inputs["Scale"].default_value = (1 - 2 * bf, 1 - 2 * bf, 1)
                mp.inputs["Location"].default_value = (bf, bf, 0)
                uvv = mp.outputs[0]
            g.set(tex.inputs["Vector"], uvv)
        if face["kind"] == "emissive":
            uvn = g.n("ShaderNodeTexCoord")
            sep = g.n("ShaderNodeSeparateXYZ")
            g.set(sep.inputs[0], uvn.outputs["UV"])
            du = g.math("ABSOLUTE", g.math("SUBTRACT", sep.outputs[0], 0.5))
            dv = g.math("ABSOLUTE", g.math("SUBTRACT", sep.outputs[1], 0.5))
            edge = g.math("MAXIMUM", g.math("MULTIPLY", du, 2), g.math("MULTIPLY", dv, 2))
            fall = 0.3 if face["surface"] == "lightbox" else 0.08
            f = g.math("SUBTRACT", 1.0, g.math("MULTIPLY", g.math("POWER", edge, 4.0), fall))
            if face["surface"] == "led":
                # the screen is built from cabinets (500 x 500 mm, a faint seam between them) and its LEDs sit on a
                # pitch: a fine dark grid that only shows at a 100 percent crop. In the plate and the light pass alike,
                # so the Multiply carries it onto any design
                sz = face["size"]
                pitch = face.get("pitch", 0.06)
                gx = g.math("MULTIPLY", sep.outputs[0], sz[0] / pitch)
                gy = g.math("MULTIPLY", sep.outputs[1], sz[1] / pitch)
                lx = g.math("POWER", g.math("ABSOLUTE", g.math("SINE", g.math("MULTIPLY", gx, math.pi))), 0.35)
                ly = g.math("POWER", g.math("ABSOLUTE", g.math("SINE", g.math("MULTIPLY", gy, math.pi))), 0.35)
                px_ = g.maprange(g.math("MULTIPLY", lx, ly), 0.0, 1.0, 0.72, 1.0)
                cx_ = g.math("FRACT", g.math("MULTIPLY", sep.outputs[0], sz[0] / 0.5))
                cy_ = g.math("FRACT", g.math("MULTIPLY", sep.outputs[1], sz[1] / 0.5))
                seam = g.math("MULTIPLY", g.maprange(g.math("MINIMUM", cx_, g.math("SUBTRACT", 1.0, cx_)), 0.0, 0.03, 0.72, 1.0),
                              g.maprange(g.math("MINIMUM", cy_, g.math("SUBTRACT", 1.0, cy_)), 0.0, 0.03, 0.72, 1.0))
                f = g.math("MULTIPLY", f, g.math("MULTIPLY", px_, seam))
            strength = face["emit"]
            if mode == "light":
                # an LED's light pass sits just under clipping, so its pixel grid and cabinet seams survive the Multiply
                s = g.math("MULTIPLY", f, face.get("light_emit", strength))
                em = g.n("ShaderNodeEmission", {"Color": (1, 1, 1), "Strength": s})
            elif tex is not None:
                em = g.n("ShaderNodeEmission", {"Color": tex.outputs["Color"], "Strength": g.math("MULTIPLY", f, strength)})
            else:
                em = g.n("ShaderNodeEmission", {"Color": (1, 1, 1), "Strength": g.math("MULTIPLY", f, strength * face.get("plate_k", 0.21))})
            # a glossy skin on top: screens and acrylic reflect the room
            gl = g.n("ShaderNodeBsdfGlossy", {"Color": (1, 1, 1), "Roughness": rough})
            lw = g.n("ShaderNodeLayerWeight", {"Blend": 0.12})
            mx = g.n("ShaderNodeMixShader")
            g.set(mx.inputs[0], lw.outputs["Fresnel"])
            g.set(mx.inputs[1], em)
            g.set(mx.inputs[2], gl)
            sh = mx if mode != "light" else em
        else:
            if mode == "light":
                sh = principled(g, (1, 1, 1), max(rough, 0.6), 0.0, 0.25, normal=nrm)
            elif tex is not None:
                sh = principled(g, tex.outputs["Color"], rough, 0.0, 0.4, normal=nrm,
                                sheen=0.8 if face["surface"] == "woven" else 0.0)
            else:
                # woven thread catches a sheen: it stays in the plate, so PhotoCraft's Reflections layer carries it
                sh = principled(g, lin((0.5, 0.5, 0.5)), rough, 0.0, 0.6 if face["surface"] == "woven" else 0.4,
                                normal=nrm, sheen=0.8 if face["surface"] == "woven" else 0.0)
        return finish(m, g, out, sh, (0.5, 0.5, 0.5))

    def set_face_mode(mode, design_image=None):
        for f in FACES:
            me = f["obj"].data
            me.materials.clear()
            me.materials.append(face_material(f, mode, design_image))

    # ---------------------------------------------------------------- reusable props
    def street_lamp(name, x, y, h=9.0, arm=2.2, yaw=0.0, temp=3000, energy=600, double=False, parent=None):
        """Bangkok road lamp: tapered pole, curved arm, flat LED head lighting down."""
        mt = mat_metal("lamp_pole", "#7d8387", 0.5)
        g_ = group(name, (x, y, 0), yaw, parent)
        cyl(name + "_base", 0.2, 0.6, (0, 0, 0.3), mt, 16, parent=g_)
        cyl(name + "_pole", 0.11, h, (0, 0, h / 2), mt, 16, r2=0.07, parent=g_)
        for s in ([1, -1] if double else [1]):
            pts = [(0, 0, h - 0.6)]
            for i in range(1, 9):
                t = i / 8
                pts.append((s * arm * t, 0, h - 0.6 + 0.75 * math.sin(t * math.pi / 2)))
            for i in range(len(pts) - 1):
                beam(name + "_arm%d_%d" % (s, i), pts[i], pts[i + 1], 0.045, mt, 8, parent=g_)
            hx = s * (arm + 0.25)
            boxc(name + "_head%d" % s, (hx, 0, h + 0.17), (0.75, 0.3, 0.1), mt, bevel=0.03, parent=g_)
            boxc(name + "_lens%d" % s, (hx, 0, h + 0.115), (0.62, 0.22, 0.012),
                 mat_emit("lamp_lens_%d" % temp, None, 60.0, temp), parent=g_)
            g_.matrix_world  # noqa
            bpy.context.view_layer.update()
            wp = g_.matrix_world @ Vector((hx, 0, h + 0.08))
            light(name + "_L%d" % s, "SPOT", tuple(wp), energy, temp=temp, target=(wp.x, wp.y, 0),
                  spot_deg=125, blend=0.7, shadow_soft=0.2)
        return g_

    def ac_unit(name, center, yaw=0.0, parent=None):
        g_ = group(name, center, yaw, parent)
        boxc(name + "_body", (0, 0, 0), (0.82, 0.3, 0.56),
             mat_surface("ac_body", "#d9d8d2", "#a9a79e", 3, (0.4, 0.6), 0.05, streaks=0.25), bevel=0.02, parent=g_)
        cyl(name + "_fan", 0.2, 0.02, (0.12, -0.155, 0), mat_plain("ac_grille", "#2b2c2c", 0.6), 20,
            rot=(math.radians(90), 0, 0), parent=g_)
        boxc(name + "_bracket", (0, 0.05, -0.3), (0.86, 0.4, 0.03), mat_metal("bracket", "#4a4a48", 0.6), parent=g_)
        return g_

    def car(name, x, y, yaw=0.0, color="#9aa0a6", lights="tail", parent=None, length=4.5, width=1.8):
        """A sedan in a few bevelled blocks: reads as a car at street distance."""
        g_ = group(name, (x, y, 0), yaw, parent)
        paint = mat_paint("carpaint_" + color, color, 0.3, 0.9)
        glass = mat_window("car_glass", (0.01, 0.012, 0.015))
        tyre = mat_plain("tyre", "#0d0d0d", 0.7)
        l2, w2 = length / 2, width / 2
        boxc(name + "_body", (0, 0, 0.62), (length, width, 0.62), paint, bevel=0.14, parent=g_)
        prism_y(name + "_cabin", [(-1.05, 0.9), (1.05, 0.9), (0.55, 1.42), (-0.85, 1.42)],
                -w2 + 0.12, w2 - 0.12, glass, parent=g_, bevel=0.06)
        boxc(name + "_roof", (-0.15, 0, 1.43), (1.35, width - 0.3, 0.04), paint, bevel=0.02, parent=g_)
        for sx in (-1, 1):
            for sy in (-1, 1):
                cyl(name + "_w%d%d" % (sx, sy), 0.32, 0.22, (sx * (l2 - 0.85), sy * (w2 - 0.08), 0.32), tyre, 20,
                    rot=(math.radians(90), 0, 0), parent=g_)
        # car local frame: front is +X
        tail = mat_emit("taillight", "#ff1a0a", 18.0)
        head = mat_emit("headlight", None, 45.0, 5600)
        for sy in (-1, 1):
            boxc(name + "_tl%d" % sy, (-l2 - 0.005, sy * (w2 - 0.25), 0.78), (0.03, 0.32, 0.1), tail, parent=g_)
            boxc(name + "_hl%d" % sy, (l2 + 0.005, sy * (w2 - 0.25), 0.72), (0.03, 0.3, 0.09),
                 head if lights == "both" else mat_plain("hl_off", "#c8ccd0", 0.1, 1.0), parent=g_)
        return g_

    def traffic_signal(name, x, y, yaw=0.0, arm=4.0, state="red", parent=None):
        g_ = group(name, (x, y, 0), yaw, parent)
        mt = mat_paint("signal_paint", "#d8d6cc", 0.5, 0.2)
        cyl(name + "_pole", 0.12, 6.2, (0, 0, 3.1), mt, 16, parent=g_)
        beam(name + "_arm", (0, 0, 5.9), (arm, 0, 5.9), 0.08, mt, 12, parent=g_)
        hx = arm - 0.4
        boxc(name + "_head", (hx, -0.05, 5.4), (0.35, 0.3, 1.05), mat_plain("signal_black", "#111111", 0.5),
             bevel=0.03, parent=g_)
        cols = {"red": "#ff2a14", "amber": "#ffb000", "green": "#18ff8a"}
        for k, c in enumerate(("red", "amber", "green")):
            on = c == state
            m = mat_emit("sig_" + c, cols[c], 30.0) if on else mat_plain("sig_off_" + c, cols[c], 0.2)
            cyl(name + "_lamp%d" % k, 0.11, 0.02, (hx, -0.21, 5.75 - k * 0.33), m, 20,
                rot=(math.radians(90), 0, 0), parent=g_)
        # pedestrian head on the pole
        boxc(name + "_ped", (0.0, -0.2, 2.8), (0.3, 0.2, 0.55), mat_plain("signal_black", "#111111", 0.5),
             bevel=0.02, parent=g_)
        boxc(name + "_pedlamp", (0.0, -0.305, 2.95), (0.2, 0.01, 0.2), mat_emit("ped_red", "#ff3020", 12.0), parent=g_)
        return g_

    def tower(name, x0, x1, y0, y1, h, wall="#9aa3a6", bay=3.0, floor_h=3.4, lit=0.35, seed=0.0,
              strength=2.5, crown=True, band=False, crown_light=None):
        m = mat_facade("facade_%s_%d" % (name, int(seed * 10)), wall, bay=bay, floor_h=floor_h, win_w=bay * 0.75,
                       win_h=floor_h * 0.6, lit_ratio=lit, seed=seed, lit_strength=strength, band=band)
        objs = [box(name, x0, x1, y0, y1, 0, h, m)]
        if crown:
            objs.append(box(name + "_crown", x0 + 1, x1 - 1, y0 + 1, y1 - 1, h, h + 2.5,
                            mat_surface("crown", "#6c7275", "#4c5254", 2, (0.6, 0.8), 0.1)))
        if crown_light:
            objs.append(box(name + "_crownlight", x0 - 0.05, x1 + 0.05, y0 - 0.05, y1 + 0.05, h - 0.6, h - 0.4,
                            mat_emit("crown_" + crown_light, crown_light, 8.0)))
        return objs

    UPPER_SIGNS = [("คลินิกทันตกรรม", "#f2f4f5", "#1d5fa8"), ("สถาบันกวดวิชา", "#1d3f8a", "#ffffff"),
                   ("ห้องพักรายวัน", "#f4c430", "#7a1f1f"), ("คลินิก", "#ffffff", "#1f7a46"),
                   ("รับทำบัญชี", "#e9e4d6", "#2c2a26")]
    UPPER_SIGNS_MORE = [("สอนภาษาอังกฤษ", "#ffffff", "#b3161b"), ("ฟิตเนส", "#1b1b1b", "#f4c430"),
                        ("สำนักงานบัญชี", "#2f6f5e", "#ffffff")]
    SIGN_WORDS = [("ร้านทอง", "#b3161b", "#f7d26a"), ("กาแฟสด", "#f2efe6", "#2c2a26"),
                  ("ร้านยา", "#1f7a46", "#ffffff"), ("ซักรีด", "#2a62a8", "#ffffff"),
                  ("ข้าวมันไก่", "#f4c430", "#a3171b"), ("ตัดผม", "#ffffff", "#1d3f8a"),
                  ("โรงแรม", "#1b1b1b", "#f2e6c8"), ("นวดแผนไทย", "#5b2a6e", "#f6e7c1"),
                  ("ถ่ายเอกสาร", "#e6e6e6", "#c0151d"), ("อะไหล่ยนต์", "#d64a1c", "#ffffff"),
                  ("ร้านอาหาร", "#7a1f1f", "#ffe9b0"), ("โต๊ะจีน", "#c0151d", "#ffd75e")]
    SIGN_WORDS_MORE = [("ก๋วยเตี๋ยว", "#c0151d", "#ffffff"), ("ร้านแว่นตา", "#14365e", "#ffffff"),
                       ("ซ่อมมือถือ", "#f2f2ee", "#14365e"), ("เบเกอรี่", "#f6e7c1", "#7a3b1f"),
                       ("ร้านดอกไม้", "#ffffff", "#b0204a"), ("ร้านผ้า", "#2f6f5e", "#f6e7c1"),
                       ("ข้าวแกง", "#e8a33a", "#3a1a0c"), ("เครื่องเขียน", "#ffffff", "#2a62a8")]

    def sign_panel(name, x0, x1, z0, z1, y, words, lit=True, parent=None, depth=0.12, font=None, fill=0.66):
        """A shop sign: a coloured panel with real lettering (Thai), lit or not."""
        bg, fg = words[1], words[2]
        lum = sum(w_ * c_ for w_, c_ in zip((0.2126, 0.7152, 0.0722), lin(bg)))
        if lit:
            # the panel glows softly, the letters glow several times brighter: legible at night, and they bloom
            bgs = min(1.1, 0.32 / max(lum, 0.1))
            pm = mat_lightbox("signbg_" + bg, bg, bgs)
            flum = sum(w_ * c_ for w_, c_ in zip((0.2126, 0.7152, 0.0722), lin(fg)))
            if flum > lum:
                tm = mat_emit("signfg_" + fg, fg, 6.0)
            else:
                tm = mat_plain("signfg_ink_" + fg, fg, 0.35)
        else:
            pm = mat_surface("signbg_flat_" + bg, bg, bg, 3, (0.35, 0.5), 0.02, streaks=0.2)
            tm = mat_plain("signfg_flat_" + fg, fg, 0.4)
        box(name, x0, x1, y - depth, y, z0, z1, pm, 0.01, parent=parent)
        hh = (z1 - z0)
        t = text3d(name + "_txt", words[0], hh * 0.92, ((x0 + x1) / 2, y - depth - 0.012, (z0 + z1) / 2 - hh * 0.06),
                   (90, 0, 0), tm, 0.008, font=font or load_font(FONT_SIGN_TH), parent=parent)
        if t is not None:
            # the words fill about two thirds of the panel's width, never more than 70 percent of its height,
            # so a sign reads at a 100 percent crop
            bpy.context.view_layer.update()
            wdt, hgt = t.dimensions.x, t.dimensions.y
            if wdt > 0 and hgt > 0:
                k = min((x1 - x0) * fill / wdt, (z1 - z0) * 0.7 / hgt)
                t.scale = (k, k, k)
        return t

    def blade_sign(name, x, z0, z1, words, lit=True, parent=None, out=1.1, thick=0.22):
        """A vertical sign sticking out from the facade (both faces lettered)."""
        bg, fg = words[1], words[2]
        lum = sum(w_ * c_ for w_, c_ in zip((0.2126, 0.7152, 0.0722), lin(bg)))
        pm = mat_lightbox("signbg_" + bg, bg, min(2.6, 0.6 / max(lum, 0.12))) if lit else mat_plain("signbg_flat_" + bg, bg, 0.4)
        tm = mat_emit("signfg_" + fg, fg, 4.0) if lit else mat_plain("signfg_flat_" + fg, fg, 0.4)
        y0, y1 = -0.25 - out, -0.25
        box(name, x - thick / 2, x + thick / 2, y0, y1, z0, z1, pm, 0.02, parent=parent)
        box(name + "_brk", x - 0.03, x + 0.03, -0.25, 0.0, z1 - 0.2, z1 - 0.1, mat_metal("bracket", "#4a4a48", 0.6), parent=parent)
        n = len(words[0])
        for s in (-1, 1):
            # stack the letters vertically, one glyph cluster per line
            chunks = thai_clusters(words[0])
            step = (z1 - z0) * 0.88 / max(1, len(chunks))
            for i, ch in enumerate(chunks):
                zc = z1 - (z1 - z0) * 0.06 - step * (i + 0.5)
                text3d(name + "_t%d_%d" % (s, i), ch, min(step * 0.95, out * 0.62),
                       (x + s * (thick / 2 + 0.006), (y0 + y1) / 2, zc), (90, 0, 90 * s), tm, 0.004, parent=parent)
        return n

    def thai_clusters(word):
        """Split Thai text into display clusters (a base letter with its marks)."""
        marks = set("ัิีึืฺุู็่้๊๋์ํ")
        out = []
        for ch in word:
            if out and ch in marks:
                out[-1] += ch
            elif out and ch == "ำ":
                out[-1] += ch
            else:
                out.append(ch)
        return out

    PACK_COLORS = ["#c94b3b", "#e8d9b5", "#2e5f7a", "#d9a441", "#5f7d55", "#f2efe6", "#3b3b3b", "#b86a7c",
                   "#8a5a3a", "#1f4e8c", "#e2c044"]

    def stock_run(name, sx, y0, z0, rng, parent):
        """A facing of one product on a shelf: the same pack repeated, the way shops stock. Boxes, bottles, jars or
        cup noodles, each with a printed label."""
        kind = rng.choice(["box", "box", "bottle", "jar", "cup"])
        c = rng.choice(PACK_COLORS)
        mt = mat_pack("pack_%s_%s" % (kind, c), c, kind)
        n_ = rng.randint(2, 5)
        if kind == "box":
            w_, h_ = rng.uniform(0.07, 0.14), rng.uniform(0.12, 0.3)
            for i in range(n_):
                y = y0 + i * (w_ + 0.006)
                box(name + "_%d" % i, sx - 0.14, sx + 0.14, y, y + w_, z0, z0 + h_, mt, 0.003, parent=parent)
        else:
            r_ = {"bottle": 0.035, "jar": 0.05, "cup": 0.048}[kind]
            h_ = {"bottle": rng.uniform(0.22, 0.3), "jar": rng.uniform(0.1, 0.16), "cup": 0.1}[kind]
            for row in (-0.07, 0.07):
                for i in range(n_):
                    ob = cyl(name + "_%d_%.2f" % (i, row), r_, h_, (sx + row, y0 + r_ + i * (2 * r_ + 0.004), z0 + h_ / 2),
                             mt, 14, r2=r_ * (0.8 if kind == "cup" else 1.0), parent=parent)
                    if kind == "bottle":
                        cyl(name + "_n%d_%.2f" % (i, row), r_ * 0.35, 0.05,
                            (sx + row, y0 + r_ + i * (2 * r_ + 0.004), z0 + h_ + 0.025), mt, 10, parent=parent)

    def shophouse(name, origin, rot, width, floors=4, ground_h=4.0, floor_h=3.2, depth=12.0, wall="#d8d0bf",
                  shop="open", interior_temp=4500, interior_power=180, sign=None, sign_lit=True, rng=None,
                  shutter_color="#7f8a86", lit_windows=0.35, grills=True, ac=True, awning=None, blade=None,
                  interior=True, parent=None, vary=False, spill=0.0, tube_k=1.0):
        """One Bangkok shophouse unit. Built in local space (facade on y=0 facing -Y,
        x from 0 to width, body toward +Y), then placed at origin with rotation rot (deg).
        shop: "open" (lit interior), "half" (shutter half down), "closed" (shutter down)."""
        rng = rng or random.Random(7)
        # vary: each unit gets its own floor heights, paint, window states, balconies, shutters and an odd upper
        # sign, from its own seed (the street's main random sequence is left alone)
        vr = random.Random(zlib.crc32(name.encode()))
        if vary:
            floor_h *= vr.uniform(0.93, 1.09)
            ground_h *= vr.uniform(0.96, 1.07)
        P = group(name, (origin[0], origin[1], 0), rot, parent)
        P["ground_h"] = ground_h
        top = ground_h + (floors - 1) * floor_h
        w = width
        plaster = mat_surface("plaster_" + wall, wall, "#8f897d", 1.3, (0.7, 0.95), 0.14, streaks=0.6, grime=0.4, patches=0.18)
        conc = mat_surface("concrete_light", "#bdb8ae", "#8a857b", 1.8, (0.8, 0.95), 0.2, streaks=0.35)
        # body
        box(name + "_body", 0.05, w - 0.05, 0.5, depth, ground_h - 0.1, top + 0.9, plaster, parent=P)
        box(name + "_rear", 0.05, w - 0.05, 9.2, depth, 0, ground_h, plaster, parent=P)
        for px in (0.0, w - 0.25):
            box(name + "_pil%.1f" % px, px, px + 0.25, -0.14, 0.5, 0, top + 0.9, plaster, 0.015, parent=P)
        for k in range(floors):
            zf = ground_h + k * floor_h
            lip = 0.28
            box(name + "_slab%d" % k, 0.25, w - 0.25, -lip, 0.5, zf - 0.08, zf + 0.18, conc, 0.015, parent=P)
        box(name + "_parapet", 0.0, w, -0.14, 0.4, top + 0.2, top + 1.0, plaster, 0.015, parent=P)
        if rng.random() < 0.6:
            cyl(name + "_tank", 0.55, 1.3, (w * rng.uniform(0.3, 0.7), depth * 0.5, top + 1.55),
                mat_surface("tank", "#a8b3b9", "#6d777c", 3, (0.4, 0.6), 0.05), 20, parent=P)
        gx0, gx1 = 0.25, w - 0.25
        open_top = 3.0
        # sign band above the opening
        box(name + "_band", gx0, gx1, 0.0, 0.5, open_top, ground_h - 0.08, plaster, parent=P)
        if sign is not None:
            sign_panel(name + "_sign", gx0 + 0.08, gx1 - 0.08, open_top + 0.12, ground_h - 0.18, -0.02, sign, sign_lit, P)
        if awning:
            am = mat_corrugated("awning_" + awning, awning, 0.08, 0.5, axis="X", metal=0.2)
            ob = box(name + "_awning", gx0, gx1, -1.3, 0.0, -0.015, 0.015, am, parent=P)
            ob.location = (0, 0, open_top - 0.05)
            ob.rotation_euler = (math.radians(12), 0, 0)      # slopes down and out, clear of the sign above
        # roller shutter housing
        box(name + "_shbox", gx0, gx1, 0.02, 0.35, open_top - 0.32, open_top, mat_corrugated("shbox_" + shutter_color, shutter_color, 0.05), 0.01, parent=P)
        sh_m = mat_corrugated("shutter_" + shutter_color, shutter_color, 0.045, 0.5, rust=0.35)
        if shop in ("closed", "half"):
            zb = 0.0 if shop == "closed" else rng.uniform(1.2, 1.8)
            box(name + "_shutter", gx0, gx1, 0.08, 0.12, zb, open_top - 0.3, sh_m, parent=P)
        if shop in ("open", "half") and interior:
            inner = mat_surface("interior_wall", "#efe8dc", "#cfc5b5", 2, (0.8, 0.9), 0.02)
            box(name + "_ifloor", gx0, gx1, 0.1, 9, -0.02, 0.005,
                mat_tiles("int_floor", "#b9b2a6", "#aaa397", "#7d776d", 0.6, 0.004, 0.2, 0.1, coat=0.3), parent=P)
            box(name + "_iback", gx0, gx1, 9, 9.2, 0, open_top, inner, parent=P)
            box(name + "_iceil", gx0, gx1, 0.3, 9, open_top - 0.05, open_top, inner, parent=P)
            for sx in (gx0, gx1 - 0.02):
                box(name + "_iside%.1f" % sx, sx, sx + 0.02, 0.3, 9, 0, open_top, inner, parent=P)
            shelf = mat_surface("shelf_metal", "#8f9493", "#6e7372", 4, (0.4, 0.6), 0.02)
            for side, sx in ((-1, gx0 + 0.3), (1, gx1 - 0.3)):
                for lvl in range(5):
                    zz = 0.35 + lvl * 0.42
                    box(name + "_sh%d%d" % (side, lvl), sx - 0.25, sx + 0.25, 1.2, 8.5, zz, zz + 0.025, shelf, parent=P)
                    yy = 1.3
                    while yy < 8.3:
                        stock_run(name + "_st%d%d" % (side, lvl), sx, yy, zz + 0.025, rng, P)
                        yy += rng.uniform(0.45, 0.8)
            # fluorescent tubes: the cool white of a Bangkok shop at night
            for k in range(3):
                yy = 1.5 + k * 2.4
                box(name + "_tube%d" % k, w / 2 - 0.6, w / 2 + 0.6, yy - 0.03, yy + 0.03, open_top - 0.12, open_top - 0.08,
                    mat_emit("tube_%d_%.2f" % (interior_temp, tube_k), None, 25.0 * tube_k, interior_temp), parent=P)
            bpy.context.view_layer.update()
            wp = P.matrix_world @ Vector((w / 2, 4.0, open_top - 0.2))
            light(name + "_in", "AREA", tuple(wp), interior_power, temp=interior_temp,
                  target=tuple(P.matrix_world @ Vector((w / 2, 4.0, 0))), size=w - 0.8, size_y=6.0)
            if spill:
                # the open front throws its light out across the pavement
                sp_ = P.matrix_world @ Vector((w / 2, 0.6, 2.2))
                light(name + "_spill", "AREA", tuple(sp_), spill, temp=min(interior_temp, 3400),
                      target=tuple(P.matrix_world @ Vector((w / 2, -3.0, 0.0))), size=w - 0.6, size_y=1.6, spread=120)
            # counter near the front
            box(name + "_counter", gx0 + 0.3, gx0 + 1.4, 0.8, 1.4, 0, 0.95,
                mat_surface("counter", "#c9c2b5", "#a8a194", 3, (0.4, 0.6), 0.02), 0.01, parent=P)
        # upper floors: real window openings, frames, grills, AC units
        frame = mat_metal("win_frame", "#5d5f5c", 0.5)
        gm = mat_metal("grill", "#2a2d2c", 0.55)
        for k in range(1, floors):
            zf = ground_h + (k - 1) * floor_h
            wx0, wx1 = 0.6, w - 0.6
            wz0, wz1 = zf + 0.95, zf + floor_h - 0.45
            box(name + "_wl%d" % k, 0.25, wx0, 0.0, 0.5, zf + 0.18, zf + floor_h - 0.08, plaster, parent=P)
            box(name + "_wr%d" % k, wx1, w - 0.25, 0.0, 0.5, zf + 0.18, zf + floor_h - 0.08, plaster, parent=P)
            box(name + "_wb%d" % k, wx0, wx1, 0.0, 0.5, zf + 0.18, wz0, plaster, parent=P)
            box(name + "_wt%d" % k, wx0, wx1, 0.0, 0.5, wz1, zf + floor_h - 0.08, plaster, parent=P)
            lit = rng.random() < lit_windows
            style = vr.choices(["grill", "balcony", "shutter", "ribbon"], [0.45, 0.25, 0.15, 0.15])[0] if vary else "grill"
            if lit:
                wm = mat_window("win_lit_%d" % (k % 3), lit=kelvin(rng.choice([2700, 3200, 4500])), strength=1.6)
            else:
                wm = mat_window("win_dark")
            if style == "shutter":
                # a floor used as storage: a rolled-down shutter, rust at the bottom
                box(name + "_win%d" % k, wx0, wx1, 0.18, 0.24, wz0, wz1,
                    mat_corrugated("upshutter_" + shutter_color, shutter_color, 0.045, 0.5, rust=0.5), parent=P)
            elif style == "ribbon":
                box(name + "_win%d" % k, wx0, wx1, 0.2, 0.24, wz0, wz1,
                    mat_window("win_ribbon", (0.03, 0.045, 0.05), lit=tuple(x * 0.8 for x in kelvin(6500)), strength=0.4) if lit else mat_window("win_tint", (0.04, 0.06, 0.07)), parent=P)
            else:
                box(name + "_win%d" % k, wx0, wx1, 0.2, 0.24, wz0, wz1, wm, parent=P)
            if style == "balcony":
                rail = mat_metal("balc_rail", vr.choice(["#3d4140", "#6f6a5f", "#e2ded3"]), 0.5)
                box(name + "_balc%d" % k, 0.3, w - 0.3, -0.95, 0.0, zf + 0.02, zf + 0.16, conc, 0.01, parent=P)
                box(name + "_btop%d" % k, 0.3, w - 0.3, -0.97, -0.93, zf + 1.08, zf + 1.12, rail, parent=P)
                nb_ = int((w - 0.6) / 0.12)
                for i in range(nb_ + 1):
                    xx = 0.3 + i * (w - 0.6) / nb_
                    box(name + "_bal%d_%d" % (k, i), xx - 0.01, xx + 0.01, -0.96, -0.94, zf + 0.16, zf + 1.08, rail, parent=P)
                for j_ in range(vr.randint(0, 3)):
                    # laundry over the rail: a towel, a shirt
                    cx_ = vr.uniform(0.6, w - 0.8)
                    c_ = vr.choice(["#c23b3b", "#f0ece2", "#2f5f9a", "#e3b23c", "#6d8f6a"])
                    box(name + "_cloth%d_%d" % (k, j_), cx_, cx_ + vr.uniform(0.35, 0.6), -0.99, -0.975, zf + 0.6, zf + 1.1,
                        mat_surface("cloth_" + c_, c_, c_, 20, (0.85, 0.95), 0.2), parent=P)
                for j_ in range(vr.randint(1, 3)):
                    px_ = vr.uniform(0.5, w - 0.5)
                    cyl(name + "_bpot%d_%d" % (k, j_), 0.14, 0.26, (px_, -0.6, zf + 0.29), mat_plain("pot_tc", "#9a5638", 0.7), 14, parent=P)
                    for q_ in range(5):
                        a_ = vr.uniform(0, 6.28)
                        ln_ = vr.uniform(0.25, 0.5)
                        beam(name + "_bleaf%d_%d_%d" % (k, j_, q_), (px_, -0.6, zf + 0.4),
                             (px_ + math.cos(a_) * 0.12, -0.6 + math.sin(a_) * 0.12, zf + 0.4 + ln_), 0.02,
                             mat_surface("leaf_dk", "#36592c", "#1d3418", 18, (0.4, 0.6), 0.1), 6, parent=P)
            if vary and k == 1 and vr.random() < 0.22:
                # an upper sign: a tutor school, a clinic, a dentist, lit at night
                words = sign_draw(vr.choice(UPPER_SIGNS), "upper_deck")
                sign_panel(name + "_usign", 0.45, w - 0.45, wz1 - 0.05, zf + floor_h - 0.14, -0.05, words, True, P, 0.08)
            nm = max(2, int((wx1 - wx0) / 0.9))
            for i in range(nm + 1):
                xx = wx0 + i * (wx1 - wx0) / nm
                box(name + "_mul%d_%d" % (k, i), xx - 0.03, xx + 0.03, 0.14, 0.2, wz0, wz1, frame, parent=P)
            box(name + "_tr%d" % k, wx0, wx1, 0.14, 0.2, wz1 - 0.55, wz1 - 0.5, frame, parent=P)
            box(name + "_sill%d" % k, wx0 - 0.05, wx1 + 0.05, -0.08, 0.3, wz0 - 0.08, wz0, conc, 0.01, parent=P)
            if grills and rng.random() < 0.8 and style in ("grill", "balcony"):
                nb = int((wx1 - wx0) / 0.13)
                for i in range(nb + 1):
                    xx = wx0 + i * (wx1 - wx0) / nb
                    box(name + "_gb%d_%d" % (k, i), xx - 0.008, xx + 0.008, -0.03, -0.01, wz0, wz1, gm, parent=P)
                for zz in (wz0 + 0.04, (wz0 + wz1) / 2, wz1 - 0.04):
                    box(name + "_gh%d_%.2f" % (k, zz), wx0, wx1, -0.04, -0.01, zz - 0.012, zz + 0.012, gm, parent=P)
            if ac and rng.random() < 0.55:
                ax = rng.choice([0.9, w - 0.9])
                ac_unit(name + "_ac%d" % k, (ax, -0.32, zf + 0.55), 0, P)
            if rng.random() < 0.3 and False:
                # potted plants on the sill
                for j in range(rng.randint(1, 3)):
                    px = rng.uniform(wx0 + 0.2, wx1 - 0.2)
                    cyl(name + "_pot%d%d" % (k, j), 0.11, 0.18, (px, -0.0, wz0 + 0.09), mat_plain("pot", "#8a4b32", 0.7), 12, parent=P)
                    sphere(name + "_leaf%d%d" % (k, j), 0.17, (px, -0.0, wz0 + 0.3), mat_surface("leaf", "#2f5a2a", "#18301a", 8, (0.45, 0.6), 0.3), 10, parent=P, scale=(1, 1, 0.8))
        if blade is not None:
            blade_sign(name + "_blade", rng.choice([0.45, w - 0.45]), ground_h + 0.6, ground_h + 0.6 + min(4.2, (floors - 1) * floor_h - 1.0),
                       blade, True, P)
        return P

    def sansevieria(name, loc, h=0.75, rng=None, pot_color="#d7d1c4", pot_r=0.2, pot_h=0.42):
        """A snake plant in a pot: tall blade leaves, the shop doorway classic."""
        rng = rng or random.Random(3)
        g_ = group(name, loc, rng.uniform(0, 360))
        cyl(name + "_pot", pot_r, pot_h, (0, 0, pot_h / 2), mat_surface("pot_" + pot_color, pot_color, "#a49d90", 4, (0.45, 0.7), 0.05, grime=0.2),
            28, r2=pot_r * 0.82, parent=g_)
        cyl(name + "_soil", pot_r * 0.95, 0.02, (0, 0, pot_h - 0.03), mat_plain("soil", "#2a2018", 0.95), 24, parent=g_)
        leaf = mat_surface("sansev", "#3c5e2f", "#1d3418", 18, (0.35, 0.55), 0.1, stretch=(1, 1, 0.15))
        edge = mat_plain("sansev_edge", "#b8b04a", 0.5)
        n = rng.randint(11, 15)
        for i in range(n):
            a = rng.uniform(0, 2 * math.pi)
            r0 = rng.uniform(0.0, pot_r * 0.6)
            ln = h * rng.uniform(0.55, 1.0)
            wd = rng.uniform(0.045, 0.07)
            tilt = rng.uniform(4, 16)
            # a blade: tapered quad strip with a slight twist
            verts, faces = [], []
            seg = 6
            for j in range(seg + 1):
                t = j / seg
                ww = wd * (1 - t ** 1.6) + 0.002
                tw = math.radians(25 * t)
                for s_ in (-1, 1):
                    verts.append((s_ * ww * math.cos(tw), s_ * ww * math.sin(tw) + 0.012 * math.sin(t * 3), t * ln))
            for j in range(seg):
                faces.append((2 * j, 2 * j + 1, 2 * j + 3, 2 * j + 2))
            ob = mesh_obj(name + "_blade%d" % i, verts, faces, leaf, smooth=True)
            md = ob.modifiers.new("solid", "SOLIDIFY")
            md.thickness = 0.004
            ob.parent = g_
            ob.location = (r0 * math.cos(a), r0 * math.sin(a), pot_h - 0.03)
            ob.rotation_euler = (math.radians(tilt) * math.cos(a), math.radians(tilt) * math.sin(a), a)
        return g_

    def clipped_shrub(name, center, size, rng, density=1700.0, leaf=(0.045, 0.022)):
        """A clipped hedge (box-leaf or Ficus): a dark rounded core under thousands of real leaf cards scattered on
        its clipped surface, each leaf tilted a little off the surface normal and pushed out a little, each with its
        own green. Reads as foliage at a 100 percent crop, not as a green blob."""
        sx, sy, sz = size
        cx, cy, cz = center
        core = mat_surface("shrub_core", "#16260f", "#0c1609", 6, (0.7, 0.85), 0.3)
        cb = boxc(name + "_core", center, (sx - 0.06, sy - 0.06, sz - 0.05), core, bevel=min(sx, sy, sz) * 0.18)
        cb.modifiers["bevel"].segments = 4
        # the leaf material: per-leaf colour from a colour attribute, a little translucency, a waxy sheen
        m, g, out = new_mat("leaf_card")
        vc = g.n("ShaderNodeVertexColor")
        vc.layer_name = "leafcol"
        tc = g.n("ShaderNodeTexCoord")
        sep = g.n("ShaderNodeSeparateXYZ")
        g.set(sep.inputs[0], tc.outputs["UV"])
        vein = g.math("SUBTRACT", 1.0, g.math("MULTIPLY", g.math("ABSOLUTE", g.math("SUBTRACT", sep.outputs[1], 0.5)), 2.0))
        col = g.mix(g.maprange(vein, 0.9, 1.0, 0.0, 0.35), vc.outputs["Color"], (0.35, 0.45, 0.18))
        sh = principled(g, col, 0.42, 0.0, 0.5, subsurface=0.25, coat=0.25, coat_rough=0.2)
        leafm = finish(m, g, out, sh, (0.1, 0.25, 0.08))
        pal = [lin(c_) for c_ in ("#2f5a24", "#3c6b2b", "#26491d", "#4a7a33", "#355f28", "#5b8a3c", "#1f3c18")]
        faces_ = [((0, 0, 1), sx * sy), ((1, 0, 0), sy * sz), ((-1, 0, 0), sy * sz), ((0, 1, 0), sx * sz), ((0, -1, 0), sx * sz)]
        tot = sum(a for _, a in faces_)
        verts, fcs, cols, uvs = [], [], [], []
        n_ = int(density * tot)
        for _ in range(n_):
            r_ = rng.uniform(0, tot)
            for nrm_, a in faces_:
                if r_ <= a:
                    break
                r_ -= a
            nx, ny, nz = nrm_
            # a point on that face of the clipped box, rounded toward the edges
            px = cx + (nx * sx / 2 if nx else rng.uniform(-sx / 2, sx / 2))
            py = cy + (ny * sy / 2 if ny else rng.uniform(-sy / 2, sy / 2))
            pz = cz + (sz / 2 if nz else rng.uniform(-sz / 2, sz / 2))
            ex = abs(px - cx) / (sx / 2)
            ey = abs(py - cy) / (sy / 2)
            ez = (pz - cz) / (sz / 2)
            pull = max(0.0, max(ex, ey) - 0.85) * max(0.0, ez - 0.6) * 0.25
            N = Vector((nx, ny, nz)) + Vector(((px - cx) / sx, (py - cy) / sy, max(0, ez) * 0.5)) * 0.6
            N.normalize()
            P_ = Vector((px, py, pz)) - N * pull + N * rng.uniform(-0.01, 0.025)
            T = N.orthogonal().normalized()
            T = Matrix.Rotation(rng.uniform(0, 6.283), 3, N) @ T
            Nt = Matrix.Rotation(rng.uniform(-0.9, 0.9), 3, T) @ N
            B = Nt.cross(T).normalized()
            ln = leaf[0] * rng.uniform(0.7, 1.25)
            wd = leaf[1] * rng.uniform(0.7, 1.2)
            fold = Nt * wd * 0.18
            i0 = len(verts)
            verts += [tuple(P_), tuple(P_ + T * ln * 0.45 + B * wd / 2 + fold), tuple(P_ + T * ln),
                      tuple(P_ + T * ln * 0.45 - B * wd / 2 + fold)]
            fcs.append((i0, i0 + 1, i0 + 2, i0 + 3))
            c_ = rng.choice(pal)
            k_ = rng.uniform(0.85, 1.4)
            cols.append((min(1, c_[0] * k_), min(1, c_[1] * k_), min(1, c_[2] * k_), 1.0))
            uvs.append(((0, 0.5), (0.45, 1.0), (1, 0.5), (0.45, 0.0)))
        ob = mesh_obj(name + "_leaves", verts, fcs, leafm)
        me = ob.data
        ca = me.color_attributes.new("leafcol", "FLOAT_COLOR", "CORNER")
        uvl = me.uv_layers.new(name="UVMap")
        for pi, poly in enumerate(me.polygons):
            for k_, li in enumerate(poly.loop_indices):
                ca.data[li].color = cols[pi]
                uvl.data[li].uv = uvs[pi][k_]
        return ob

    def boxes_mesh(name, boxes, colors, rough=(0.5, 0.7), parent=None, sheen=0.0, bevel=0.0):
        """Many small boxes in one mesh, each with its own colour (a colour attribute): book spines, tins, frames.
        boxes: [(x0, x1, y0, y1, z0, z1)], colors: one hex or linear tuple per box."""
        verts, faces, cols = [], [], []
        for (x0, x1, y0, y1, z0, z1), c in zip(boxes, colors):
            i0 = len(verts)
            verts += [(x0, y0, z0), (x1, y0, z0), (x1, y1, z0), (x0, y1, z0),
                      (x0, y0, z1), (x1, y0, z1), (x1, y1, z1), (x0, y1, z1)]
            for f in ((0, 3, 2, 1), (4, 5, 6, 7), (0, 1, 5, 4), (1, 2, 6, 5), (2, 3, 7, 6), (3, 0, 4, 7)):
                faces.append(tuple(i0 + k for k in f))
                cols.append(L(c) if isinstance(c, str) else c)
        m, g, out = new_mat("boxes_" + name)
        vc = g.n("ShaderNodeVertexColor")
        vc.layer_name = "bcol"
        nz = g.noise(g.coords("Object"), 30, 4, 0.5)
        sh = principled(g, vc.outputs["Color"], g.maprange(nz, 0.3, 0.7, rough[0], rough[1]), 0.0, 0.45, sheen=sheen)
        mat = finish(m, g, out, sh, (0.5, 0.5, 0.5))
        ob = mesh_obj(name, verts, faces, mat)
        ca = ob.data.color_attributes.new("bcol", "FLOAT_COLOR", "CORNER")
        for pi, poly in enumerate(ob.data.polygons):
            c = cols[pi]
            for li in poly.loop_indices:
                ca.data[li].color = (c[0], c[1], c[2], 1.0)
        if bevel:
            add_bevel(ob, bevel, 1)
        if parent is not None:
            ob.parent = parent
        return ob

    BOOK_COLS = ["#efe6d2", "#1f2a44", "#7a2421", "#2f4a35", "#c9962e", "#151515", "#8a8f93", "#b5532f", "#d8cfbf",
                 "#3b5a7a", "#e4b9a0", "#5a4636", "#a7b39a", "#f2efe8", "#2b2b2b"]

    def bookshelf_fill(name, x0, x1, y_back, z0, depth, rng, lean=True):
        """One shelf of books: spines of real widths and heights, the odd short stack lying flat."""
        boxes, cols = [], []
        x = x0 + rng.uniform(0.0, 0.03)
        while x < x1 - 0.03:
            if rng.random() < 0.07:
                # a short stack lying flat
                w_ = rng.uniform(0.16, 0.24)
                if x + w_ > x1:
                    break
                zz = z0
                for k in range(rng.randint(3, 6)):
                    t_ = rng.uniform(0.02, 0.04)
                    dd = depth * rng.uniform(0.75, 0.95)
                    boxes.append((x, x + w_ * rng.uniform(0.85, 1.0), y_back - dd, y_back, zz, zz + t_))
                    cols.append(rng.choice(BOOK_COLS))
                    zz += t_
                x += w_ + 0.01
                continue
            w_ = rng.uniform(0.018, 0.05)
            h_ = rng.uniform(0.17, 0.3)
            dd = depth * rng.uniform(0.7, 0.98)
            boxes.append((x, x + w_, y_back - dd, y_back, z0, z0 + h_))
            cols.append(rng.choice(BOOK_COLS))
            x += w_ + rng.uniform(0.0, 0.004)
            if rng.random() < 0.04:
                x += rng.uniform(0.04, 0.12)          # a gap where a book was sold
        return boxes, cols

    def mall_shop(kind, x0, x1, yf, yb, h, rng):
        """A ground-floor shop as a real room behind its glass: books, tea, or a gallery, with warm light."""
        nm = "fs_%s" % kind
        wallc = {"books": "#efe6d6", "tea": "#e9dcc6", "gallery": "#f3f1ec"}[kind]
        wall = mat_surface("shopwall_" + kind, wallc, wallc, 2, (0.75, 0.9), 0.02)
        floor = {"books": mat_surface("shopfloor_oak", "#9a7550", "#7d5c3d", 3, (0.35, 0.5), 0.04, stretch=(1, 8, 1)),
                 "tea": mat_tiles("shopfloor_tea", "#d9d2c4", "#cbc3b4", "#9c9384", 0.3, 0.003, 0.3, 0.1, coat=0.3),
                 "gallery": mat_surface("shopfloor_conc", "#a8a49c", "#8f8b83", 1.5, (0.3, 0.45), 0.03, coat=0.4)}[kind]
        box(nm + "_floor", x0, x1, yf, yb, -0.01, 0.002, floor)
        box(nm + "_back", x0, x1, yb, yb + 0.2, 0, h, wall)
        box(nm + "_ceil", x0, x1, yf, yb, h, h + 0.1, wall)
        for xx in (x0 - 0.15, x1):
            box(nm + "_side%.1f" % xx, xx, xx + 0.15, yf, yb, 0, h, wall)
        oak = mat_surface("shop_oak", "#8a6440", "#6b4c30", 3, (0.35, 0.55), 0.05, stretch=(1, 1, 6))
        cx = (x0 + x1) / 2
        if kind == "books":
            # floor-to-ceiling oak bookcase on the back wall, full of spines
            box(nm + "_case", x0 + 0.2, x1 - 0.2, yb - 0.42, yb, 0, 0.06, oak)
            allb, allc = [], []
            for lv in range(8):
                z = 0.08 + lv * 0.44
                if z > h - 0.4:
                    break
                box(nm + "_shelf%d" % lv, x0 + 0.2, x1 - 0.2, yb - 0.4, yb, z - 0.03, z, oak)
                b_, c_ = bookshelf_fill(nm + "_b%d" % lv, x0 + 0.25, x1 - 0.25, yb - 0.02, z, 0.3, rng)
                allb += b_
                allc += c_
            for k in range(int((x1 - x0 - 0.4) / 1.2) + 1):
                xx = x0 + 0.2 + k * (x1 - x0 - 0.4) / max(1, int((x1 - x0 - 0.4) / 1.2))
                box(nm + "_upr%d" % k, xx - 0.02, xx + 0.02, yb - 0.4, yb, 0, h - 0.3, oak)
            # a display table of new titles, stacks lying face up
            box(nm + "_table", cx - 1.1, cx + 1.1, yf + 1.0, yf + 2.0, 0.0, 0.78, oak, 0.01)
            for k in range(14):
                tx = cx - 1.0 + (k % 7) * 0.29
                ty = yf + 1.15 + (k // 7) * 0.45
                zz = 0.78
                for j in range(rng.randint(2, 5)):
                    t_ = rng.uniform(0.02, 0.035)
                    allb.append((tx, tx + 0.2, ty, ty + 0.28, zz, zz + t_))
                    allc.append(rng.choice(BOOK_COLS))
                    zz += t_
            boxes_mesh(nm + "_books", allb, allc, (0.45, 0.75))
            temp, power = 3000, 520
        elif kind == "tea":
            # counter, menu board, tins on the shelves, a row of pendant lamps
            green_t = mat_tiles("tea_tiles", "#2f5a48", "#294f40", "#c9c2b0", 0.1, 0.002, 0.25, 0.2, coat=0.5, row=0.1)
            box(nm + "_counter", x0 + 0.6, x1 - 0.6, yf + 1.6, yf + 2.3, 0, 1.0, green_t, 0.01)
            box(nm + "_ctop", x0 + 0.55, x1 - 0.55, yf + 1.55, yf + 2.35, 1.0, 1.05,
                mat_surface("tea_stone", "#ece7dc", "#d8d1c3", 5, (0.15, 0.3), 0.02), 0.004)
            steel = mat_metal("tea_steel", "#cfd2d3", 0.2, True)
            for k in range(4):
                xx = x0 + 1.2 + k * 0.55
                cyl(nm + "_urn%d" % k, 0.13, 0.45, (xx, yf + 2.05, 1.28), steel, 24)
                cyl(nm + "_urnlid%d" % k, 0.135, 0.03, (xx, yf + 2.05, 1.52), mat_plain("tea_black", "#141414", 0.4), 24)
            for k in range(6):
                cyl(nm + "_cup%d" % k, 0.045, 0.12, (x1 - 1.6 + (k % 3) * 0.12, yf + 1.85 + (k // 3) * 0.12, 1.11),
                    mat_plain("tea_cup", "#f4f1ea", 0.3), 16, r2=0.035)
            board = mat_surface("menu_slate", "#1b1f1d", "#232826", 6, (0.7, 0.85), 0.05)
            bw_ = min(3.0, x1 - x0 - 1.0)
            box(nm + "_board", cx - bw_ / 2, cx + bw_ / 2, yb - 0.05, yb, 1.75, 3.25, board)
            chalk = mat_emit("menu_chalk", "#f3ead8", 1.6)
            items = [("ชาไทย", "45"), ("ชาเขียวนม", "50"), ("ชามะนาว", "40"), ("โกโก้เย็น", "55"), ("กาแฟเย็น", "50"), ("ชานมไข่มุก", "60")]
            fth = load_font(FONT_SIGN_TH)
            text3d(nm + "_menuhd", "เมนู", 0.2, (cx - bw_ / 2 + 0.25, yb - 0.07, 3.0), (90, 0, 0), chalk, 0.002, font=fth, align="LEFT")
            for i, (it, pr) in enumerate(items):
                col_ = i // 3
                row_ = i % 3
                xl = cx - bw_ / 2 + 0.25 + col_ * bw_ / 2
                zz = 2.62 - row_ * 0.3
                text3d(nm + "_mi%d" % i, it, 0.15, (xl, yb - 0.07, zz), (90, 0, 0), chalk, 0.002, font=fth, align="LEFT")
                text3d(nm + "_mp%d" % i, pr, 0.15, (xl + bw_ / 2 - 0.55, yb - 0.07, zz), (90, 0, 0), chalk, 0.002, font=fth, align="RIGHT")
            tins, tcol = [], []
            for lv in range(2):
                z = 1.0 + lv * 0.36
                for side in (-1, 1):
                    xa = x0 + 0.3 if side < 0 else x1 - 1.3
                    box(nm + "_tshelf%d%d" % (lv, side), xa, xa + 1.0, yb - 0.3, yb, z - 0.025, z, oak)
                    xx = xa + 0.04
                    while xx < xa + 0.9:
                        w_ = rng.uniform(0.08, 0.11)
                        tins.append((xx, xx + w_, yb - 0.22, yb - 0.08, z, z + rng.uniform(0.14, 0.2)))
                        tcol.append(rng.choice(["#b23a2c", "#2f5a48", "#d9b44a", "#1f2a44", "#e9e2d4", "#6b3e2a"]))
                        xx += w_ + 0.02
            boxes_mesh(nm + "_tins", tins, tcol, (0.2, 0.35))
            for k in range(3):
                px_ = x0 + 1.4 + k * (x1 - x0 - 2.8) / 2
                beam(nm + "_cord%d" % k, (px_, yf + 1.95, h), (px_, yf + 1.95, 2.25), 0.004, mat_plain("cord", "#111111", 0.5))
                cyl(nm + "_shade%d" % k, 0.17, 0.16, (px_, yf + 1.95, 2.18), mat_metal("brass", "#c9a46a", 0.3), 24, r2=0.05)
                sphere(nm + "_bulb%d" % k, 0.05, (px_, yf + 1.95, 2.08), mat_emit("bulb", None, 60, 2400), 12)
                light(nm + "_pl%d" % k, "POINT", (px_, yf + 1.95, 2.04), 45, temp=2500, shadow_soft=0.05)
            temp, power = 2900, 380
        else:
            # gallery: framed colour-field paintings on white walls, track spots on each, a plinth with a vessel
            frames_, fcol = [], []
            pieces = [(-1.9, 1.75, 1.2, 1.5, ("#c4552f", "#e9c89b", "#2a2d34")), (0.0, 1.55, 1.0, 1.0, ("#1f3f5a", "#d9d2c2", "#b23a2c")),
                      (1.85, 1.75, 1.1, 1.45, ("#e2c35a", "#3a4a3a", "#f1ece2"))]
            for i, (dx, zc, w_, h_, pal) in enumerate(pieces):
                fx = cx + dx
                frames_.append((fx - w_ / 2 - 0.04, fx + w_ / 2 + 0.04, yb - 0.06, yb, zc - h_ / 2 - 0.04, zc + h_ / 2 + 0.04))
                fcol.append("#141414" if i != 1 else "#b48a5c")
                m, g, out = new_mat("painting%d" % i)
                tc = g.n("ShaderNodeTexCoord")
                sep = g.n("ShaderNodeSeparateXYZ")
                g.set(sep.inputs[0], tc.outputs["Generated"])
                nz = g.noise(g.coords("Generated", (1, 1, 1)), 6, 4, 0.6)
                zt = g.math("ADD", sep.outputs[2], g.math("MULTIPLY", nz, 0.04))
                c1 = g.mix(g.math("GREATER_THAN", zt, 0.62), L(pal[0]), L(pal[1]))
                c2 = g.mix(g.math("LESS_THAN", zt, 0.22), c1, L(pal[2]))
                sh = principled(g, c2, 0.75, 0.0, 0.4, normal=g.bump(g.noise(g.coords("Object"), 200, 6, 0.6), 0.1, 0.002))
                box(nm + "_canvas%d" % i, fx - w_ / 2, fx + w_ / 2, yb - 0.07, yb - 0.05, zc - h_ / 2, zc + h_ / 2, finish(m, g, out, sh, L(pal[0])))
                light(nm + "_spot%d" % i, "SPOT", (fx, yb - 1.6, h - 0.15), 160, temp=3400, target=(fx, yb, zc), spot_deg=34,
                      blend=0.35, shadow_soft=0.03)
            boxes_mesh(nm + "_frames", frames_, fcol, (0.3, 0.45))
            box(nm + "_track", x0 + 0.3, x1 - 0.3, yb - 1.65, yb - 1.55, h - 0.12, h - 0.06, mat_plain("track_black", "#111111", 0.4))
            boxc(nm + "_plinth", (cx - 1.0, yf + 1.4, 0.5), (0.45, 0.45, 1.0), mat_plain("plinth_white", "#f1efe9", 0.6), bevel=0.005)
            cyl(nm + "_vessel", 0.13, 0.32, (cx - 1.0, yf + 1.4, 1.16), mat_surface("vessel", "#3d3a36", "#2a2825", 20, (0.3, 0.45), 0.1, coat=0.6), 32, r2=0.06)
            box(nm + "_bench", cx + 0.2, cx + 1.8, yf + 1.6, yf + 2.0, 0, 0.45, oak, 0.01)
            temp, power = 3600, 220
        light(nm + "_fill", "AREA", (cx, (yf + yb) / 2, h - 0.05), power, temp=temp, target=(cx, (yf + yb) / 2, 0),
              size=x1 - x0 - 0.4, size_y=yb - yf - 0.4)

    def hanging_banner(name, x, y, z0, z1, w, color, ink, lines, rng):
        """A fabric banner suspended in the void on two steel cables: a soft wave in the cloth, a weighted pole at
        the foot, the event's type set across it."""
        nx, nz_ = 12, 40
        verts, faces, uvs = [], [], []
        for j in range(nz_ + 1):
            v = j / nz_
            for i in range(nx + 1):
                u = i / nx
                yy = 0.018 * math.sin(u * math.pi * 2.0 + v * 3.0) * (0.3 + 0.7 * v) + 0.008 * math.sin(v * 9.0)
                verts.append((x + (u - 0.5) * w, y + yy, z0 + v * (z1 - z0)))
        for j in range(nz_):
            for i in range(nx):
                a = j * (nx + 1) + i
                faces.append((a, a + 1, a + nx + 2, a + nx + 1))
        m, g, out = new_mat("banner_cloth_" + name)
        nzt = g.noise(g.coords("Object"), 300, 4, 0.5)
        sh = principled(g, L(color), g.maprange(nzt, 0.3, 0.7, 0.7, 0.85), 0.0, 0.4, sheen=0.4,
                        normal=g.bump(nzt, 0.08, 0.001))
        ob = mesh_obj(name, verts, faces, finish(m, g, out, sh, L(color)), smooth=True)
        md = ob.modifiers.new("solid", "SOLIDIFY")
        md.thickness = 0.004
        alu = mat_metal("banner_pole", "#c9cbcc", 0.3, True)
        for zz in (z0 - 0.02, z1 + 0.02):
            beam(name + "_pole%.1f" % zz, (x - w / 2 - 0.05, y, zz), (x + w / 2 + 0.05, y, zz), 0.022, alu, 12)
        cabm = mat_metal("cable_steel", "#9a9c9d", 0.35)
        for sx in (-1, 1):
            beam(name + "_cable%d" % sx, (x + sx * (w / 2 - 0.1), y, z1 + 0.02), (x + sx * (w / 2 - 0.1), y, 21.5), 0.004, cabm, 6)
        # the graphic: one left edge; 'งานหนังสือ' over 'ฤดูฝน' at leading 1.05, the first line filling 70 percent of the
        # width, top-aligned 6 percent below the banner top; the dates a third of the size in the regular weight;
        # 'ชั้น 2 ร้านหนังสือ' on the bottom margin
        fb = load_font(FONT_SIGN_TH)
        fr = load_font([os.path.join(os.path.dirname(os.path.abspath(__file__)), "..", "example", "sv", "fonts", "Sarabun-Regular.ttf")])
        ink_m = mat_plain("banner_ink_" + ink, ink, 0.6)
        mg = 0.06 * (z1 - z0)
        xl = x - w / 2 + 0.15 * w
        yt = y - 0.03

        def zspan(o):
            bpy.context.view_layer.update()
            zs = [(o.matrix_world @ Vector(c_)).z for c_ in o.bound_box]
            return min(zs), max(zs)

        t1 = text3d(name + "_t0", "งานหนังสือ", 1.0, (xl, yt, 0.0), (90, 0, 0), ink_m, 0.002, font=fb, align="LEFT",
                    valign="BOTTOM_BASELINE")
        bpy.context.view_layer.update()
        em = 0.70 * w / max(t1.dimensions.x, 1e-3)
        t1.data.size = em
        lo, hi = zspan(t1)
        base1 = (z1 - mg) - hi            # the top of the marks sits on the top margin
        t1.location.z = base1
        emt = 0.543 * em                  # Blender's font size to the font's em (Sarabun's 0.70 em cap height is 0.38 per unit)
        base2 = base1 - 1.05 * emt
        text3d(name + "_t1", "ฤดูฝน", em, (xl, yt, base2), (90, 0, 0), ink_m, 0.002, font=fb, align="LEFT", valign="BOTTOM_BASELINE")
        base3 = base2 - 0.35 * emt - 0.2 * emt - 0.70 * emt / 3      # under the ฤ's tail, no double spacing
        text3d(name + "_t2", "10 ถึง 25 ตุลาคม", em / 3, (xl, yt, base3), (90, 0, 0), ink_m, 0.001, font=fr, align="LEFT",
               valign="BOTTOM_BASELINE")
        t4 = text3d(name + "_t3", "ชั้น 2  ร้านหนังสือ", em / 3, (xl, yt, 0.0), (90, 0, 0), ink_m, 0.001, font=fb, align="LEFT",
                    valign="BOTTOM_BASELINE")
        lo4, hi4 = zspan(t4)
        t4.location.z = (z0 + mg) - lo4
        C["banner_em"] = em
        return ob

    def motorbike(name, loc, yaw=0.0, color="#b8241c", rider=None):
        """A step-through motorbike (the Bangkok motorbike taxi): two wheels, a body shell, a seat, bars and a lamp;
        rider: None, or a vest colour for a seated rider (the orange win vest)."""
        G_ = group(name, loc, yaw)
        tyre = mat_plain("tyre", "#0d0d0d", 0.7)
        rim = mat_metal("bike_rim", "#b9bcbd", 0.25)
        paint = mat_paint("bikepaint_" + color, color, 0.3, 0.8)
        blk = mat_plain("bike_black", "#161616", 0.5)
        for yy in (-0.62, 0.62):
            cyl(name + "_tyre%.1f" % yy, 0.29, 0.09, (0, yy, 0.29), tyre, 28, rot=(0, math.radians(90), 0), parent=G_)
            cyl(name + "_rim%.1f" % yy, 0.18, 0.1, (0, yy, 0.29), rim, 20, rot=(0, math.radians(90), 0), parent=G_)
        boxc(name + "_floor", (0, 0.05, 0.42), (0.26, 0.6, 0.08), blk, bevel=0.03, parent=G_)
        boxc(name + "_rear", (0, 0.42, 0.62), (0.3, 0.62, 0.4), paint, bevel=0.1, parent=G_)
        boxc(name + "_seat", (0, 0.38, 0.86), (0.28, 0.62, 0.09), blk, bevel=0.04, parent=G_)
        boxc(name + "_front", (0, -0.42, 0.72), (0.3, 0.16, 0.7), paint, rot=(math.radians(-14), 0, 0), bevel=0.06, parent=G_)
        beam(name + "_fork", (0, -0.62, 0.29), (0, -0.52, 1.0), 0.025, rim, 8, parent=G_)
        beam(name + "_bars", (-0.34, -0.5, 1.05), (0.34, -0.5, 1.05), 0.016, blk, 8, parent=G_)
        boxc(name + "_lamp", (0, -0.6, 0.98), (0.16, 0.06, 0.1), mat_emit("bike_lamp", None, 4.0, 5200), parent=G_)
        boxc(name + "_tail", (0, 0.74, 0.66), (0.12, 0.03, 0.05), mat_emit("bike_tail", "#ff2010", 3.0), parent=G_)
        if rider:
            person(name + "_rider", (0, 0.35, 0.0), 0, "ride", shirt=rider, pants="#25282c", seat=0.84, parent=G_)
            # the numbered vest of a motorbike taxi rider, over the shirt
        return G_

    def torus(name, R, r, center, mat=None, seg=64, rseg=20, axis="X", parent=None, squash=1.0):
        """A torus around a local axis (a tyre). squash < 1 flattens the tube toward the axle (a tyre's square
        shoulder)."""
        v, f = [], []
        for i in range(seg):
            a = 2 * math.pi * i / seg
            for j in range(rseg):
                b = 2 * math.pi * j / rseg
                rr_ = R + r * math.cos(b)
                w_ = r * math.sin(b) * squash
                p = (w_, rr_ * math.cos(a), rr_ * math.sin(a))          # axis X
                if axis == "Y":
                    p = (p[1], p[0], p[2])
                elif axis == "Z":
                    p = (p[1], p[2], p[0])
                v.append(p)
        for i in range(seg):
            for j in range(rseg):
                a0, a1 = i * rseg + j, ((i + 1) % seg) * rseg + j
                b0, b1 = i * rseg + (j + 1) % rseg, ((i + 1) % seg) * rseg + (j + 1) % rseg
                f.append((a0, a1, b1, b0))
        ob = mesh_obj(name, v, f, mat, smooth=True)
        recalc(ob)
        ob.location = center
        if parent is not None:
            ob.parent = parent
        return ob

    @cached
    def mat_tyre(name, R=0.24, lugs=72):
        """Rubber with a tread: lugs around the wheel's axle (local X), only on the crown, plus sidewall noise."""
        m, g, out = new_mat(name)
        tc = g.n("ShaderNodeTexCoord")
        sep = g.n("ShaderNodeSeparateXYZ")
        g.set(sep.inputs[0], tc.outputs["Object"])
        ang = g.math("ARCTAN2", sep.outputs[2], sep.outputs[1])
        rad = g.n("ShaderNodeVectorMath", operation="LENGTH")
        cc = g.n("ShaderNodeCombineXYZ")
        g.set(cc.inputs[1], sep.outputs[1])
        g.set(cc.inputs[2], sep.outputs[2])
        g.set(rad.inputs[0], cc.outputs[0])
        crown = g.maprange(rad.outputs["Value"], R + 0.035, R + 0.05, 0.0, 1.0)
        # chevron lugs: a sine around the wheel, offset by the lateral position
        ph = g.math("ADD", g.math("MULTIPLY", ang, lugs / (2 * math.pi) * 2 * math.pi),
                    g.math("MULTIPLY", g.math("ABSOLUTE", sep.outputs[0]), 60.0))
        lug = g.math("GREATER_THAN", g.math("SINE", ph), 0.1)
        groove = g.math("LESS_THAN", g.math("ABSOLUTE", sep.outputs[0]), 0.006)
        h = g.math("MULTIPLY", g.math("SUBTRACT", lug, g.math("MULTIPLY", groove, 1.0)), crown)
        nz = g.noise(tc.outputs["Object"], 220.0, 4.0, 0.6)
        nrm = g.bump(nz, 0.15, 0.0006, normal=g.bump(h, 0.9, 0.003))
        rr = g.maprange(h, 0, 1, 0.82, 0.62)
        col = g.mix(g.maprange(nz, 0.3, 0.7, 0, 1), lin("#121212"), lin("#1b1b1a"))
        return finish(m, g, out, principled(g, col, rr, 0.0, 0.35, normal=nrm), lin("#121212"))

    @cached
    def mat_chrome(name, rough=0.06):
        m, g, out = new_mat(name)
        rr = g.maprange(g.noise(g.coords("Object"), 30, 4, 0.5), 0.3, 0.7, rough * 0.6, rough * 1.6)
        return finish(m, g, out, principled(g, lin("#e8e8e6"), rr, 1.0), lin("#e8e8e6"))

    @cached
    def mat_seat(name):
        """Vinyl seat: black, a slight grain, a clear coat that catches the street light."""
        m, g, out = new_mat(name)
        nz = g.noise(g.coords("Object"), 300.0, 6.0, 0.6)
        return finish(m, g, out, principled(g, lin("#0f0f10"), g.maprange(nz, 0.3, 0.7, 0.42, 0.55), 0.0, 0.5,
                                             normal=g.bump(nz, 0.08, 0.0005), coat=0.7, coat_rough=0.12), lin("#0f0f10"))

    def motorbike_parked(name, loc, yaw=0.0, color="#b8241c", helmet="#f2efe6", lean=7.0):
        """A parked underbone motorbike, no rider: torus tyres with tread, spoked rims, chrome exhaust, a clear-coated
        vinyl seat, mirrors on stalks with a helmet hung by its strap. Leans on its side stand. Faces -Y."""
        G_ = group(name, loc, yaw)
        B = group(name + "_lean", (0, 0, 0), 0, G_)
        B.rotation_euler = (0, math.radians(-lean), 0)
        tyre = mat_tyre("tyre_tread")
        rim = mat_metal("bike_rim2", "#c9cccd", 0.18)
        chrome = mat_chrome("chrome")
        paint = mat_paint("bikepaint2_" + color, color, 0.25, 1.0)
        blk = mat_plain("bike_black2", "#121212", 0.45)
        plast = mat_surface("bike_plastic", "#1a1a1a", "#141414", 30, (0.5, 0.65), 0.05)
        for yy in (-0.63, 0.63):
            # tyre, an open cast wheel (a rim ring, five flat spokes, a hub) and a brake disc: you see through it
            torus(name + "_tyre%.1f" % yy, 0.24, 0.05, (0, yy, 0.29), tyre, 72, 16, parent=B, squash=0.9)
            torus(name + "_rim%.1f" % yy, 0.196, 0.014, (0, yy, 0.29), rim, 64, 10, parent=B, squash=1.8)
            cyl(name + "_hub%.1f" % yy, 0.05, 0.09, (0, yy, 0.29), rim, 24, rot=(0, math.radians(90), 0), parent=B)
            for k in range(5):
                a = 2 * math.pi * k / 5 + (0.3 if yy > 0 else 0)
                sp_ = boxc(name + "_spoke%.1f_%d" % (yy, k), (0.0, yy + 0.12 * math.cos(a), 0.29 + 0.12 * math.sin(a)),
                           (0.018, 0.15, 0.026), rim, bevel=0.006, parent=B)
                sp_.rotation_euler = (a, 0, 0)
            cyl(name + "_disc%.1f" % yy, 0.11, 0.005, (-0.055, yy, 0.29), chrome, 40, rot=(0, math.radians(90), 0), parent=B)
            boxc(name + "_caliper%.1f" % yy, (-0.07, yy - 0.06, 0.38), (0.04, 0.06, 0.05), blk, bevel=0.01, parent=B)
        # fenders
        for yy, a0, a1 in ((-0.63, 20, 150), (0.63, 30, 170)):
            pts = []
            for k in range(13):
                a = math.radians(a0 + (a1 - a0) * k / 12)
                pts.append((0.0, yy - 0.33 * math.cos(a), 0.29 + 0.33 * math.sin(a)))
            for k in range(12):
                p0, p1 = pts[k], pts[k + 1]
                beam(name + "_fend%.1f_%d" % (yy, k), p0, p1, 0.06, paint if yy < 0 else blk, 10, parent=B)
        # frame, legshield, floor, body
        beam(name + "_down", (0, -0.45, 0.95), (0, -0.05, 0.38), 0.04, blk, 12, parent=B)
        boxc(name + "_floor", (0, 0.0, 0.4), (0.24, 0.42, 0.05), plast, bevel=0.02, parent=B)
        boxc(name + "_shield", (0, -0.36, 0.7), (0.4, 0.06, 0.55), paint, rot=(math.radians(-18), 0, 0), bevel=0.05, parent=B)
        sphere(name + "_body", 1.0, (0, 0.38, 0.62), paint, 28, scale=(0.17, 0.4, 0.17), parent=B)
        boxc(name + "_side", (0, 0.42, 0.55), (0.3, 0.6, 0.18), paint, bevel=0.07, parent=B)
        boxc(name + "_seat", (0, 0.32, 0.84), (0.27, 0.66, 0.1), mat_seat("seat_vinyl"), bevel=0.045, parent=B)
        boxc(name + "_rack", (0, 0.73, 0.82), (0.22, 0.14, 0.02), chrome, bevel=0.005, parent=B)
        boxc(name + "_tail", (0, 0.8, 0.72), (0.14, 0.04, 0.06), mat_emit("bike_tail_off", "#5a0a06", 0.4), bevel=0.01, parent=B)
        # swingarm, shock, chrome exhaust with its heat shield on the right
        beam(name + "_arm", (0.1, 0.05, 0.36), (0.1, 0.63, 0.29), 0.025, blk, 8, parent=B)
        beam(name + "_shock", (0.12, 0.62, 0.32), (0.12, 0.5, 0.7), 0.02, chrome, 10, parent=B)
        beam(name + "_pipe", (0.06, -0.05, 0.3), (0.17, 0.45, 0.32), 0.022, chrome, 14, parent=B)
        beam(name + "_muffler", (0.17, 0.38, 0.33), (0.18, 0.92, 0.42), 0.048, chrome, 24, parent=B)
        beam(name + "_shieldex", (0.22, 0.45, 0.37), (0.225, 0.78, 0.43), 0.03, blk, 12, parent=B)
        cyl(name + "_tip", 0.03, 0.03, (0.18, 0.93, 0.42), blk, 16, rot=(math.radians(90), 0, 0), parent=B)
        # fork, headset, bars, lamp, mirrors
        for sx in (-0.07, 0.07):
            beam(name + "_fork%.2f" % sx, (sx, -0.63, 0.29), (sx, -0.5, 0.95), 0.02, chrome, 12, parent=B)
        boxc(name + "_head", (0, -0.5, 1.02), (0.36, 0.2, 0.14), paint, bevel=0.05, parent=B)
        boxc(name + "_lamp", (0, -0.61, 1.0), (0.16, 0.03, 0.08), mat_glass("lamp_glass", (0.95, 0.95, 0.9), 0.15), bevel=0.01, parent=B)
        beam(name + "_bars", (-0.36, -0.47, 1.06), (0.36, -0.47, 1.06), 0.013, chrome, 10, parent=B)
        for sx in (-1, 1):
            beam(name + "_grip%d" % sx, (sx * 0.28, -0.47, 1.06), (sx * 0.38, -0.47, 1.06), 0.02, blk, 12, parent=B)
            beam(name + "_stalk%d" % sx, (sx * 0.2, -0.47, 1.08), (sx * 0.26, -0.43, 1.36), 0.007, chrome, 6, parent=B)
            cyl(name + "_mirror%d" % sx, 0.055, 0.02, (sx * 0.265, -0.43, 1.4), chrome, 24, rot=(math.radians(90), 0, 0), parent=B)
        # the helmet, hung by its strap from the left mirror
        hx, hy, hz = -0.27, -0.44, 1.12
        beam(name + "_strap", (-0.265, -0.43, 1.37), (hx, hy, hz + 0.14), 0.006, blk, 6, parent=B)
        H_ = group(name + "_helmet", (hx, hy, hz), 0, B)
        H_.rotation_euler = (math.radians(-25), math.radians(15), 0)
        sphere(name + "_shell", 0.135, (0, 0, 0), mat_paint("helmet_" + helmet, helmet, 0.12, 1.0), 32, scale=(0.9, 1.1, 1.0), parent=H_)
        boxc(name + "_visor", (0, -0.1, 0.0), (0.2, 0.05, 0.09), mat_plain("visor", "#0a0a0a", 0.05, spec=0.9), bevel=0.02, parent=H_)
        cyl(name + "_hrim", 0.13, 0.03, (0, 0, -0.07), blk, 32, parent=H_)
        # the side stand
        beam(name + "_stand", (-0.08, 0.05, 0.38), (-0.26, 0.1, 0.0), 0.012, blk, 6, parent=B)
        # smooth the bevelled panels (no faceted toy look)
        for o in bpy.data.objects:
            if o.name.startswith(name + "_") and o.type == "MESH":
                for p_ in o.data.polygons:
                    p_.use_smooth = True
                bv = o.modifiers.get("bevel")
                if bv:
                    bv.segments = 4
                    try:
                        bv.harden_normals = True
                    except Exception:
                        pass
        return G_

    def stool_stack(name, loc, n=5, color="#c8261c", parent=None):
        """Plastic stools nested in a stack, the way a cart stores them: each lip shows."""
        m = mat_paint("plastic_" + color, color, 0.38, 0.25)
        g_ = group(name, loc, random.uniform(0, 90), parent)
        for k in range(n):
            z = k * 0.055
            cyl(name + "_body%d" % k, 0.18, 0.4, (0, 0, z + 0.2), m, 32, r2=0.145, parent=g_)
            cyl(name + "_lip%d" % k, 0.152, 0.025, (0, 0, z + 0.41), m, 32, parent=g_)
            torus(name + "_rim%d" % k, 0.148, 0.012, (0, 0, z + 0.42), m, 32, 8, axis="Z", parent=g_)
        return g_

    def food_cart(name, loc, yaw=0.0):
        """A stainless street food cart: a glass case, a canopy on a pole, one bare bulb that blooms."""
        G_ = group(name, loc, yaw)
        ss = mat_metal("cart_steel", "#c4c7c8", 0.22, True)
        boxc(name + "_body", (0, 0, 0.55), (1.5, 0.7, 0.62), ss, bevel=0.02, parent=G_)
        boxc(name + "_top", (0, 0, 0.88), (1.56, 0.76, 0.04), ss, bevel=0.01, parent=G_)
        for sx in (-1, 1):
            cyl(name + "_wheel%d" % sx, 0.22, 0.06, (sx * 0.5, -0.38, 0.22), mat_plain("tyre", "#0d0d0d", 0.7), 20,
                rot=(math.radians(90), 0, 0), parent=G_)
        case = mat_glass("cart_glass", (0.95, 0.97, 0.96), 0.08)
        g_ = boxc(name + "_case", (0.2, 0.05, 1.12), (0.95, 0.5, 0.44), case, parent=G_)
        g_["sv_mask_ignore"] = True
        foods = ["#c9862f", "#e7d3a2", "#8a3a1e", "#f0e6c8", "#5b7d3a"]
        for k in range(10):
            boxc(name + "_food%d" % k, (-0.15 + (k % 5) * 0.17, -0.05 + (k // 5) * 0.16, 0.95), (0.13, 0.12, 0.05),
                 mat_plain("food_" + foods[k % 5], foods[k % 5], 0.45), bevel=0.01, parent=G_)
        pot = mat_metal("pot_alu", "#a9adae", 0.3)
        cyl(name + "_pot", 0.17, 0.26, (-0.5, 0.0, 1.03), pot, 24, parent=G_)
        beam(name + "_pole", (-0.68, 0.3, 0.9), (-0.68, 0.3, 2.35), 0.018, ss, 8, parent=G_)
        boxc(name + "_canopy", (-0.1, 0.0, 2.38), (1.8, 1.2, 0.03), mat_surface("canopy", "#c23a2a", "#a02e22", 3, (0.7, 0.85), 0.05),
             rot=(math.radians(-6), 0, 0), parent=G_)
        beam(name + "_cord", (0.15, 0.0, 2.34), (0.15, 0.0, 2.02), 0.004, mat_plain("cord", "#111111", 0.5), 6, parent=G_)
        sphere(name + "_bulb", 0.035, (0.15, 0.0, 1.98), mat_emit("bare_bulb", None, 220.0, 2300), 16, parent=G_)
        bpy.context.view_layer.update()
        wp = G_.matrix_world @ Vector((0.15, 0.0, 1.96))
        light(name + "_bulbL", "POINT", tuple(wp), 70, temp=2300, shadow_soft=0.03)
        return G_

    def kerb_clutter(name, x, y, rng, side=1):
        """What collects on a Bangkok pavement by 7 pm: stools, a crate stack, a bin, a barrel, a pot plant, a cone."""
        plastic_stool(name + "_st1", (x, y, 0.15), rng.choice(["#c8261c", "#2c5aa0", "#1f7a46"]))
        plastic_stool(name + "_st2", (x + side * 0.45, y + 0.6, 0.15), "#c8261c")
        for k in range(rng.randint(2, 4)):
            boxc(name + "_crate%d" % k, (x + side * 0.6, y - 1.1, 0.3 + k * 0.29), (0.55, 0.38, 0.28),
                 mat_paint("crate_" + ("g" if k % 2 else "y"), "#1f7a46" if k % 2 else "#d8a51c", 0.5, 0.1), bevel=0.01)
        cyl(name + "_bin", 0.26, 0.85, (x - side * 0.2, y + 1.6, 0.575), mat_paint("bin_blue", "#1f4e8c", 0.45, 0.2), 24, r2=0.29)
        water_barrel(name + "_barrel", (x + side * 0.1, y - 2.3, 0.15))
        cyl(name + "_pot", 0.22, 0.4, (x + side * 0.7, y + 2.4, 0.35), mat_plain("pot_tc", "#9a5638", 0.7), 20, r2=0.18)
        sphere(name + "_shrub", 0.36, (x + side * 0.7, y + 2.4, 0.78), mat_surface("leaf_dk", "#36592c", "#1d3418", 18, (0.4, 0.6), 0.3), 14,
               scale=(1, 1, 0.8))
        cyl(name + "_cone", 0.16, 0.6, (x - side * 0.4, y - 3.2, 0.45), mat_plain("cone_or", "#ff5a14", 0.5), 20, r2=0.03)

    def train_interior_image(name, w=2048, h=360, seed=4):
        """The far wall of a lit carriage, painted by numpy and blurred as a lens at f/2.8 would blur it two metres
        behind the focus: ceiling light, far windows on the night, bench seats, grab poles, riders as soft shapes."""
        rng = np.random.default_rng(seed)
        img = np.zeros((h, w, 3), np.float32)
        yy = np.linspace(0, 1, h)[:, None]                 # 0 at the top
        wall = np.array(lin("#d7d9d6"))
        img[:] = wall * (0.75 + 0.25 * (1 - yy))[..., None]
        img[: int(h * 0.12)] = np.array(kelvin(5600)) * 2.2           # the ceiling light strip
        img[int(h * 0.12): int(h * 0.16)] = np.array(lin("#9da3a3"))
        win = np.array(lin("#16202e"))
        px_m = w / 15.5
        x = 0.4 * px_m
        while x < w - 1.4 * px_m:
            x0, x1 = int(x), int(x + 1.35 * px_m)
            img[int(h * 0.22): int(h * 0.58), x0:x1] = win
            for k in range(6):                         # far city lights through the far windows
                cx_, cy_ = rng.integers(x0, x1), rng.integers(int(h * 0.3), int(h * 0.55))
                img[cy_ - 1: cy_ + 2, cx_ - 2: cx_ + 3] = np.array(kelvin(rng.choice([2700, 3500, 5600]))) * rng.uniform(0.6, 1.6)
            x += 1.75 * px_m
        img[int(h * 0.66):] = np.array(lin("#3f566e"))            # bench seats
        img[int(h * 0.62): int(h * 0.66)] = np.array(lin("#8b9497"))
        for k in range(int(15.5 / 1.25)):                  # grab poles
            xp = int((0.6 + k * 1.25) * px_m)
            img[int(h * 0.14):, xp - 2: xp + 2] = np.array(lin("#c9cdce"))
        Y, X = np.mgrid[0:h, 0:w].astype(np.float32)
        cloth = ["#1f2a44", "#e9e4d8", "#7a2421", "#2f4a35", "#c9962e", "#151515", "#8a8f93", "#4b5f8a", "#d8b5a0"]
        skin = ["#c99a7c", "#b98463", "#d8ad8f", "#a8745a"]
        for k in range(18):
            cx_ = rng.uniform(0.03, 0.97) * w
            standing = rng.random() < 0.45
            hy = (0.30 if standing else 0.50) * h + rng.uniform(-0.03, 0.03) * h
            hr = 0.055 * h
            body_top = hy + hr * 0.9
            body_h = (0.55 if standing else 0.3) * h
            bw = rng.uniform(0.055, 0.075) * h * 1.8
            c = np.array(lin(rng.choice(cloth)))
            m = ((np.abs(X - cx_) < bw / 2) & (Y > body_top) & (Y < body_top + body_h)).astype(np.float32)
            img = img * (1 - m[..., None]) + c * m[..., None]
            m2 = (((X - cx_) / hr) ** 2 + ((Y - hy) / (hr * 1.15)) ** 2 < 1).astype(np.float32)
            img = img * (1 - m2[..., None]) + np.array(lin(rng.choice(skin))) * m2[..., None]
            m3 = (((X - cx_) / hr) ** 2 + ((Y - hy + hr * 0.35) / (hr * 0.8)) ** 2 < 1).astype(np.float32) * (Y < hy - hr * 0.1)
            img = img * (1 - m3[..., None]) + np.array(lin("#141110")) * m3[..., None]

        def box_blur(a, r, axis):
            c = np.cumsum(np.pad(a, [(r + 1, r) if i == axis else (0, 0) for i in range(3)], mode="edge"), axis=axis)
            sl1 = [slice(None)] * 3
            sl2 = [slice(None)] * 3
            sl1[axis] = slice(2 * r + 1, None)
            sl2[axis] = slice(0, -2 * r - 1)
            return (c[tuple(sl1)] - c[tuple(sl2)]) / (2 * r + 1)
        for _ in range(3):
            img = box_blur(img, 7, 1)
            img = box_blur(img, 6, 0)
        im = bpy.data.images.new(name, w, h, alpha=False, float_buffer=True)
        rgba_ = np.concatenate([img, np.ones((h, w, 1), np.float32)], axis=2)
        im.pixels.foreach_set(rgba_[::-1].astype(np.float32).ravel())
        im.pack()
        return im

    def train_car(name, x0, length, yc, z0, parent, interior, nose=None):
        """One car in metres, in the parent's frame: livery bands in a metallic clear-coated paint, window glass
        with a reflection layer, a lit interior behind it (a ceiling of light, the far wall soft as at f/2.8)."""
        x1 = x0 + length
        m, g, out = new_mat("train_paint")
        sep = g.n("ShaderNodeSeparateXYZ")
        g.set(sep.inputs[0], g.n("ShaderNodeTexCoord").outputs["Object"])
        z = sep.outputs[2]
        base = lin("#dfe3e5")
        navy = lin("#16326e")
        sky = lin("#2f9bd8")
        # the livery sits where it shows above the parapet: a navy band under the windows, a sky-blue line over
        # them, a navy cant rail
        band = g.math("MULTIPLY", g.math("GREATER_THAN", z, z0 + 0.92), g.math("LESS_THAN", z, z0 + 1.22))
        stripe = g.math("MULTIPLY", g.math("GREATER_THAN", z, z0 + 2.9), g.math("LESS_THAN", z, z0 + 3.0))
        top = g.math("MULTIPLY", g.math("GREATER_THAN", z, z0 + 3.02), g.math("LESS_THAN", z, z0 + 3.14))
        col = g.mix(band, base, navy)
        col = g.mix(stripe, col, sky)
        col = g.mix(top, col, navy)
        nz = g.noise(g.coords("Object"), 3, 4, 0.5)
        sh = principled(g, col, g.maprange(nz, 0.3, 0.7, 0.3, 0.4), 0.55, 0.5, coat=1.0, coat_rough=0.25)
        paint = finish(m, g, out, sh, base)
        dark = mat_plain("train_dark", "#16191b", 0.5)
        m, g, out = new_mat("train_glass")
        tr = g.n("ShaderNodeBsdfTransparent", {"Color": (0.82, 0.88, 0.9)})
        gl = g.n("ShaderNodeBsdfGlossy", {"Color": (1, 1, 1), "Roughness": 0.03})
        lw = g.n("ShaderNodeLayerWeight", {"Blend": 0.25})
        mx = g.n("ShaderNodeMixShader")
        g.set(mx.inputs[0], g.math("MAXIMUM", lw.outputs["Fresnel"], 0.1))
        g.set(mx.inputs[1], tr)
        g.set(mx.inputs[2], gl)
        glass = finish(m, g, out, mx.outputs[0], (0.5, 0.6, 0.65))
        y0, y1 = yc - 1.5, yc + 1.5
        zs, zt = z0 + 1.25, z0 + 2.85
        box(name + "_lower", x0, x1, y0, y1, z0, zs, paint, 0.03, parent=parent)
        box(name + "_upper", x0, x1, y0, y1, zt, z0 + 3.2, paint, 0.03, parent=parent)
        roof = box(name + "_roof", x0 + 0.05, x1 - 0.05, y0 + 0.05, y1 - 0.05, z0 + 3.1, z0 + 3.6, paint, 0.22, parent=parent)
        roof.modifiers["bevel"].segments = 4
        box(name + "_skirt", x0 + 0.5, x1 - 0.5, y0 + 0.3, y1 - 0.3, z0 - 0.55, z0, dark, parent=parent)
        # near side: pillars between windows, doors with tall narrow lights
        xx = x0
        k = 0
        layout = []
        pos = x0 + 0.35
        while pos < x1 - 0.5:
            door = k % 3 == 1
            wl = 1.3 if door else 1.45
            if pos + wl > x1 - 0.35:
                break
            layout.append((pos, pos + wl, door))
            pos += wl + 0.42
            k += 1
        edges = [x0] + [v for a_, b_, _ in layout for v in (a_, b_)] + [x1]
        for i in range(0, len(edges), 2):
            if edges[i + 1] - edges[i] > 0.01:
                box(name + "_pil%d" % i, edges[i], edges[i + 1], y0, y0 + 0.08, zs, zt, paint, parent=parent)
        for i, (a_, b_, door) in enumerate(layout):
            if door:
                box(name + "_door%d" % i, a_, b_, y0 + 0.02, y0 + 0.08, z0 + 0.05, zs, paint, parent=parent)
                box(name + "_doorseam%d" % i, (a_ + b_) / 2 - 0.01, (a_ + b_) / 2 + 0.01, y0 - 0.002, y0 + 0.02, z0 + 0.05, zt,
                    dark, parent=parent)
            gp = box(name + "_glass%d" % i, a_, b_, y0 + 0.03, y0 + 0.05, zs, zt, glass, parent=parent)
            gp["sv_mask_ignore"] = True
        # the far side behind the interior card, the ends, a lit ceiling, a dark floor
        box(name + "_far", x0, x1, y1 - 0.08, y1, zs, zt, paint, parent=parent)
        for xe in (x0, x1 - 0.06):
            box(name + "_end%.1f" % xe, xe, xe + 0.06, y0, y1, zs, zt, paint, parent=parent)
        box(name + "_floor", x0, x1, y0, y1, z0 + 0.95, z0 + 1.0, dark, parent=parent)
        m, g, out = new_mat("train_ceiling")
        sep2 = g.n("ShaderNodeSeparateXYZ")
        g.set(sep2.inputs[0], g.n("ShaderNodeTexCoord").outputs["Object"])
        strip = g.math("LESS_THAN", g.math("ABSOLUTE", g.math("SUBTRACT", sep2.outputs[1], yc)), 0.32)
        em = g.n("ShaderNodeEmission", {"Color": kelvin(5600), "Strength": g.maprange(strip, 0, 1, 0.35, 4.5)})
        ceil = finish(m, g, out, em, (0.9, 0.9, 0.9))
        box(name + "_ceil", x0 + 0.06, x1 - 0.06, y0 + 0.1, y1 - 0.1, z0 + 2.98, z0 + 3.02, ceil, parent=parent)
        m, g, out = new_mat("train_far_wall")
        tex = g.n("ShaderNodeTexImage")
        tex.image = interior
        tex.extension = "REPEAT"
        mp = g.n("ShaderNodeMapping")
        g.set(mp.inputs["Vector"], g.n("ShaderNodeTexCoord").outputs["UV"])
        g.set(tex.inputs["Vector"], mp.outputs[0])
        em = g.n("ShaderNodeEmission", {"Color": tex.outputs["Color"], "Strength": 0.75})
        card = finish(m, g, out, em, (0.8, 0.8, 0.8))
        cv = [(x0 + 0.06, y1 - 0.12, zs - 0.75), (x1 - 0.06, y1 - 0.12, zs - 0.75), (x1 - 0.06, y1 - 0.12, z0 + 2.98),
              (x0 + 0.06, y1 - 0.12, z0 + 2.98)]
        co = mesh_obj(name + "_interior", cv, [(0, 1, 2, 3)], card)
        uvl = co.data.uv_layers.new(name="UVMap")
        for li, uv_ in zip(co.data.polygons[0].loop_indices, ((0, 0), (1, 0), (1, 1), (0, 1))):
            uvl.data[li].uv = uv_
        co.parent = parent
        for o in bpy.data.objects:
            if o.name.startswith(name + "_"):
                o["sv_probe"] = "train"
        if nose:
            # a sloped cab at the train's end, a headlamp pair and a dark windscreen
            xe = x0 if nose < 0 else x1
            d_ = -1 if nose < 0 else 1
            prism_y(name + "_cab", [(xe, z0), (xe + d_ * 0.9, z0), (xe + d_ * 0.9, z0 + 3.3), (xe + d_ * 0.35, z0 + 3.45), (xe, z0 + 3.45)],
                    y0 + 0.1, y1 - 0.1, paint, parent=parent, bevel=0.08)
            box(name + "_wind", xe + d_ * 0.92, xe + d_ * 0.96, y0 + 0.4, y1 - 0.4, z0 + 1.6, z0 + 2.9, dark, parent=parent)
            for sy in (-1, 1):
                box(name + "_hl%d" % sy, xe + d_ * 0.9, xe + d_ * 0.95, yc + sy * 0.95 - 0.2, yc + sy * 0.95 + 0.2, z0 + 0.7, z0 + 0.86,
                    mat_emit("train_head", None, 30.0, 5200), parent=parent)

    def plastic_stool(name, loc, color="#c8261c", parent=None):
        m = mat_paint("plastic_" + color, color, 0.35, 0.3)
        g_ = group(name, loc, random.uniform(0, 90), parent)
        cyl(name + "_seat", 0.15, 0.03, (0, 0, 0.44), m, 28, parent=g_)
        cyl(name + "_body", 0.135, 0.42, (0, 0, 0.215), m, 28, r2=0.18, parent=g_)
        return g_

    def water_barrel(name, loc, color="#2c5aa0"):
        m = mat_paint("barrel_" + color, color, 0.4, 0.1)
        cyl(name, 0.29, 0.88, (loc[0], loc[1], 0.44 + loc[2]), m, 32)
        cyl(name + "_lid", 0.3, 0.04, (loc[0], loc[1], 0.9 + loc[2]), m, 32)

    def limb(name, p0, p1, r0, r1, mat, parent, segs=10):
        """A tapered limb between two joints (a cone frustum), local to the parent."""
        p0, p1 = Vector(p0), Vector(p1)
        d = p1 - p0
        ob = cyl(name, r0, max(d.length, 1e-4), (p0 + p1) / 2, mat, segs, r2=r1, parent=parent)
        ob.rotation_mode = "QUATERNION"
        ob.rotation_quaternion = d.to_track_quat("-Z", "Y") if False else d.to_track_quat("Z", "Y")
        # cyl's cone runs radius1 at -Z to radius2 at +Z: p0 is the thick end
        return ob

    SKIN = ["#c99a7c", "#b98463", "#d8ad8f", "#a8745a", "#e0b99d"]

    def person(name, loc, yaw=0.0, pose="walk", shirt="#d9d4c8", pants="#2b2f36", skin=None, hair="#16120f",
               h=1.70, parent=None, step=0.0, vel=None, seat=0.46, lean=0.0, rng=None, bag=None):
        """A low-poly person at true scale (1.70 m by default): tapered limbs, an ellipsoid torso and head, hair, shoes.
        Faces -Y in its own frame (yaw turns it). pose: walk, stand, sit (on a seat of height seat), ride (a motorbike
        pillion or driver, hands forward on the bars). vel: world velocity in m/s for motion blur (keyed on frames 0 and
        2, rendered on frame 1)."""
        rng = rng or random.Random(zlib.crc32(name.encode()))
        k = h / 1.70
        skin = skin or rng.choice(SKIN)
        P = group(name, loc, yaw, parent)
        ms = mat_surface("cloth_" + shirt, shirt, shirt, 30, (0.75, 0.92), 0.12)
        mp = mat_surface("cloth_" + pants, pants, pants, 30, (0.7, 0.9), 0.12)
        mk = mat_surface("skin_" + skin, skin, skin, 8, (0.42, 0.55), 0.03)
        mh = mat_plain("hair_" + hair, hair, 0.5)
        mshoe = mat_plain("shoe_dark", "#141414", 0.45)
        hip = 0.93 * k
        sh_z = 1.42 * k
        thigh, shin = 0.44 * k, 0.43 * k
        uarm, farm = 0.29 * k, 0.26 * k

        def rot_x(v, a):
            a = math.radians(a)
            x, y, z = v
            return (x, y * math.cos(a) - z * math.sin(a), y * math.sin(a) + z * math.cos(a))
        legs, arms = [], []
        if pose in ("walk", "stand"):
            sw = (22 * math.sin(step) if pose == "walk" else 0.0)
            for s_, a_, bend in ((-1, sw, max(0, -sw) * 0.9 + 4), (1, -sw, max(0, sw) * 0.9 + 4)):
                hp = (s_ * 0.095 * k, 0.0, hip)
                kn = tuple(Vector(hp) + Vector(rot_x((0, 0, -thigh), -a_)))
                an = tuple(Vector(kn) + Vector(rot_x((0, 0, -shin), -a_ + bend)))
                legs.append((hp, kn, an))
            for s_, a_ in ((-1, -sw * 0.8), (1, sw * 0.8)):
                sp = (s_ * 0.2 * k, 0.0, sh_z)
                el = tuple(Vector(sp) + Vector(rot_x((s_ * 0.03 * k, 0, -uarm), -a_)))
                wr = tuple(Vector(el) + Vector(rot_x((s_ * 0.01 * k, 0, -farm), -a_ - 12)))
                arms.append((sp, el, wr))
            pelvis_z, torso_lean = hip, 2.0 + lean
        else:
            # seated: thighs forward (-Y), shins down to the floor or the foot pegs
            sz = seat + 0.02 * k
            for s_ in (-1, 1):
                hp = (s_ * 0.1 * k, 0.0, sz + 0.06 * k)
                kn = (s_ * 0.12 * k, -thigh * 0.97, sz + 0.07 * k)
                if pose == "ride":
                    an = (s_ * 0.16 * k, -thigh * 0.97 + 0.12, max(0.32, sz - shin * 0.8))
                else:
                    an = (s_ * 0.12 * k, -thigh * 0.97 - 0.05, max(0.07, sz + 0.07 * k - shin))
                legs.append((hp, kn, an))
            pelvis_z = sz + 0.06 * k
            torso_lean = 8.0 + lean
            reach = 0.42 * k if pose == "ride" else 0.36 * k
            for s_ in (-1, 1):
                sp_l = Vector((s_ * 0.2 * k, 0.0, pelvis_z + 0.49 * k))
                sp_l = Vector(rot_x(tuple(sp_l - Vector((0, 0, pelvis_z))), torso_lean)) + Vector((0, 0, pelvis_z))
                hand = Vector((s_ * (0.2 if pose == "ride" else 0.16) * k, -reach - 0.08, pelvis_z + (0.32 if pose == "ride" else 0.24) * k))
                el = sp_l.lerp(hand, 0.5) + Vector((s_ * 0.06 * k, 0.05, -0.12 * k))
                arms.append((tuple(sp_l), tuple(el), tuple(hand)))
        # legs and shoes
        for i, (hp, kn, an) in enumerate(legs):
            limb(name + "_thigh%d" % i, hp, kn, 0.075 * k, 0.058 * k, mp, P)
            limb(name + "_shin%d" % i, kn, an, 0.056 * k, 0.04 * k, mp, P)
            sphere(name + "_knee%d" % i, 0.058 * k, kn, mp, 10, parent=P)
            boxc(name + "_shoe%d" % i, (an[0], an[1] - 0.06 * k, max(0.035 * k, an[2] - 0.04 * k)), (0.095 * k, 0.26 * k, 0.075 * k),
                 mshoe, bevel=0.025 * k, parent=P)
        # pelvis, torso, neck, head, hair
        T = group(name + "_torso", (0, 0, pelvis_z), 0, P)
        T.rotation_euler = (math.radians(-torso_lean), 0, 0)
        sphere(name + "_pelvis", 1.0, (0, 0, 0.0), mp, 16, scale=(0.17 * k, 0.11 * k, 0.12 * k), parent=T)
        sphere(name + "_chest", 1.0, (0, 0.0, 0.27 * k), ms, 18, scale=(0.19 * k, 0.115 * k, 0.27 * k), parent=T)
        limb(name + "_neck", (0, 0, 0.5 * k), (0, 0, 0.6 * k), 0.05 * k, 0.045 * k, mk, T)
        sphere(name + "_head", 1.0, (0, -0.005, 0.7 * k), mk, 18, scale=(0.088 * k, 0.1 * k, 0.115 * k), parent=T)
        sphere(name + "_hair", 1.0, (0, 0.018 * k, 0.735 * k), mh, 18, scale=(0.094 * k, 0.1 * k, 0.1 * k), parent=T)
        if bag:
            boxc(name + "_bag", (0.0, 0.14 * k, 0.3 * k), (0.28 * k, 0.12 * k, 0.36 * k), mat_plain("bag_" + bag, bag, 0.6),
                 bevel=0.03 * k, parent=T)
        # arms: shoulders follow the torso for the walking poses
        for i, (sp, el, wr) in enumerate(arms):
            if pose in ("walk", "stand"):
                sp_ = Vector(rot_x(tuple(Vector(sp) - Vector((0, 0, pelvis_z))), torso_lean)) + Vector((0, 0, pelvis_z))
                el = tuple(Vector(el) + (sp_ - Vector(sp)))
                wr = tuple(Vector(wr) + (sp_ - Vector(sp)))
                sp = tuple(sp_)
            sphere(name + "_shoulder%d" % i, 0.062 * k, sp, ms, 10, parent=P)
            limb(name + "_uarm%d" % i, sp, el, 0.055 * k, 0.045 * k, ms, P)
            limb(name + "_farm%d" % i, el, wr, 0.042 * k, 0.034 * k, mk, P)
            sphere(name + "_hand%d" % i, 0.043 * k, wr, mk, 10, parent=P)
        if vel is not None:
            v = Vector(vel) / 24.0
            base = Vector(loc)
            for fr, off in ((0, -v), (2, v)):
                P.location = base + off
                P.keyframe_insert("location", frame=fr)
            P.location = base
            if P.animation_data and P.animation_data.action:
                try:
                    for fc in P.animation_data.action.fcurves:
                        for kp in fc.keyframe_points:
                            kp.interpolation = "LINEAR"
                except Exception:
                    pass
        for o in bpy.data.objects:
            if o.name.startswith(name + "_") or o == P:
                o["sv_probe"] = "person"
        return P

    # =========================================================================
    # Presets
    # =========================================================================
    def sign_draw(fallback, key="sign_deck"):
        """With a sign deck set (the skytrain street), words come off a shuffled deck, so no two signs on screen
        repeat: a refill puts the words used in the last dozen draws at its end. The main random sequence still draws
        its choice, so the street's layout stays as it was."""
        deck = C.get(key)
        if deck is None:
            return fallback
        if not deck["left"]:
            fresh = list(deck["all"])
            deck["rng"].shuffle(fresh)
            recent = [w_ for w_ in fresh if w_[0] in deck["last"]]
            deck["left"] = [w_ for w_ in fresh if w_[0] not in deck["last"]] + recent
        w_ = deck["left"].pop(0)
        deck["last"] = (deck["last"] + [w_[0]])[-deck.get("keep", 12):]
        return w_

    def soi_signs(name, P, w, ground_h, rng, near=False):
        """Each unit signs itself its own way: a flush lightbox over the shutter, a painted fascia, a box sign
        sticking out over the pavement, sometimes two. Sizes and heights vary; the words fill two thirds of each."""
        style = rng.choices(["flush", "painted", "project", "flush+project", "painted+project"], [0.32, 0.2, 0.18, 0.18, 0.12])[0]
        words = sign_draw(rng.choice(SIGN_WORDS))
        if "flush" in style or "painted" in style:
            span = rng.uniform(0.58, 1.0) * (w - 0.6)
            x0 = 0.3 + rng.uniform(0.0, (w - 0.6) - span)
            hgt = rng.uniform(0.55, 0.95)
            z0 = rng.uniform(3.05, ground_h - 0.12 - hgt)
            sign_panel(name + "_sg", x0, x0 + span, z0, z0 + hgt, -0.02, words, "flush" in style, P,
                       depth=rng.uniform(0.08, 0.18), fill=rng.uniform(0.6, 0.7))
        if "project" in style:
            w2 = sign_draw(rng.choice(SIGN_WORDS))
            zc = ground_h + rng.uniform(0.3, 1.4)
            out_ = rng.uniform(0.9, 1.3)
            hh = rng.uniform(0.6, 0.85)
            xs = rng.choice([0.4, w - 0.4])
            bg, fg = w2[1], w2[2]
            lum = sum(w_ * c_ for w_, c_ in zip((0.2126, 0.7152, 0.0722), lin(bg)))
            pm = mat_lightbox("signbg_" + bg, bg, min(1.6, 0.45 / max(lum, 0.1)))
            box(name + "_pj", xs - 0.09, xs + 0.09, -0.3 - out_, -0.3, zc - hh / 2, zc + hh / 2, pm, 0.01, parent=P)
            box(name + "_pjarm", xs - 0.025, xs + 0.025, -0.3, 0.0, zc + hh / 2 - 0.08, zc + hh / 2 - 0.02,
                mat_metal("bracket", "#4a4a48", 0.6), parent=P)
            tm = mat_emit("signfg_" + fg, fg, 5.0)
            for sd in (-1, 1):
                t = text3d(name + "_pjt%d" % sd, w2[0], hh * 0.5, (xs + sd * 0.095, -0.3 - out_ / 2, zc - hh * 0.04), (90, 0, 90 * sd),
                           tm, 0.004, font=load_font(FONT_SIGN_TH), parent=P)
                if t is not None:
                    bpy.context.view_layer.update()
                    k = min(out_ * 0.66 / max(t.dimensions.x, 1e-3), hh * 0.7 / max(t.dimensions.y, 1e-3))
                    t.scale = (k, k, k)

    SKY = {"cam": (float(os.environ.get("SKY_CX", "6.4")), float(os.environ.get("SKY_CY", "-5.0"))),
           "cz": float(os.environ.get("SKY_CZ", "1.05")), "far_back": float(os.environ.get("SKY_FAR", "12.0")),
           "yaw": float(os.environ.get("SKY_YAW", "-9.0")), "pitch": float(os.environ.get("SKY_PITCH", "4.5")),
           "train_x": float(os.environ.get("SKY_TX", "-3.6")), "theta": float(os.environ.get("SKY_TH", "4.0"))}

    # the placed puddles (world metres: x, y, rx, ry): one in front of the lens where the board's lower half mirrors,
    # one across the cross road's centre line that takes the shop signs, two small ones further up
    SKY_PUDDLES = [(5.2, 0.8, 2.6, 1.5), (2.4, 6.5, 2.2, 1.2), (-2.5, 11.0, 1.6, 0.8), (6.6, 15.5, 1.2, 0.6)]

    def skytrain_sign_pass(cam):
        """Sign clean-up seen from the lens: the projecting sign at the top right reads 'ร้านขายยา' in full (the balcony
        rail that cut through its letters goes), and no projecting box sign may cross a big blade sign on screen:
        each one that does slides along the street until it clears, or comes down."""
        bpy.context.view_layer.update()
        objs = bpy.data.objects
        top = [o for o in objs if o.name.startswith("sh_1_19_pj")]
        if top:
            green = mat_lightbox("signbg_#1f7a46", "#1f7a46", 1.4)
            white = mat_emit("signfg_#ffffff", "#ffffff", 5.0)
            panel = objs.get("sh_1_19_pj")
            hh = panel.dimensions.z if panel else 0.7
            out_ = panel.dimensions.y if panel else 1.0
            for o in top:
                if o.type == "FONT":
                    o.data.body = "ร้านขายยา"
                    o.data.materials.clear()
                    o.data.materials.append(white)
                    o.scale = (1, 1, 1)
                    bpy.context.view_layer.update()
                    k = min(out_ * 0.7 / max(o.dimensions.x, 1e-3), hh * 0.62 / max(o.dimensions.y, 1e-3))
                    o.scale = (k, k, k)
                elif o.type == "MESH" and o.name == "sh_1_19_pj":
                    o.data.materials.clear()
                    o.data.materials.append(green)
            for o in list(objs):
                if any(o.name.startswith("sh_1_19_" + p_) for p_ in ("bal1_", "btop1", "balc1", "cloth1_", "bpot1_", "bleaf1_")):
                    bpy.data.objects.remove(o, do_unlink=True)
        bpy.context.view_layer.update()

        def screen_box(prefix):
            bb = None
            for o in objs:
                if o.name.startswith(prefix) and o.type in ("MESH", "FONT"):
                    b = px_bbox(o, cam)
                    if b:
                        bb = list(b) if bb is None else [min(bb[0], b[0]), min(bb[1], b[1]), max(bb[2], b[2]), max(bb[3], b[3])]
            return bb

        def overlap(a, b, pad=12):
            return a and b and a[0] < b[2] + pad and b[0] < a[2] + pad and a[1] < b[3] + pad and b[1] < a[3] + pad

        blades = [screen_box("bigblade%d" % k) for k in range(2)]
        groups = sorted({o.name.split("_pj")[0] for o in objs if o.name.startswith("sh_1_") and "_pj" in o.name})
        for gname in groups:
            pre = gname + "_pj"
            box_ = screen_box(pre)
            if not any(overlap(box_, b) for b in blades):
                continue
            members = [o for o in objs if o.name.startswith(pre)]
            moved = False
            for dy in (-1.5, -2.0, -2.5, -3.0, 1.5, 2.0):
                for o in members:
                    o.matrix_world = Matrix.Translation((0, dy, 0)) @ o.matrix_world
                bpy.context.view_layer.update()
                if not any(overlap(screen_box(pre), b) for b in blades):
                    print("SV_SIGN moved %s by %.1f m along the street" % (pre, dy))
                    moved = True
                    break
                for o in members:
                    o.matrix_world = Matrix.Translation((0, -dy, 0)) @ o.matrix_world
                bpy.context.view_layer.update()
            if not moved:
                for o in members:
                    bpy.data.objects.remove(o, do_unlink=True)
                print("SV_SIGN removed %s (crossed a blade sign wherever it went)" % pre)
        bpy.context.view_layer.update()

    def scene_skytrain():
        """A cross road meets the main road under the elevated railway, 7 pm, just after rain. The billboard hangs on
        the near face of the viaduct with its catwalk below it, the six floodlights above; a two-car train stands
        above it. The road is lived in: open shopfronts throw light on the pavement, motorbike taxis wait, a food
        cart's bare bulb burns, the kerb collects stools, crates and barrels."""
        rng = random.Random(42)
        C["sign_deck"] = {"all": SIGN_WORDS + SIGN_WORDS_MORE, "left": [], "last": [], "rng": random.Random(1010)}
        C["upper_deck"] = {"all": UPPER_SIGNS + UPPER_SIGNS_MORE, "left": [], "last": [], "rng": random.Random(2020), "keep": 6}
        asphalt = mat_street_wet("asphalt_v3", puddles=SKY_PUDDLES, noise_puddle=float(os.environ.get("SKY_PUD", "0.30")))
        side = mat_tiles("sidewalk", "#8f897e", "#7d776c", "#4d4a45", 0.3, 0.006, 0.45, 0.3, wet=0.7)
        curb = mat_surface("curb", "#a19e96", "#6f6c65", 4, (0.45, 0.8), 0.15, grime=0.3)
        concrete = mat_surface("viaduct_concrete", "#b7b3aa", "#7d7972", 0.6, (0.7, 0.92), 0.25, streaks=0.55, grime=0.0)
        paint = mat_surface("road_paint", "#d9d6cc", "#a9a69c", 8, (0.35, 0.6), 0.05)
        yellow = mat_surface("road_yellow", "#d9a62b", "#a87f20", 8, (0.35, 0.6), 0.05)
        steel = mat_metal("bb_steel", "#3a3e41", 0.45)
        cab = mat_plain("cable", "#0e0e0e", 0.55)
        box("ground", -400, 400, -200, 500, -0.3, 0.0, asphalt)
        # a damp, low-gloss sheen on the road under the board: the bright board draws its streak down the wet asphalt
        m, g, out = new_mat("road_sheen")
        nz = g.noise(g.coords("Object"), 0.8, 6, 0.6)
        agg = g.noise(g.coords("Object"), 60.0, 10.0, 0.7)
        sh = principled(g, g.ramp(agg, lin("#1d1d1e"), lin("#2a2a2b"), 0.35, 0.65), g.maprange(nz, 0.3, 0.7, 0.06, 0.2), 0.0, 0.6,
                        normal=g.bump(agg, 0.15, 0.004))
        sheen = finish(m, g, out, sh, lin("#222222"))
        pv = []
        for k in range(28):
            a_ = 2 * math.pi * k / 28
            r_ = 1.0 + 0.18 * math.sin(3 * a_ + 1.0) + 0.1 * math.sin(7 * a_)
            pv.append((r_ * math.cos(a_) * 2.8, r_ * math.sin(a_) * 7.5, 0.0015))
        # (the road's own damp sheen carries the streak; no separate patch)

        RW, BX = 8.5, 11.5
        for xs in (-1, 1):
            box("x_side_%d" % xs, xs * RW, xs * BX, -120, 24.0, 0.0, 0.15, side)
            box("x_curb_%d" % xs, xs * (RW - 0.15), xs * RW, -120, 24.0, 0.0, 0.16, curb, 0.01)
        for i in range(0, 30):
            y0 = -6.0 + i * 1.0
            for xs in (-1, 1):
                box("kerb_%d_%d" % (i, xs), xs * (RW - 0.17), xs * (RW - 0.13), y0, y0 + 1.0, 0.0, 0.165,
                    mat_plain("kerb_red" if i % 2 else "kerb_white", "#a8352b" if i % 2 else "#e2ded3", 0.5))
        y = -110.0
        while y < 20:
            box("xdash", -0.075, 0.075, y, y + 3.0, 0.0, 0.006, paint)
            y += 9.0
        box("x_stop", -RW + 0.2, 0.0, 20.6, 21.0, 0.0, 0.006, paint)
        for k, xx in enumerate((-6.2, -2.4)):
            box("arrow%d" % k, xx - 0.12, xx + 0.12, 12.0, 16.0, 0.0, 0.006, paint)
        cyl("manhole", 0.38, 0.02, (2.6, 9.0, 0.0), mat_metal("manhole", "#3a3936", 0.35), 32)

        # ---- the cross road's shophouses: bay widths, setbacks, heights, open fronts, signs all varied
        walls = ["#d8d0bf", "#cfc6b0", "#e2dccd", "#c9c2b4", "#d6cfbd", "#bfb8a8", "#d9c9a8", "#c6cfc8",
                 "#9fb8b0", "#d9b8a8", "#c9cfa8", "#e0c98f", "#a9b4bd"]
        shades = ["#7f8a86", "#8e8a7f", "#6f7f7a", "#9aa19c", "#5f6e78"]
        awnings = [None, None, None, "#2f6f5e", "#a33a2c", "#355a8a", "#c9c3b5"]
        for side_ in (-1, 1):
            y = -70.0
            u = 0
            while y < 23.5:
                wdt = min(rng.uniform(3.6, 4.5), 23.6 - y)
                if wdt < 2.5:
                    break
                near_mouth = y > 10.0
                fl = rng.choice([2, 3, 3]) if near_mouth else rng.choice([3, 4, 4, 5])
                st = "open" if rng.random() < 0.36 else rng.choice(["closed", "half", "closed"])
                if side_ > 0 and 8.0 < y < 23.0:
                    st = "open"          # the units beside the camera are trading: their light falls on the pavement
                sb = rng.uniform(0.0, 0.4)
                if side_ < 0:
                    origin, rot = (-BX - sb, y), 90
                else:
                    origin, rot = (BX + sb, y + wdt), -90
                gh = rng.uniform(3.9, 4.4)
                P_ = shophouse("sh_%d_%d" % (side_, u), origin, rot, wdt, floors=fl, ground_h=gh, wall=rng.choice(walls),
                               shop=st, sign=None, rng=rng, shutter_color=rng.choice(shades),
                               interior_temp=rng.choice([2700, 3000, 3200, 4200]), interior_power=rng.choice([160, 220, 300]),
                               lit_windows=0.42, awning=rng.choice(awnings), blade=None, vary=True,
                               spill=rng.choice([90, 140, 200]) if st == "open" else 0.0)
                soi_signs("sh_%d_%d" % (side_, u), P_, wdt, gh, rng)
                y += wdt
                u += 1
        # two big blade signs, both lit: a hotel and a clinic, each 7.5 m tall and 1.8 m out from the wall
        for k, (yy, words) in enumerate(((19.0, ("โรงแรม", "#1b1b1b", "#f2e6c8")), (22.6, ("คลินิก", "#ffffff", "#1f7a46")))):
            G_ = group("bigblade%d" % k, (BX, yy, 0), -90)
            blade_sign("bigblade%d_s" % k, 0.5, 3.6, 8.9, words, True, G_, out=2.0, thick=0.3)
        # the street, lived in, without people near the lens: a parked motorbike with a helmet on its mirror, the food
        # cart under its red canopy with one bare 2300 K bulb, stacks of plastic stools, the kerb's clutter
        motorbike_parked("bike_parked", (9.75, 15.2, 0.15), 62, "#b8241c", helmet="#f2efe6", lean=7.0)
        motorbike_parked("bike_parked2", (10.55, 13.4, 0.15), 70, "#1d1f22", helmet="#2c5aa0", lean=6.0)
        food_cart("cart", (10.0, 20.2, 0.15), 92)
        stool_stack("stools_red", (9.35, 18.6, 0.15), 6, "#c8261c")
        stool_stack("stools_blue", (9.75, 18.25, 0.15), 4, "#2c5aa0")
        plastic_stool("cart_stool1", (9.2, 21.4, 0.15), "#c8261c")
        plastic_stool("cart_stool2", (9.55, 22.1, 0.15), "#c8261c")
        kerb_clutter("clut1", 10.6, 9.0, rng, 1)
        kerb_clutter("clut2", -10.2, 17.0, rng, -1)
        for k, yy in enumerate((3.0, 5.0)):
            motorbike("parked%d" % k, (RW - 0.6, yy, 0.0), 5 + 8 * k, ["#2a62a8", "#e3e3e0"][k])

        # ---- the main road and the viaduct, a few degrees off square
        M = group("main_road", (0, 40.0, 0), SKY["theta"])
        box("main_side_n", -200, 200, 12.0, 16.0, 0.0, 0.15, side, parent=M)
        box("main_side_s", -200, 200, -16.0, -12.0, 0.0, 0.15, side, parent=M)
        box("main_curb_n", -200, 200, 11.85, 12.0, 0.0, 0.16, curb, parent=M)
        box("main_curb_s", -200, 200, -12.0, -11.85, 0.0, 0.16, curb, parent=M)
        for yy in (-8.0, -4.5, 4.5, 8.0):
            x = -150.0
            while x < 150:
                box("dash", x, x + 3.0, yy - 0.075, yy + 0.075, 0.0, 0.006, paint, parent=M)
                x += 9.0
        for yy in (-2.2, 2.2):
            box("edge_line", -200, 200, yy - 0.08, yy + 0.08, 0.0, 0.006, yellow, parent=M)
        for k in range(14):
            xx = -7.5 + k * 1.1
            box("zebra%d" % k, xx, xx + 0.55, -11.6, -7.6, 0.0, 0.006, paint, parent=M)
        box("median", -200, 200, -2.0, 2.0, 0.0, 0.3, curb, parent=M)
        x = -120.0
        while x < 120:
            clipped_shrub("hedge%d" % int(x), (x, 0, 0.62), (rng.uniform(3, 5), 2.6, 0.6), rng, density=500.0, leaf=(0.07, 0.035))
            bpy.context.view_layer.update()
            x += rng.uniform(6, 9)
        for o in bpy.data.objects:
            if o.name.startswith("hedge") and o.parent is None:
                mw = o.matrix_world.copy()
                o.parent = M
                o.matrix_world = M.matrix_world @ mw
        # the deck: rail level 10.8 m, a low parapet, the underside at 9.0 m
        dz = -1.6
        prof = [(-2.6, 10.6 + dz), (2.6, 10.6 + dz), (4.3, 11.8 + dz), (5.0, 11.95 + dz), (5.0, 13.2 + dz), (4.75, 13.2 + dz),
                (4.75, 12.4 + dz), (-4.75, 12.4 + dz), (-4.75, 13.2 + dz), (-5.0, 13.2 + dz), (-5.0, 11.95 + dz), (-4.3, 11.8 + dz)]
        for seg in range(-200, 200, 18):
            prism_x("deck_%d" % seg, prof, seg + 0.02, seg + 17.98, concrete, parent=M, bevel=0.02)
        cap = [(-1.2, 9.2 + dz), (1.2, 9.2 + dz), (3.4, 10.05 + dz), (3.4, 10.6 + dz), (-3.4, 10.6 + dz), (-3.4, 10.05 + dz)]
        # pier spacing 36 m; one pier stands just left of the board, clear of the signs, so the deck reads as raised
        for px in (-48, -12, 24, 60, 96):
            boxc("pier_%d" % px, (px, 0, (9.4 + dz) / 2), (2.4, 1.8, 9.4 + dz), concrete, bevel=0.45, parent=M)["sv_probe"] = "pier_%d" % px
            prism_x("pier_cap_%d" % px, cap, px - 1.3, px + 1.3, concrete, parent=M, bevel=0.04)
        for px in range(-75, 90, 18):
            boxc("udl_%d" % px, (px + 9, -2.0, 10.52 + dz), (0.6, 0.3, 0.12), mat_emit("udl", None, 30.0, 2700), parent=M)
        bpy.context.view_layer.update()
        for px in range(-39, 54, 18):
            wp = M.matrix_world @ Vector((px + 9, -2.0, 10.4 + dz))
            light("under_%d" % px, "SPOT", tuple(wp), 700, temp=2700, target=(wp.x, wp.y - 1, 0), spot_deg=110, blend=0.8, shadow_soft=0.2)

        # THE BILLBOARD on the deck's near face, the catwalk hung under its bottom edge, six floodlights above
        bw, bh, bz0 = 14.0, 5.25, 5.9
        bx0 = -bw / 2
        box("bb_cabinet", bx0 - 0.25, bx0 + bw + 0.25, -5.35, -5.0, bz0 - 0.25, bz0 + bh + 0.25, steel, 0.02, parent=M)
        for (a_, b_, c_, d_) in ((bx0 - 0.25, bx0 + bw + 0.25, bz0 + bh, bz0 + bh + 0.25),
                                 (bx0 - 0.25, bx0 + bw + 0.25, bz0 - 0.25, bz0),
                                 (bx0 - 0.25, bx0, bz0, bz0 + bh), (bx0 + bw, bx0 + bw + 0.25, bz0, bz0 + bh)):
            box("bb_lip", a_, b_, -5.43, -5.35, c_, d_, steel, 0.005, parent=M)
        for xx in (bx0 + 1.5, bx0 + bw / 2, bx0 + bw - 1.5):
            box("bb_hanger%.1f" % xx, xx - 0.12, xx + 0.12, -5.0, -3.6, bz0 + bh - 0.3, 10.6 + dz, steel, parent=M)
        design_face("billboard", (bx0 + bw / 2, -5.4, bz0 + bh / 2), bw, bh, kind="printed", surface="vinyl",
                    label="Billboard 14 x 5.25 m", parent=M)
        # the catwalk: grating on brackets 1.2 m under the board, its railing stops below the bottom edge
        zw = bz0 - 1.35
        box("bb_walk", bx0 - 0.2, bx0 + bw + 0.2, -6.3, -5.0, zw - 0.06, zw, mat_corrugated("grating", "#4b4f50", 0.03, axis="X"), parent=M)
        for k in range(8):
            xx = bx0 + k * bw / 7
            beam("bb_bracket%d" % k, (xx, -5.05, zw - 0.05), (xx, -5.05, bz0 - 0.3), 0.035, steel, 8, parent=M)
            beam("bb_strut%d" % k, (xx, -6.25, zw - 0.05), (xx, -5.05, bz0 - 0.35), 0.03, steel, 8, parent=M)
        for i in range(int(bw / 0.7) + 1):
            box("bb_rail", bx0 - 0.2 + i * 0.7, bx0 - 0.17 + i * 0.7, -6.3, -6.27, zw, zw + 0.95, steel, parent=M)
        box("bb_railtop", bx0 - 0.2, bx0 + bw + 0.2, -6.32, -6.25, zw + 0.92, zw + 0.97, steel, parent=M)
        NL = 6
        for i in range(NL):
            lx = bx0 + 0.9 + i * (bw - 1.8) / (NL - 1)
            beam("bb_arm%d" % i, (lx, -5.35, bz0 + bh + 0.2), (lx, -7.35, bz0 + bh + 0.55), 0.04, steel, parent=M)
            boxc("bb_lamp%d" % i, (lx, -7.45, bz0 + bh + 0.52), (0.55, 0.32, 0.16), steel, rot=(math.radians(-58), 0, 0), bevel=0.02, parent=M)
            boxc("bb_lens%d" % i, (lx, -7.4, bz0 + bh + 0.43), (0.45, 0.22, 0.01), mat_emit("bb_lens", None, 80.0, 3800),
                 rot=(math.radians(-58), 0, 0), parent=M)
        bpy.context.view_layer.update()
        for i in range(NL):
            lx = bx0 + 0.9 + i * (bw - 1.8) / (NL - 1)
            wp = M.matrix_world @ Vector((lx, -7.45, bz0 + bh + 0.36))
            tp = M.matrix_world @ Vector((lx, -5.4, bz0 + 1.6))
            # warm and wide, overlapping: an even wash a touch brighter at the board's top, falling off downward
            light("bb_flood%d" % i, "SPOT", tuple(wp), 1300, temp=3800, target=tuple(tp), spot_deg=150, blend=1.0, shadow_soft=0.3)
        # a denser pocket of haze in front of the board: the floodlights draw their cones in it
        hz = boxc("bb_haze", (0, -8.5, bz0 + bh / 2 + 0.6), (bw + 6, 6.0, bh + 4.0), mat_haze("bb_haze_m", 0.04, (0.95, 0.9, 0.82), 0.7),
                  collection=coll("SV_atmosphere"), parent=M)
        hz["sv_mask_ignore"] = True
        hz.visible_shadow = False

        # the train: two cars on the near track, standing in the station approach, lit inside
        interior = train_interior_image("train_interior")
        tx = SKY["train_x"]
        L_ = 13.0
        z0 = 10.8 + 0.55
        train_car("car_a", tx - L_ - 0.3, L_, -2.3, z0, M, interior, nose=-1)
        train_car("car_b", tx + 0.3, L_, -2.3, z0, M, interior, nose=1)
        box("gangway", tx - 0.3, tx + 0.3, -3.5, -1.1, z0 + 0.2, z0 + 3.0, mat_plain("bellows", "#1a1c1e", 0.7), parent=M)
        box("rail_n", -200, 200, -3.1, -3.0, 10.8, 10.95, mat_metal("rail", "#5a5c5c", 0.3), parent=M)
        box("rail_s", -200, 200, -1.6, -1.5, 10.8, 10.95, mat_metal("rail", "#5a5c5c", 0.3), parent=M)

        # far side of the main road: a service lane, then a shophouse row with lit shops, then towers
        FB = SKY["far_back"]
        box("main_side_far", -200, 200, 13.6 + FB, 16.0 + FB, 0.0, 0.15, side, parent=M)
        box("main_curb_far", -200, 200, 13.45 + FB, 13.6 + FB, 0.0, 0.16, curb, parent=M)
        for lx in range(-80, 90, 22):
            street_lamp("lamp_f%d" % lx, lx + 7, 14.0 + FB, 7.5, 1.6, -90, 2700, 380, parent=M)
        y = -60.0
        u = 0
        while y < 50:
            wdt = rng.uniform(3.6, 4.5)
            words = rng.choice(SIGN_WORDS)
            words = sign_draw(words)
            fl_ = rng.choice([3, 4, 4, 5, 2])
            st_ = rng.choice(["open", "open", "half", "closed"])
            tk_ = rng.choice([3000, 4200, 5600])
            # pushed back across a service lane, kept low (two or three floors) so the sky and the haze show between
            # the roofs and the deck, and lit warm inside: the piers stand against it and the viaduct reads as raised
            Pf = shophouse("far_%d" % u, (y, 16.0 + FB + random.Random(u * 7 + 1).uniform(0.0, 0.4)), 0, wdt,
                           floors=min(fl_, 3) if fl_ != 2 else 2,
                           wall=rng.choice(walls), shop="open" if st_ != "closed" or u % 3 else "closed", sign=None, sign_lit=True,
                           rng=rng, interior_temp=[2700, 3000, 3200][u % 3] if tk_ else 3000, interior_power=260, lit_windows=0.45,
                           awning=rng.choice(awnings), blade=None, parent=M, interior=True, ac=True, vary=True)
            for o_ in Pf.children_recursive:
                o_["sv_probe"] = "far_row"
            # each unit signs itself: heights 0.5 to 0.9 m, spans and positions vary, about a third unlit, and the unit
            # under the middle of the board has a dark fascia with painted letters
            sr = random.Random(900 + u)
            gh_ = Pf.get("ground_h", 4.0)
            if -3.0 < y + wdt / 2 < 1.5:
                hgt = 0.62
                span = (wdt - 0.6) * 0.92
                x0 = 0.3 + (wdt - 0.6 - span) / 2
                sign_panel("far_%d_sg" % u, x0, x0 + span, 3.1, 3.1 + hgt, -0.02, ("ร้านข้าวต้ม", "#1c1d1c", "#e9dcb6"), False, Pf,
                           depth=0.05, font=load_font(FONT_SIGN2), fill=0.62)
            elif sr.random() < 0.88:
                hgt = sr.uniform(0.5, 0.9)
                hgt = min(hgt, gh_ - 0.15 - 3.05)
                span = sr.uniform(0.45, 0.95) * (wdt - 0.6)
                x0 = 0.3 + sr.uniform(0.0, (wdt - 0.6) - span)
                z0 = 3.05 + sr.uniform(0.0, max(0.0, gh_ - 0.15 - 3.05 - hgt))
                lit_ = sr.random() < 0.64
                sign_panel("far_%d_sg" % u, x0, x0 + span, z0, z0 + hgt, -0.02, words, lit_, Pf,
                           depth=sr.uniform(0.05, 0.18), fill=sr.uniform(0.58, 0.7))
            y += wdt
            u += 1
        for lx in range(-90, 100, 30):
            street_lamp("lamp_n%d" % lx, lx, 12.6, 9.5, 2.4, -90, 3000, 650, parent=M)
            street_lamp("lamp_s%d" % lx, lx + 15, -12.6, 9.5, 2.4, 90, 3000, 650, parent=M)
        # three thin light trails of different brightness, not a band: a bright tail-light pair in the near lane, a
        # dimmer white headlight streak on the far side, a faint amber motorbike line
        tail = mat_emit("trail_tail", "#ff2a12", 7.0)
        head = mat_emit("trail_head", None, 3.2, 4600)
        amber = mat_emit("trail_amber", "#ffb030", 1.8)
        beam("trail_t1", (-70.0, -9.4, 0.78), (46.0, -9.4, 0.78), 0.010, tail, 6, parent=M)
        beam("trail_t2", (-62.0, -8.0, 0.76), (40.0, -8.0, 0.76), 0.008, tail, 6, parent=M)
        beam("trail_h1", (-90.0, 6.6, 0.68), (90.0, 6.6, 0.68), 0.008, head, 6, parent=M)
        beam("trail_bike", (-35.0, -5.6, 1.08), (22.0, -5.6, 1.08), 0.006, amber, 6, parent=M)
        for o in bpy.data.objects:
            if o.name.startswith("trail_"):
                o.visible_shadow = False
        for k in range(46):
            ang = rng.uniform(-1.05, 1.05)
            d = rng.uniform(230, 700)
            x0 = -4 + d * math.sin(ang)
            y0 = -20 + d * math.cos(ang)
            wdt = rng.uniform(22, 45)
            elev = rng.uniform(4.0, 10.0) if abs(ang) < 0.45 else rng.uniform(6.0, 15.0)
            h = d * math.tan(math.radians(elev))
            tower("tower%d" % k, x0, x0 + wdt, y0, y0 + rng.uniform(18, 34), h,
                  rng.choice(["#8f989c", "#a6adaf", "#7b878c", "#b2b4ae", "#6f7b80", "#c2bba9"]),
                  bay=rng.choice([1.6, 2.0, 2.4]), floor_h=rng.choice([3.3, 3.6]), lit=rng.uniform(0.12, 0.32),
                  seed=10 + k, strength=rng.uniform(1.2, 2.6), band=rng.random() < 0.35,
                  crown_light=rng.choice([None, None, None, None, "#ffffff", "#9fdcff"]))
        traffic_signal("signal_l", -RW - 0.6, 21.0, 0, 3.5, "red")
        pole_m = mat_surface("pole_conc", "#a7a39a", "#6e6a62", 3, (0.7, 0.9), 0.1, streaks=0.3)
        prev = None
        for i, py in enumerate((-36, -22, -8, 6, 20)):
            px = -RW - 0.55
            cyl("epole%d" % i, 0.17, 11.0, (px, py, 5.5), pole_m, 12, r2=0.11)
            boxc("ecross%d" % i, (px, py, 9.8), (1.8, 0.14, 0.14), pole_m)
            if i % 2:
                boxc("etrafo%d" % i, (px + 0.4, py, 7.6), (0.55, 0.55, 1.0), mat_metal("trafo", "#69706c", 0.5), bevel=0.03)
            if prev is not None:
                for j, (dx, dz_) in enumerate(((-0.8, 9.85), (-0.35, 9.85), (0.35, 9.85), (0.8, 9.85), (0.0, 8.4), (0.2, 7.8))):
                    cable("wire%d_%d" % (i, j), (px + dx, prev, dz_), (px + dx, py, dz_), 0.35 + 0.07 * j, 0.011 + 0.007 * (j % 3), cab)
            prev = py
        for i, py in enumerate((-30, -17, -4)):
            street_lamp("xlamp%d" % i, RW + 0.6, py, 8.5, 2.0, 180, 3000, 500)
        for i, py in enumerate((-12, 2, 16)):
            street_lamp("xlampL%d" % i, -RW - 0.6, py, 8.5, 2.0, 0, 3000, 520)
        haze(0.0024, (0.75, 0.82, 0.95), (0, 40, 30), (420, 300, 70))
        world_sky(elev_deg=-3.4, rot_deg=-60, strength=2.2, air=1.0, aerosol=2.0, ozone=1.6)
        bpy.context.view_layer.update()
        cx_, cy_ = SKY["cam"]
        yaw, pit = math.radians(SKY["yaw"]), math.radians(SKY["pitch"])
        cz_ = SKY["cz"]
        loc = (cx_, cy_, cz_)
        tgt = (cx_ + math.sin(yaw) * 10, cy_ + math.cos(yaw) * 10, cz_ + math.tan(pit) * 10)
        cam = camera("SV_camera", loc, tgt, lens=35.0)
        over = camera("SV_overview", (-14.0, -34.0, 34.0), (0.0, 26.0, 6.0), lens=30.0)
        skytrain_sign_pass(cam)
        return {"camera": cam, "overview": over, "exposure": 0.3, "samples": 768,
                "finish": {"k": 0.016, "ca": 0.0012, "vignette": 0.22, "grain": 0.013, "split": 0.5},
                "glare": {"threshold": 1.2, "strength": 0.55, "size": 0.85}}

    def scene_shopfront():
        """SV Academy's shophouse unit in a quiet soi, early evening after rain: lightbox sign
        above the door, A0 poster inside the left window, warm interior."""
        rng = random.Random(11)
        asphalt = mat_wet_asphalt("asphalt", puddle=0.52, puddle_scale=0.3, wet=0.8)
        side = mat_tiles("sidewalk", "#8f897d", "#7e786c", "#4f4b45", 0.3, 0.005, 0.32, 0.3, wet=0.9)
        curb = mat_surface("curb", "#a19e96", "#6f6c65", 4, (0.45, 0.8), 0.15, grime=0.3)
        cab = mat_plain("cable", "#0e0e0e", 0.55)
        box("ground", -120, 120, -80, 60, -0.3, 0.0, asphalt)
        box("sidewalk", -60, 60, -2.4, 0.3, 0.0, 0.15, side)
        box("curb", -60, 60, -2.55, -2.4, 0.0, 0.16, curb, 0.01)
        box("sidewalk_far", -60, 60, -11.0, -8.6, 0.0, 0.15, side)
        box("curb_far", -60, 60, -8.6, -8.45, 0.0, 0.16, curb, 0.01)
        for i in range(-30, 30):
            box("kerbpaint%d" % i, i * 1.0, i * 1.0 + 1.0, -2.57, -2.53, 0.0, 0.165,
                mat_plain("kerb_red" if i % 2 else "kerb_white", "#a8352b" if i % 2 else "#e2ded3", 0.5))
        box("lane", -60, 60, -5.55, -5.45, 0.0, 0.006, mat_surface("road_paint", "#d9d6cc", "#a9a69c", 8, (0.35, 0.6), 0.05))

        walls = ["#d8d0bf", "#cfc6b0", "#e2dccd", "#c9c2b4", "#bfb8a8", "#d9c9a8"]
        # the neighbours: every sign a real, legible shop name, set clear of the awnings
        units = [(-12.7, 4.2, dict(shop="closed", sign=("ซักรีด", "#2a62a8", "#ffffff"), sign_lit=False, wall="#cdbfa3", shutter_color="#6f7f7a")),
                 (-8.5, 4.2, dict(shop="open", sign=("ร้านขายยา", "#1f7a46", "#ffffff"), sign_lit=True, interior_temp=6000, wall="#ddd5c5", awning="#2f6f5e")),
                 (-4.3, 4.2, dict(shop="half", sign=("ถ่ายเอกสาร", "#e6e6e6", "#c0151d"), sign_lit=False, wall="#e2dccd", shutter_color="#8a8f8c")),
                 (-0.1, 4.2, None),
                 (4.1, 4.2, dict(shop="open", sign=("มินิมาร์ท", "#f4c430", "#a3171b"), sign_lit=True, interior_temp=4200, wall="#c9c2b2", awning="#a33a2c", tube_k=0.45, interior_power=90)),
                 (8.3, 4.2, dict(shop="closed", sign=("ร้านทอง", "#b3161b", "#f7d26a"), sign_lit=False, wall="#d8cfbb", shutter_color="#5f6e78")),
                 (12.5, 4.2, dict(shop="open", sign=("ร้านกาแฟ", "#5b2a6e", "#f6e7c1"), sign_lit=True, interior_temp=3000, wall="#d1c8b6"))]
        legible = [("โรงแรม", "#1b1b1b", "#f2e6c8"), ("ตัดผม", "#ffffff", "#1d3f8a"), ("ร้านอาหาร", "#7a1f1f", "#ffe9b0"),
                   ("กาแฟสด", "#f2efe6", "#2c2a26"), ("ร้านยา", "#1f7a46", "#ffffff"), ("ซักรีด", "#2a62a8", "#ffffff")]
        x = 16.7
        k = 0
        while x < 70:
            wdt = rng.choice([3.8, 4.0, 4.2, 4.5])
            units.append((x, wdt, dict(shop=rng.choice(["open", "closed", "half", "open"]), sign=rng.choice(legible),
                                       sign_lit=rng.random() < 0.6, interior_temp=rng.choice([3000, 4200, 6000]),
                                       wall=rng.choice(walls), awning=rng.choice([None, None, "#2f6f5e", "#a33a2c"]))))
            x += wdt
            k += 1
        x = -16.9
        while x > -50:
            wdt = rng.choice([3.8, 4.0, 4.2])
            units.append((x - wdt + 4.2, wdt, dict(shop=rng.choice(["open", "closed", "half"]), sign=rng.choice(legible),
                                                   sign_lit=rng.random() < 0.5, wall=rng.choice(walls))))
            x -= wdt
        for x0, wdt, opt in units:
            if opt is None:
                shophouse("hero", (x0, 0.0), 0, wdt, floors=4, wall="#e7e0d1", shop="custom", sign=None, rng=rng,
                          lit_windows=0.7, shutter_color="#6c7572", ac=True)
            else:
                opt.setdefault("interior_power", 200)
                shophouse("unit%.1f" % x0, (x0, 0.0), 0, wdt, floors=rng.choice([4, 4, 5]), rng=rng, lit_windows=0.35,
                          blade=rng.choice(legible) if rng.random() < 0.35 else None, vary=True, **opt)

        # ---- the hero ground floor: SV Academy's studio, the most alive room on the street
        X0 = -0.1
        gx0, gx1 = X0 + 0.25, X0 + 3.95
        cxm = (gx0 + gx1) / 2
        open_top = 3.0
        wall_in = mat_surface("hero_wall", "#ece6da", "#d9d0c0", 2, (0.8, 0.9), 0.02)
        oak = mat_surface("oak", "#b48a5c", "#8a6440", 3, (0.35, 0.55), 0.06, stretch=(1, 10, 1))
        slat = mat_surface("slat", "#8f6a46", "#6a4a2f", 3, (0.45, 0.65), 0.08, stretch=(1, 1, 12))
        # terrazzo: marble chips of several sizes and colours in a warm grey matrix, honed, a soft sheen
        m, g, out = new_mat("terrazzo_chips")
        vec = g.coords("Object", (1, 1, 1))
        vor = g.n("ShaderNodeTexVoronoi", {"Vector": vec, "Scale": 38.0})
        vor2 = g.n("ShaderNodeTexVoronoi", {"Vector": vec, "Scale": 95.0})
        chip = g.math("LESS_THAN", vor.outputs["Distance"], 0.22)
        chip2 = g.math("LESS_THAN", vor2.outputs["Distance"], 0.18)
        mat_c = g.ramp(g.noise(vec, 1.5, 4, 0.5), lin("#cfc6b6"), lin("#d9d1c2"), 0.3, 0.7)
        cc = g.mix(g.math("GREATER_THAN", vor.outputs["Color"], 0.5), lin("#8f877a"), lin("#efe9dd"))
        col = g.mix(chip, mat_c, cc)
        col = g.mix(chip2, col, g.mix(g.math("GREATER_THAN", vor2.outputs["Color"], 0.6), lin("#5d5a54"), lin("#b9a58a")))
        rough = g.maprange(g.noise(vec, 3, 4, 0.5), 0.3, 0.7, 0.22, 0.4)
        terr = finish(m, g, out, principled(g, col, rough, 0.0, 0.5, coat=0.35, coat_rough=0.12), lin("#d4ccbd"))
        box("h_floor", gx0, gx1, 0.3, 9.0, -0.02, 0.01, terr)
        box("h_back", gx0, gx1, 8.8, 9.0, 0, open_top, wall_in)
        for k in range(18):
            xx = gx0 + 0.08 + k * 0.205
            box("h_slat%d" % k, xx, xx + 0.09, 8.66, 8.8, 0, open_top, slat)
        box("h_ceil", gx0, gx1, 0.3, 9.0, open_top - 0.06, open_top, wall_in)
        for sx in (gx0 - 0.02, gx1):
            box("h_side%.1f" % sx, sx, sx + 0.02, 0.3, 9.0, 0, open_top, wall_in)
        # the long oak workbench across the room, four laptops facing the street, two people working at it
        bench_y0, bench_y1 = 3.3, 4.15
        box("bench_top", gx0 + 0.25, gx1 - 0.25, bench_y0, bench_y1, 0.74, 0.78, oak, 0.006)
        dark = mat_metal("blackmetal", "#1a1a1a", 0.4)
        for xx in (gx0 + 0.4, gx1 - 0.4):
            box("bench_leg%.1f" % xx, xx - 0.03, xx + 0.03, bench_y0 + 0.08, bench_y1 - 0.08, 0.0, 0.74, dark)
        # screens carry real code (monospace, two indent levels, grey with green keywords), drawn by Pillow
        code_png = os.path.join(C["args"].out, "shopfront-sign", "code-screen.png")
        os.makedirs(os.path.dirname(code_png), exist_ok=True)
        try:
            # Pillow draws it. On a Mac without the command line tools, /usr/bin/python3 would offer to install them
            # in a window, so python3 is only called when they are there.
            if sys.platform == "darwin" and subprocess.run(["xcode-select", "-p"], capture_output=True).returncode != 0:
                raise RuntimeError("no command line tools")
            subprocess.run(["python3", os.path.abspath(__file__), "codetex", code_png], check=True, capture_output=True)
            code_img = bpy.data.images.load(code_png)
        except Exception:
            print("SV_WARN no python3 with Pillow: the shopfront's screens stay plain dark")
            code_img = bpy.data.images.new("code-screen", 64, 64)
            code_img.generated_color = (0.02, 0.03, 0.025, 1.0)

        def code_mat(name, strength=2.0, crop=(0.0, 0.0, 1.0, 1.0)):
            m, g, out = new_mat(name)
            tc = g.n("ShaderNodeTexCoord")
            sep = g.n("ShaderNodeSeparateXYZ")
            g.set(sep.inputs[0], tc.outputs["Generated"])
            u = g.math("ADD", g.math("MULTIPLY", sep.outputs[0], crop[2] - crop[0]), crop[0])
            v = g.math("ADD", g.math("MULTIPLY", sep.outputs[2], crop[3] - crop[1]), crop[1])
            cc = g.n("ShaderNodeCombineXYZ")
            g.set(cc.inputs[0], u)
            g.set(cc.inputs[1], v)
            ti = g.n("ShaderNodeTexImage", {"Vector": cc.outputs[0]}, interpolation="Cubic")
            ti.image = code_img
            em = g.n("ShaderNodeEmission", {"Color": ti.outputs["Color"], "Strength": strength})
            return finish(m, g, out, em, lin("#22c55e"))
        scr = code_mat("laptop_screen", 2.0, (0.0, 0.35, 0.62, 0.95))
        wall_scr = code_mat("wall_code", 2.0)
        alu_l = mat_metal("laptop_alu", "#b9bcbf", 0.25, True)
        for k, xx in enumerate((0.75, 1.6, 2.45, 3.3)):
            lx_ = gx0 + xx - 0.25
            # the screens face the window side; the third laptop is half closed (its lid at 50 degrees)
            ang = 12.0 if k != 2 else 50.0
            box("lap_base%d" % k, lx_ - 0.16, lx_ + 0.16, bench_y0 + 0.12, bench_y0 + 0.34, 0.78, 0.795, alu_l, 0.003)
            hy, hz = bench_y0 + 0.34, 0.795
            a_ = math.radians(ang)
            cy_l, cz_l = hy - math.sin(a_) * 0.11, hz + math.cos(a_) * 0.11
            boxc("lap_lid%d" % k, (lx_, cy_l + 0.006 * math.cos(a_), cz_l + 0.006 * math.sin(a_)), (0.32, 0.008, 0.22), alu_l,
                 rot=(math.radians(-ang), 0, 0), bevel=0.003)
            if k != 2:
                boxc("lap_scr%d" % k, (lx_, cy_l - 0.0005, cz_l), (0.29, 0.002, 0.19), scr, rot=(math.radians(-ang), 0, 0))
        # nobody at the bench, but the room is in use: two bar stools pulled out at angles, a jacket over one
        # backrest, a canvas tote on the bench, two takeaway coffees with lids
        canvas = mat_surface("tote_canvas_in", "#d9cfb9", "#c7bca5", 40, (0.8, 0.95), 0.2)
        jacket = mat_surface("jacket", "#2f3a4a", "#232c38", 30, (0.75, 0.9), 0.25)
        for k, (xx, dy, yaw) in enumerate(((0.75, -0.62, 24.0), (2.45, -0.48, -16.0))):
            px_ = gx0 + xx - 0.25
            S_ = group("barstool%d" % k, (px_, bench_y0 + dy, 0.0), yaw)
            cyl("stool_seat%d" % k, 0.19, 0.04, (0, 0, 0.66), oak, 32, parent=S_)
            for j in range(4):
                a2 = math.radians(45 + 90 * j)
                beam("stool_leg%d_%d" % (k, j), (0.13 * math.cos(a2), 0.13 * math.sin(a2), 0.64), (0.2 * math.cos(a2), 0.2 * math.sin(a2), 0.0),
                     0.013, dark, 8, parent=S_)
            torus("stool_ring%d" % k, 0.17, 0.008, (0, 0, 0.26), dark, 32, 8, axis="Z", parent=S_)
            for sx in (-0.13, 0.13):
                beam("stool_post%d_%.2f" % (k, sx), (sx, 0.15, 0.66), (sx, 0.19, 0.95), 0.011, dark, 8, parent=S_)
            boxc("stool_back%d" % k, (0, 0.19, 0.93), (0.34, 0.03, 0.09), oak, rot=(math.radians(-8), 0, 0), bevel=0.01, parent=S_)
            if k == 0:
                # the jacket: folded over the backrest, hanging down both sides, sleeves loose
                boxc("jacket_top", (0, 0.19, 0.985), (0.4, 0.07, 0.035), jacket, bevel=0.015, parent=S_)
                boxc("jacket_back", (0, 0.235, 0.78), (0.42, 0.03, 0.42), jacket, rot=(math.radians(6), 0, 0), bevel=0.012, parent=S_)
                boxc("jacket_front", (0, 0.15, 0.86), (0.38, 0.025, 0.25), jacket, rot=(math.radians(-10), 0, 0), bevel=0.012, parent=S_)
                beam("jacket_sleeve", (0.19, 0.24, 0.95), (0.23, 0.27, 0.55), 0.04, jacket, 10, parent=S_)
        # the tote on the bench, slumped, handles folded over
        T_ = group("bench_tote", (gx0 + 1.2, bench_y0 + 0.55, 0.78), 18)
        boxc("tote_body", (0, 0, 0.17), (0.36, 0.1, 0.34), canvas, rot=(math.radians(-8), 0, 0), bevel=0.03, parent=T_)
        for sx in (-0.08, 0.08):
            beam("tote_h%.2f" % sx, (sx, -0.03, 0.33), (sx * 0.8, -0.12, 0.38), 0.012, canvas, 8, parent=T_)
        # two takeaway coffees with lids
        cup = mat_plain("cup_white", "#f1efe9", 0.45)
        lid_m = mat_plain("cup_lid", "#1c1c1c", 0.35)
        sleeve = mat_surface("cup_sleeve", "#b38b5d", "#a07b50", 20, (0.7, 0.85), 0.05)
        for k, (cx2, cy2) in enumerate(((gx0 + 1.95, bench_y0 + 0.2), (gx0 + 2.1, bench_y0 + 0.62))):
            cyl("cup%d" % k, 0.033, 0.13, (cx2, cy2, 0.78 + 0.065), cup, 28, r2=0.045)
            cyl("cup_sleeve%d" % k, 0.041, 0.05, (cx2, cy2, 0.78 + 0.07), sleeve, 28, r2=0.044)
            cyl("cup_lid%d" % k, 0.046, 0.016, (cx2, cy2, 0.78 + 0.137), lid_m, 28, r2=0.04)
        # a wall screen on the back wall: the cohort's board, green on dark
        box("wallscreen_bezel", cxm - 0.95, cxm + 0.95, 8.58, 8.64, 1.45, 2.55, mat_plain("bezel", "#0a0a0b", 0.35, coat=0.6), 0.01)
        box("wallscreen", cxm - 0.91, cxm + 0.91, 8.575, 8.58, 1.49, 2.51, wall_scr)
        # pendant lamps over the bench: warm bulbs that bloom
        for k, xx in enumerate((0.9, 2.0, 3.1)):
            px = gx0 + xx - 0.25
            yy = (bench_y0 + bench_y1) / 2
            beam("pend_cord%d" % k, (px, yy, open_top - 0.05), (px, yy, 1.7), 0.004, mat_plain("cord", "#111111", 0.5))
            cyl("pend_shade%d" % k, 0.17, 0.2, (px, yy, 1.62), mat_metal("brass", "#c9a46a", 0.3), 28, r2=0.045)
            sphere("pend_bulb%d" % k, 0.05, (px, yy, 1.52), mat_emit("bulb", None, 60, 2400), 14)
            light("pend_L%d" % k, "POINT", (px, yy, 1.48), 45, temp=2500, shadow_soft=0.05)
        # a shelf of books and a plant on the side wall, a whiteboard of sketches behind the bench
        allb, allc = [], []
        rng_b = random.Random(8)
        for lvl in range(3):
            zz = 1.2 + lvl * 0.4
            box("shelf%d" % lvl, gx0 + 0.02, gx0 + 0.3, 4.8, 7.6, zz, zz + 0.025, oak)
            b_, c_ = bookshelf_fill("hb%d" % lvl, 4.85, 7.5, 0, zz + 0.025, 0.25, rng_b)
            for (x0_, x1_, y0_, y1_, z0_, z1_), c__ in zip(b_, c_):
                allb.append((gx0 + 0.04, gx0 + 0.04 + (y1_ - y0_), x0_, x1_, z0_, z1_))
                allc.append(c__)
        boxes_mesh("hero_books", allb, allc, (0.45, 0.75))
        box("whiteboard", cxm - 1.2, cxm + 1.2, 8.5, 8.56, 0.9, 1.35, mat_plain("whiteboard", "#f2f2ee", 0.25))
        light("h_fill", "AREA", (cxm, 4.5, open_top - 0.1), 110, temp=3000, target=(cxm, 4.5, 0), size=3.0, size_y=7.0)
        light("h_back", "AREA", (cxm, 8.3, 2.9), 60, temp=2800, target=(cxm, 8.7, 1.0), size=3.4, size_y=0.3)

        # glass shopfront: aluminium frame, fixed panes, a hung glass door with a bar handle and a closer
        frame = mat_metal("alu_dark", "#2f302f", 0.35)
        m, g, out = new_mat("shop_glass_dirty")
        tr = g.n("ShaderNodeBsdfTransparent", {"Color": (0.95, 0.975, 0.97)})
        gl = g.n("ShaderNodeBsdfGlossy", {"Color": (1, 1, 1), "Roughness": 0.02})
        lw = g.n("ShaderNodeLayerWeight", {"Blend": 0.18})
        mx = g.n("ShaderNodeMixShader")
        shadow_ = g.math("SUBTRACT", 1.0, g.n("ShaderNodeLightPath").outputs["Is Shadow Ray"])
        g.set(mx.inputs[0], g.math("MULTIPLY", g.math("MAXIMUM", lw.outputs["Fresnel"], 0.05), shadow_))
        g.set(mx.inputs[1], tr)
        g.set(mx.inputs[2], gl)
        # a 2 percent dirt map: hand smears and dried rain spots, a faint diffuse film
        nzd = g.noise(g.coords("Object", (1, 1, 1)), 7.0, 6, 0.65)
        spots = g.n("ShaderNodeTexVoronoi", {"Vector": g.coords("Object"), "Scale": 60.0})
        dirt = g.math("MAXIMUM", g.maprange(nzd, 0.62, 0.75, 0.0, 0.05), g.math("MULTIPLY", g.math("LESS_THAN", spots.outputs["Distance"], 0.06), 0.04))
        df = g.n("ShaderNodeBsdfDiffuse", {"Color": (0.75, 0.73, 0.68)})
        mx2 = g.n("ShaderNodeMixShader")
        g.set(mx2.inputs[0], g.math("MULTIPLY", dirt, shadow_))
        g.set(mx2.inputs[1], mx.outputs[0])
        g.set(mx2.inputs[2], df)
        glass = finish(m, g, out, mx2.outputs[0], (0.6, 0.7, 0.75))
        gy = 0.3
        box("sf_kick", gx0, gx1, gy - 0.04, gy + 0.04, 0.0, 0.16, frame)
        # full-height glazing: the shutter box comes out, so the room shows up to its ceiling
        if "hero_shbox" in bpy.data.objects:
            bpy.data.objects.remove(bpy.data.objects["hero_shbox"], do_unlink=True)
        box("sf_head", gx0, gx1, gy - 0.04, gy + 0.04, open_top - 0.08, open_top, frame)
        door0, door1 = cxm + 0.25, cxm + 1.25
        for xx in (gx0, door0 - 0.06, door1, gx1 - 0.06):
            box("sf_mull%.2f" % xx, xx, xx + 0.06, gy - 0.05, gy + 0.05, 0.0, open_top - 0.08, frame)
        box("sf_transom", door0, door1, gy - 0.04, gy + 0.04, 2.3, 2.36, frame)
        panes = [("glass_l", gx0 + 0.06, door0 - 0.06, 0.16, open_top - 0.08),
                 ("glass_r", door1 + 0.06, gx1 - 0.06, 0.16, open_top - 0.08),
                 ("glass_t", door0, door1, 2.36, open_top - 0.08)]
        for nm, a_, b_, c_, d_ in panes:
            ob = box(nm, a_, b_, gy - 0.004, gy + 0.004, c_, d_, glass)
            ob["sv_mask_ignore"] = True
        # the door: a frameless glass leaf on top and bottom patch fittings, standing just ajar
        D = group("door", (door0 + 0.02, gy, 0.0), -7.0)
        ob = box("glass_door", 0.0, door1 - door0 - 0.04, -0.006, 0.006, 0.03, 2.28, glass, parent=D)
        ob["sv_mask_ignore"] = True
        steel_b = mat_metal("steel_brushed", "#c7c9c9", 0.22, True)
        for zz in (0.03, 2.2):
            box("door_patch%.1f" % zz, 0.0, 0.16, -0.016, 0.016, zz, zz + 0.08, steel_b, 0.003, parent=D)
        hx_ = door1 - door0 - 0.16
        for sy in (-1, 1):
            beam("door_bar%d" % sy, (hx_, sy * 0.06, 0.55), (hx_, sy * 0.06, 1.75), 0.016, steel_b, 12, parent=D)
            for zz in (0.62, 1.68):
                beam("door_stand%d_%.1f" % (sy, zz), (hx_, sy * 0.006, zz), (hx_, sy * 0.06, zz), 0.008, steel_b, 8, parent=D)
        boxc("door_closer", (door0 + 0.35, gy - 0.07, 2.27), (0.3, 0.06, 0.07), mat_metal("closer", "#9a9c9d", 0.35), bevel=0.008)
        beam("door_closer_arm", (door0 + 0.48, gy - 0.07, 2.25), (door0 + 0.6, gy - 0.12, 2.24), 0.008, steel_b, 8)
        box("step", gx0, gx1, -0.25, gy - 0.04, 0.15, 0.2, mat_surface("step_stone", "#8d887e", "#6f6b63", 4, (0.3, 0.6), 0.1))

        # THE LIGHTBOX above the opening
        alu = mat_metal("alu", "#c3c6c8", 0.3)
        box("lightbox_case", gx0 + 0.05, gx1 - 0.05, -0.25, 0.0, open_top + 0.08, 3.98, alu, 0.012)
        lbw, lbh = (gx1 - gx0) - 0.2, 0.74
        design_face("lightbox", (cxm, -0.255, (open_top + 0.08 + 3.98) / 2), lbw, lbh, kind="emissive", surface="lightbox",
                    label="Lightbox %.2f x %.2f m" % (lbw, lbh), emit_strength=3.2)
        light("lightbox_spill", "AREA", (cxm, -0.32, 3.5), 60, temp=6000, target=(cxm, -3.0, 2.6), size=lbw, size_y=0.6)
        # A1 window poster on the inside of the left pane, at eye level beside the door, with its own warm spot
        pw, ph = 0.594, 0.841
        pcz = 1.52
        pcx = door0 - 0.06 - 0.14 - pw / 2
        design_face("window-poster", (pcx, gy + 0.012, pcz), pw, ph, kind="printed", surface="paper",
                    label="Window poster A1, 594 x 841 mm")
        box("poster_back", pcx - pw / 2, pcx + pw / 2, gy + 0.013, gy + 0.015, pcz - ph / 2, pcz + ph / 2,
            mat_plain("poster_back", "#efeeea", 0.7))
        for k, (dx, dz) in enumerate(((-1, 1), (1, 1), (1, -1), (-1, -1))):
            box("tape%d" % k, pcx + dx * pw / 2 - 0.03, pcx + dx * pw / 2 + 0.03, gy + 0.0085, gy + 0.0095,
                pcz + dz * ph / 2 - 0.018, pcz + dz * ph / 2 + 0.018, mat_plain("tape", "#e8e3cf", 0.3))
            bpy.data.objects["tape%d" % k]["sv_mask_ignore"] = True
        # soffit downlights: a tight warm spot on the poster, a second one on the door and the step
        for k, dx in enumerate((pcx, door1 - 0.3)):
            cyl("downlight%d" % k, 0.06, 0.05, (dx, -0.45, 2.98), mat_metal("dl_ring", "#d8d8d4", 0.3), 20)
            cyl("downlight_lens%d" % k, 0.045, 0.01, (dx, -0.45, 2.953), mat_emit("dl_lens", None, 40, 2800), 20)
        light("poster_spot", "SPOT", (pcx, -0.45, 2.94), 550, temp=2800, target=(pcx, gy, pcz), spot_deg=30, blend=0.4, shadow_soft=0.02)
        light("door_spot", "SPOT", (door1 - 0.3, -0.45, 2.94), 180, temp=3000, target=(door1 - 0.3, 0.0, 0.2), spot_deg=55, blend=0.5, shadow_soft=0.03)
        box("soffit", gx0, gx1, -0.6, 0.0, 2.99, 3.08, mat_surface("soffit", "#e8e2d6", "#cfc7b9", 2, (0.6, 0.8), 0.02))

        # street: a concrete power pole on the near kerb, cables, a lamp on our side
        pole_m = mat_surface("pole_conc", "#a7a39a", "#6e6a62", 3, (0.7, 0.9), 0.1, streaks=0.3)
        for px in (-7.5, 12.0):          # the right pole stands on a party wall, clear of the signs
            cyl("epole%.1f" % px, 0.17, 10.5, (px, -2.25, 5.25), pole_m, 14, r2=0.11)
            boxc("ecross%.1f" % px, (px, -2.25, 9.6), (0.14, 1.6, 0.14), pole_m)
        for j in range(10):
            dz = 9.55 - (j % 4) * 0.3
            dy = -2.25 + (j - 5) * 0.11
            cable("wire%d" % j, (-30, dy, dz), (-7.5, dy, dz), 1.0 + 0.1 * (j % 3), 0.011 + 0.006 * (j % 2), cab)
            cable("wire2%d" % j, (-7.5, dy, dz), (12.0, dy, dz), 0.8 + 0.12 * (j % 3), 0.011 + 0.006 * (j % 2), cab)
            cable("wire3%d" % j, (12.0, dy, dz), (30.0, dy, dz), 0.9 + 0.1 * (j % 3), 0.011 + 0.006 * (j % 2), cab)
        for j in range(5):
            cable("drop%d" % j, (-7.5, -2.25, 8.6 - 0.25 * j), (-5.5 + j * 1.6, 0.0, 6.6 - 0.2 * j), 0.35, 0.009, cab)
        boxc("meter_box", (-7.5, -2.0, 3.0), (0.45, 0.22, 0.65), mat_metal("meterbox", "#7c8580", 0.6), bevel=0.02)
        street_lamp("lamp", 9.0, -9.6, 8.0, 1.8, 90, 5200, 260)
        # across the soi (behind the camera, seen in reflections)
        for k in range(8):
            x0 = -24 + k * 6.0
            box("opp%d" % k, x0, x0 + 5.8, -24, -11.2, 0, rng.choice([10, 13, 16]),
                mat_facade("opp_facade%d" % (k % 3), rng.choice(walls), bay=2.9, floor_h=3.2, win_w=2.0, win_h=1.6,
                           lit_ratio=0.4, seed=k * 3.1, lit_strength=2.0))
        car("parked", -11.5, -3.6, 0, "#e3e3e0", "off")
        # street life at the noodle shop next door: red plastic stools, a steel table, a water barrel
        boxc("table_top", (6.4, -1.2, 0.72), (1.0, 0.6, 0.03), mat_metal("steel_table", "#b9bcbc", 0.25, True), bevel=0.005)
        for dx in (-0.45, 0.45):
            for dy in (-0.25, 0.25):
                beam("table_leg", (6.4 + dx, -1.2 + dy, 0.15), (6.4 + dx, -1.2 + dy, 0.71), 0.015, mat_metal("steel_table", "#b9bcbc", 0.25, True))
        for k, (sx, sy) in enumerate(((5.6, -1.45), (7.25, -0.95), (6.1, -1.95), (7.0, -1.9))):
            plastic_stool("stool%d" % k, (sx, sy, 0.15), "#c8261c" if k != 2 else "#2c5aa0")
        water_barrel("barrel", (8.9, -0.6, 0.15))
        for k in range(3):
            boxc("crate%d" % k, (11.0, -0.5, 0.3 + k * 0.29), (0.55, 0.38, 0.28), mat_paint("crate", "#1f7a46" if k % 2 else "#d8a51c", 0.5, 0.1), bevel=0.01)

        haze(0.0045, (0.8, 0.84, 0.93), (0, -5, 15), (120, 80, 30))
        world_sky(elev_deg=-4.0, rot_deg=150, strength=4.0, air=1.0, aerosol=2.0, ozone=1.6)
        # 35 mm at 1.2 m, nearly square to the shopfront, turned 4 degrees toward SV's unit; lens shift sets the
        # lightbox's top edge about 8 percent below the frame top, the glazing runs full height below it
        cx_, cy_ = float(os.environ.get("SHOP_CX", "3.3")), float(os.environ.get("SHOP_CY", "-6.8"))
        yw = math.radians(float(os.environ.get("SHOP_YAW", "-4.0")))
        cam = camera("SV_camera", (cx_, cy_, 1.2), (cx_ + math.sin(yw) * 10, cy_ + math.cos(yw) * 10, 1.2), lens=35.0,
                     shift_y=float(os.environ.get("SHOP_SY", "0.165")), level=True, fstop=5.6, focus=abs(cy_) + 0.3)
        over = camera("SV_overview", (-13.0, -10.4, 9.0), (2.5, 0.0, 2.6), lens=24.0)
        return {"camera": cam, "overview": over, "exposure": 0.2, "samples": 1024, "proxy_scale": 1.2, "look": "AgX - Punchy",
                "finish": {"k": 0.016, "ca": 0.0013, "vignette": 0.26, "grain": 0.013, "split": 1.0},
                "glare": {"threshold": 1.0, "strength": 0.7, "size": 0.8}}

    def scene_mall():
        """A four-level mall atrium in the late afternoon: a skylight overhead, glass
        balustrades, crossing escalators and a large LED screen on the far wall."""
        rng = random.Random(5)
        floor_m = mat_tiles("mall_floor", "#8e8a84", "#85817b", "#6a6762", 1.2, 0.0012, 0.05, 0.04, coat=0.8, row=1.2)
        white = mat_surface("mall_white", "#efeee9", "#dfddd6", 2, (0.4, 0.55), 0.01)
        fascia = mat_surface("fascia", "#e9e6df", "#d6d2c9", 2, (0.25, 0.4), 0.01, coat=0.3)
        champ = mat_metal("champagne", "#c9bb9c", 0.28, True)
        steel = mat_metal("steel", "#d2d5d7", 0.18, True)
        glass = mat_glass("balustrade_glass", (0.88, 0.95, 0.94), 0.06)
        black = mat_plain("rubber", "#0b0b0b", 0.3)
        rubber = mat_surface("handrail_rubber", "#0d0d0d", "#161616", 60, (0.42, 0.55), 0.04)
        led_strip = mat_emit("led_cool", None, 14.0, 5200)
        down = mat_emit("downlight", None, 40.0, 4500)
        L = [0.0, 5.5, 11.0, 16.5]
        roof = 22.0
        VX = 11.0          # half width of the void
        box("floor_L1", -40, 40, -50, 30, -0.3, 0.0, floor_m)
        for xs in (-1, 1):
            box("outer_wall_%d" % xs, xs * 16.0, xs * 16.6, -50, 30, 0, roof, white)
        box("far_wall", -16.6, 16.6, 16.0, 16.6, 4.4, roof, white)
        for (a_, b_) in ((-16.6, -9.85), (10.85, 16.6)):
            box("far_wall_low%d" % a_, a_, b_, 16.0, 16.6, 0, 4.4, white)
        box("near_wall", -16.6, 16.6, -50.6, -50.0, 0, roof, white)
        for i, z in enumerate(L[1:]):
            for xs in (-1, 1):
                x0, x1 = xs * VX, xs * 16.0
                box("slab_%d_%d" % (i, xs), x0, x1, -50, 16, z - 0.9, z, white)
                box("slab_fascia_%d_%d" % (i, xs), x0 - xs * 0.02, x0 + xs * 0.14, -50, 16, z - 0.92, z + 0.02, fascia, 0.01)
                box("led_under_%d_%d" % (i, xs), x0 + xs * 0.18, x0 + xs * 0.26, -50, 16, z - 0.93, z - 0.9, led_strip)
                box("bal_glass_%d_%d" % (i, xs), x0 + xs * 0.12, x0 + xs * 0.14, -50, 16, z + 0.08, z + 1.1, glass)
                box("bal_base_%d_%d" % (i, xs), x0 + xs * 0.06, x0 + xs * 0.2, -50, 16, z, z + 0.08, steel)
                box("bal_rail_%d_%d" % (i, xs), x0 + xs * 0.07, x0 + xs * 0.19, -50, 16, z + 1.1, z + 1.15, steel, 0.01)
                for yy in range(-48, 16, 4):
                    cyl("dl", 0.09, 0.012, (xs * 13.5, yy, z - 0.906), down, 16)
                    cyl("dl2", 0.09, 0.012, (xs * 15.0, yy + 2, z - 0.906), down, 16)
                for yy in range(-46, 16, 6):
                    light("dlS_%d_%d_%d" % (i, xs, yy), "SPOT", (xs * 13.5, yy, z - 0.93), 260, temp=4800,
                          target=(xs * 13.5, yy, 0), spot_deg=70, blend=0.5, shadow_soft=0.05)
        for xs in (-1, 1):
            pass
        for o in bpy.data.objects:
            if "glass" in o.name:
                o["sv_mask_ignore"] = True
        # bridge at L2 in front of the far wall
        bz = L[1]
        box("bridge", -VX, VX, 9.6, 16.0, bz - 0.9, bz, white)
        box("bridge_fascia", -VX, VX, 9.48, 9.62, bz - 0.92, bz + 0.02, fascia, 0.01)
        box("bridge_led", -VX, VX, 9.64, 9.72, bz - 0.93, bz - 0.9, led_strip)
        for (a, b) in ((-VX, -3.4), (3.4, VX)):
            g_ = box("bridge_glass_%d" % a, a, b, 9.62, 9.64, bz + 0.08, bz + 1.1, glass)
            g_["sv_mask_ignore"] = True
            box("bridge_rail_%d" % a, a, b, 9.57, 9.69, bz + 1.1, bz + 1.15, steel, 0.01)
            box("bridge_base_%d" % a, a, b, 9.56, 9.7, bz, bz + 0.08, steel)
        # round columns with a champagne metal plinth
        for xs in (-1, 1):
            for yy in range(-44, 16, 12):
                cyl("col_%d_%d" % (xs, yy), 0.6, roof, (xs * 11.7, yy, roof / 2), white, 40)
                cyl("colbase_%d_%d" % (xs, yy), 0.64, 0.3, (xs * 11.7, yy, 0.15), champ, 40)
        # shopfronts: lit glass boxes with lettered sign bands
        shop_cols = [3500, 4000, 4500, 5000, 3200]
        names = ["BOOKS", "OPTIC", "TEA HOUSE", "STUDIO", "ร้านหนังสือ", "GALLERY", "ชาไทย", "SHOES", "FLORIST",
                 "CERAMICS", "เบเกอรี่", "AUDIO", "PAPER", "ผ้าไทย", "WATCHES", "LINEN"]
        sfont = load_font(FONT_SIGN2)
        for i, z in enumerate(L):
            for xs in (-1, 1):
                yy = -48.0
                k = 0
                while yy < 15:
                    wdt = rng.choice([6.0, 8.0, 10.0])
                    y1 = min(yy + wdt, 15.5)
                    temp = rng.choice(shop_cols)
                    inner = mat_emit("shop_in_%d_%d" % (temp, k % 3), None, rng.choice([1.6, 2.2, 3.0]), temp, noise=0.4)
                    box("shop_glow_%d_%d_%d" % (i, xs, k), xs * 16.0, xs * 15.98, yy + 0.2, y1 - 0.2, z, z + 4.2, inner)
                    gm = box("shop_glass_%d_%d_%d" % (i, xs, k), xs * 15.72, xs * 15.74, yy + 0.25, y1 - 0.25, z, z + 3.7,
                             mat_glass("shopfront_glass", (0.9, 0.95, 0.95), 0.07))
                    gm["sv_mask_ignore"] = True
                    sc_ = rng.choice(["#f4f1ea", "#1d1d1d", "#2b2f33", "#e6dccb", "#25352e"])
                    box("shop_sign_%d_%d_%d" % (i, xs, k), xs * 15.62, xs * 15.76, yy + 0.4, y1 - 0.4, z + 3.78, z + 4.42,
                        mat_plain("sb_" + sc_, sc_, 0.35, coat=0.4))
                    light_txt = sc_ in ("#1d1d1d", "#2b2f33", "#25352e")
                    tm = mat_emit("sbt_w", "#fff6e6", 6.0) if light_txt else mat_plain("sbt_k", "#1a1a1a", 0.4)
                    text3d("shop_name_%d_%d_%d" % (i, xs, k), rng.choice(names), 0.36,
                           (xs * 15.61, (yy + y1) / 2, z + 4.1), (90, 0, -90 * xs), tm, 0.01, font=sfont)
                    box("shop_pier_%d_%d_%d" % (i, xs, k), xs * 15.6, xs * 16.0, y1 - 0.2, y1 + 0.2, z, z + 4.9, champ)
                    # a few display plinths inside
                    for d in range(2):
                        boxc("disp", (xs * 15.2, yy + 1.5 + d * (wdt - 3) / 1.5, z + 0.45), (0.6, 0.8, 0.9),
                             mat_plain("disp", rng.choice(["#e8e4dc", "#2d2d2b", "#c9b79a"]), 0.4), bevel=0.01)
                    yy = y1
                    k += 1
        # far wall: stone cladding and the LED screen
        clad = mat_surface("walnut", "#4a3626", "#2f2118", 2.0, (0.35, 0.5), 0.05, stretch=(1, 1, 0.08), coat=0.2)
        box("feature_wall", -VX, VX, 15.7, 16.0, bz, roof, clad)
        sw, sh = 12.8, 7.2
        scz = 11.0
        gap = 0.03
        nx0, nx1, nz0, nz1 = -sw / 2 - gap, sw / 2 + gap, scz - sh / 2 - gap, scz + sh / 2 + gap
        for k in range(int(2 * VX / 0.24)):
            xx = -VX + 0.06 + k * 0.24
            if xx + 0.1 > nx0 and xx < nx1:
                # the fins stop at the niche: the screen sits in a window cut through the slats
                box("fin%d_lo" % k, xx, xx + 0.1, 15.3, 15.7, bz, nz0, clad)
                box("fin%d_hi" % k, xx, xx + 0.1, 15.3, 15.7, nz1, roof, clad)
            else:
                box("fin%d" % k, xx, xx + 0.1, 15.3, 15.7, bz, roof, clad)
        for xs in (-1, 1):
            light("wash%d" % xs, "SPOT", (xs * 8.0, 12.0, bz + 0.3), 2200, temp=3000, target=(xs * 8.0, 15.6, roof - 3),
                  spot_deg=50, blend=0.8, shadow_soft=0.1)
        # the niche: black reveals and a black back, the screen recessed 0.2 m behind the fin fronts
        blk = mat_plain("bezel", "#060607", 0.4, coat=0.3)
        box("led_back", nx0, nx1, 15.52, 15.7, nz0, nz1, blk)
        design_face("led-screen", (0.0, 15.505, scz), sw, sh, kind="emissive", surface="led",
                    label="LED screen 12.8 x 7.2 m, 16:9", emit_strength=4.2, plate_k=0.07, pitch=0.06, light_emit=1.7)
        # the screen at a real brightness: a cool wash from its whole area onto the reveals, the balustrades, the
        # floor and the escalators (the reflection streak under them)
        light("led_spill", "AREA", (0.0, 15.27, scz), 1500, temp=8000, target=(0, 0, scz), size=sw, size_y=sh)
        bpy.data.objects["led_spill"].visible_camera = False
        # (the screen's own spill lights the slat ends round the niche: no extra edge lamps)
        for k, xx in enumerate((-10, -3, 4)):
            kind = ["books", "tea", "gallery"][k]
            mall_shop(kind, xx + 0.3, xx + 6.7, 15.6, 19.4, 4.2, rng)
            text3d("far_name%d" % k, ["BOOKS", "ชาไทย", "GALLERY"][k], 0.42, (xx + 3.5, 15.3, 4.35), (90, 0, 0),
                   mat_emit("sbt_w", "#fff6e6", 6.0), 0.01, font=sfont)
            box("far_band%d" % k, xx + 0.3, xx + 6.7, 15.32, 15.7, 4.0, 4.7, mat_plain("sb_#1d1d1d", "#1d1d1d", 0.35, coat=0.4))
            box("far_pier%d" % k, xx + 6.7, xx + 7.0, 15.3, 16.0, 0, 4.4, champ)
            box("far_shop_glass%d" % k, xx + 0.4, xx + 6.6, 15.35, 15.37, 0, 3.8, mat_glass("shopfront_glass", (0.9, 0.95, 0.95), 0.07))["sv_mask_ignore"] = True
            box("far_mull%d" % k, xx + 3.48, xx + 3.52, 15.33, 15.39, 0, 3.8, champ)
        # skylight: a steel grid over the void
        for yy in range(-50, 17, 3):
            box("sk_beam_y%d" % yy, -VX, VX, yy - 0.08, yy + 0.08, roof - 0.55, roof, steel)
        for xx in range(-11, 12, 4):
            box("sk_beam_x%d" % xx, xx - 0.08, xx + 0.08, -50, 16, roof - 0.55, roof, steel)
        box("roof_l", -16.6, -VX, -50.6, 16.6, roof, roof + 0.6, white)
        box("roof_r", VX, 16.6, -50.6, 16.6, roof, roof + 0.6, white)
        sk = box("sk_glass", -VX, VX, -50, 16, roof - 0.01, roof, mat_glass("sky_glass", (0.86, 0.92, 0.95), 0.04))
        sk["sv_mask_ignore"] = True
        portal = light("sky_portal", "AREA", (0, -17, roof - 0.6), 1, target=(0, -17, 0), size=22, size_y=66)
        portal.data.cycles.is_portal = True

        # escalators: two side by side, rising to the bridge under the screen
        def escalator(name, xc, z0, z1, y_top):
            ang = math.radians(30)
            run = (z1 - z0) / math.tan(ang)
            yb = y_top - run
            land = 1.6

            def path_z(y):
                if y <= yb:
                    return z0
                if y >= y_top:
                    return z1
                return z0 + (y - yb) * math.tan(ang)
            ys = [yb - land - 0.8 + i * 0.25 for i in range(int((y_top + 0.3 - (yb - land - 0.8)) / 0.25) + 1)]
            top = [(y, path_z(y)) for y in ys]
            prof = [(y, z - 0.08) for y, z in top] + [(y, max(z - 1.1, -0.01)) for y, z in reversed(top)]
            prism_x(name + "_truss", prof, xc - 0.82, xc + 0.82, mat_metal("esc_clad", "#c9cbcc", 0.25, True))
            for s in (-1, 1):
                xs0 = xc + s * 0.62
                sk_ = [(y, z - 0.05) for y, z in top] + [(y, z + 0.18) for y, z in reversed(top)]
                prism_x(name + "_skirt%d" % s, sk_, xs0 - 0.04, xs0 + 0.04, steel)
                prism_x(name + "_skirtled%d" % s, [(y, z + 0.17) for y, z in top] + [(y, z + 0.2) for y, z in reversed(top)],
                        xs0 - 0.05, xs0 + 0.05, mat_emit("esc_led", None, 10.0, 5600))
                gl = [(y, z + 0.2) for y, z in top[2:-1]] + [(y, z + 1.0) for y, z in reversed(top[2:-1])]
                g_ = prism_x(name + "_glass%d" % s, gl, xs0 - 0.008, xs0 + 0.008, glass)
                g_["sv_mask_ignore"] = True
                rail = [(xs0, y, z + 1.02) for y, z in top[2:-1]]
                # the handrail loops round the newel at each end, the way a real one returns under itself
                for end_, sgn in ((0, -1), (-1, 1)):
                    yE, zE = rail[end_][1], rail[end_][2]
                    loop = [(xs0, yE + sgn * 0.16 * math.sin(math.radians(a_)), zE - 0.16 + 0.16 * math.cos(math.radians(a_)))
                            for a_ in range(15, 181, 15)]
                    rail = loop[::-1] + rail if end_ == 0 else rail + loop
                cu = bpy.data.curves.new(name + "_hr%d" % s, "CURVE")
                cu.dimensions = "3D"
                cu.bevel_depth = 0.042
                cu.bevel_resolution = 3
                sp = cu.splines.new("POLY")
                sp.points.add(len(rail) - 1)
                for i, p in enumerate(rail):
                    sp.points[i].co = (p[0], p[1], p[2], 1)
                ob = bpy.data.objects.new(name + "_hr%d" % s, cu)
                link(ob)
                ob.scale = (1.35, 1, 1)
                ob.location = (xs0 * (1 - 1.35), 0, 0)
                cu.materials.append(rubber)
            stepm = mat_corrugated("steps", "#55585a", 0.012, 0.35, axis="X")
            riser = mat_corrugated("riser", "#3a3c3d", 0.01, 0.4, axis="X")
            yel = mat_plain("comb_yellow", "#e2b31c", 0.5)
            nsteps = int((z1 - z0) / 0.2)
            for i in range(nsteps):
                y0 = yb + i * (run / nsteps)
                zz = z0 + i * 0.2
                box(name + "_step%d" % i, xc - 0.5, xc + 0.5, y0, y0 + run / nsteps + 0.02, zz + 0.1, zz + 0.15, stepm)
                box(name + "_riser%d" % i, xc - 0.5, xc + 0.5, y0 - 0.005, y0 + 0.01, zz - 0.05, zz + 0.1, riser)
                box(name + "_nose%d" % i, xc - 0.5, xc + 0.5, y0 + 0.01, y0 + 0.05, zz + 0.15, zz + 0.153, yel)
                for sx in (-1, 1):
                    box(name + "_sidey%d_%d" % (i, sx), xc + sx * 0.5 - (0.035 if sx > 0 else 0), xc + sx * 0.5 + (0.035 if sx < 0 else 0),
                        y0, y0 + run / nsteps, zz + 0.15, zz + 0.153, yel)
            plate = mat_metal("comb_plate", "#a9acad", 0.3, True, axis_scale=(1, 30, 1))
            for yy0, yy1, zz in ((yb - land - 0.8, yb, 0.0), (y_top, y_top + land, z1)):
                box(name + "_land%.1f" % yy0, xc - 0.55, xc + 0.55, yy0, yy1, zz - 0.02, zz + 0.02, plate)
            for yy, zz, d_ in ((yb, 0.0, -1), (y_top, z1, 1)):
                box(name + "_comb%.1f" % yy, xc - 0.52, xc + 0.52, yy - 0.06 if d_ < 0 else yy, yy if d_ < 0 else yy + 0.06, zz, zz + 0.03, yel)
                for t in range(26):
                    tx = xc - 0.5 + t * 0.04
                    box(name + "_tooth%.1f_%d" % (yy, t), tx, tx + 0.012, yy - 0.012 if d_ < 0 else yy - 0.03, yy + 0.03 if d_ < 0 else yy + 0.012,
                        zz + 0.015, zz + 0.035, yel)
        escalator("esc_l", -1.9, 0.0, bz, 9.6)
        escalator("esc_r", 1.9, 0.0, bz, 9.6)
        # planters and benches at ground level
        hedge = mat_surface("shrub", "#3f6e35", "#1c3a18", 14, (0.55, 0.75), 0.9, fine=90)
        for k, (px, py) in enumerate(((-7.0, -8.0), (7.0, -8.0), (-7.0, 1.0), (7.0, 1.0))):
            boxc("planter%d" % k, (px, py, 0.42), (2.8, 1.3, 0.84), mat_surface("planter", "#d3cdc1", "#c2bbae", 3, (0.3, 0.45), 0.02, coat=0.3), bevel=0.05)
            clipped_shrub("hedge%d" % k, (px, py, 1.1), (2.6, 1.1, 0.52), rng)
            boxc("bench%d" % k, (px, py - 1.35, 0.23), (2.6, 0.45, 0.46), mat_surface("bench_oak", "#a7825a", "#8a6845", 3, (0.35, 0.5), 0.04, stretch=(1, 8, 1)), bevel=0.02)
        # sun through the skylight: grid shadows and soft shafts in the haze
        sun = bpy.data.lights.new("sun", "SUN")
        sun.energy = 0.0
        sun.angle = math.radians(0.6)
        sun.color = kelvin(5600)
        so = bpy.data.objects.new("sun", sun)
        link(so, coll("SV_lights"))
        so.rotation_euler = Vector((-0.42, 0.28, -0.86)).to_track_quat("-Z", "Y").to_euler()
        # cutaway for the overview shots: roof, skylight, near wall and everything left of the void
        bpy.context.view_layer.update()
        for o in bpy.data.objects:
            if o.type not in ("MESH", "CURVE", "FONT"):
                continue
            xs_ = [(o.matrix_world @ Vector(c_)).x for c_ in o.bound_box]
            if o.name.startswith(("roof_", "sk_", "near_wall")) or max(xs_) < -10.95:
                o["sv_cutaway"] = True
        # no people (10 Oct): low-poly figures read as placeholder dolls at a 100 percent crop, so the atrium is shown
        # just after closing, the escalators running empty
        # a book-fair banner hung in the void from the roof: the foreground layer
        # its top hangs inside the frame, so the type can be set from the top margin like a real mall graphic
        hanging_banner("promo_banner", -7.25, -5.5, 3.6, 7.4, 2.0, "#a8432f", "#f4ead8", None, rng)
        world_sky(elev_deg=-4.5, rot_deg=-25, strength=1.2, sun_disc=False, air=1.0, aerosol=1.5)
        haze(0.0012, (0.92, 0.95, 1.0), (0, -17, 11), (34, 68, 22))
        cam = camera("SV_camera", (-2.4, -20.0, 1.6), (0.0, 15.41, 1.6), lens=35.0, shift_y=0.14, level=True)
        over = camera("SV_overview", (-34.0, -52.0, 30.0), (0.0, -2.0, 6.0), lens=30.0)
        return {"camera": cam, "overview": over, "exposure": 0.0, "samples": 640, "proxy_scale": 4.0,
                "finish": {"k": 0.014, "ca": 0.001, "vignette": 0.22, "grain": 0.011, "split": 0.6},
                "glare": {"threshold": 1.8, "strength": 0.35, "size": 0.7}}

    def scene_tote_box():
        """Editorial product still life: a canvas tote hanging from an oak peg on a plaster wall, a kraft paper
        takeaway box on a travertine table below it, late-morning window light from the left.

        The tote is cloth, not a slab: it hangs from its straps, so the top edge sags between the straps, tension
        folds run down from where the straps pull, the top corners droop and the bottom rolls out a little. The
        print is a woven patch sewn onto the front: about 2.5 mm thick, ecru stitches round its edge, stiffer than
        the canvas, so the folds soften under it. The box is a folded kraft meal box: flared walls, a hinged lid,
        a front tuck flap and two side flaps, a label on the lid and a printed front flap."""
        rng = random.Random(3)
        # a cool lime plaster (value about 0.8) behind a warmer, darker natural canvas (about 0.6): the bag separates
        # from the wall by value and by temperature, not by an outline
        wall = mat_surface("lime_plaster", "#e3e4da", "#d3d5c9", 0.9, (0.85, 0.97), 0.1, patches=0.06, fine=25)
        trav = mat_travertine("travertine")
        WALL_Y = 0.62
        box("floor", -4, 4, -4, 1.0, -0.02, 0.0, mat_surface("studio_floor", "#8a8378", "#77716a", 2, (0.6, 0.8), 0.05))
        box("wall", -4, 4, WALL_Y, WALL_Y + 0.08, 0, 3.0, wall)
        box("table_top", -1.3, 1.3, -1.6, 0.6, 0.71, 0.75, trav, 0.006)
        box("table_base", -0.35, 0.35, -0.2, 0.3, 0.0, 0.71, trav, 0.006)

        # ---- canvas: a real plain weave (warp and weft bands), slub noise, a soft sheen, roughness that varies
        cloth_c = "#cdb894"
        m, g, out = new_mat("canvas")
        vec = g.coords("Object", (1, 1, 1))
        wx = g.n("ShaderNodeTexWave", {"Vector": vec, "Scale": 1000.0, "Distortion": 1.2, "Detail": 1.0},
                 wave_type="BANDS", bands_direction="X")
        wz = g.n("ShaderNodeTexWave", {"Vector": vec, "Scale": 1000.0, "Distortion": 1.2, "Detail": 1.0},
                 wave_type="BANDS", bands_direction="Z")
        h = g.math("MULTIPLY", wx.outputs["Fac"], wz.outputs["Fac"])
        slub = g.noise(g.coords("Object", (1, 1, 14)), 30, 4, 0.6)
        nz = g.noise(g.coords("Object"), 40, 6, 0.6)
        hh = g.math("ADD", h, g.math("MULTIPLY", slub, 0.6))
        nrm = g.bump(hh, 0.35, 0.0006)
        col = g.ramp(nz, tuple(x * 0.90 for x in lin(cloth_c)), lin(cloth_c), 0.3, 0.7)
        col = g.mix(g.maprange(slub, 0.55, 0.75, 0.0, 0.25), col, tuple(x * 0.86 for x in lin(cloth_c)))
        rr = g.maprange(nz, 0.3, 0.7, 0.78, 0.92)       # 0.85 on average, varying with the slub
        canvas = finish(m, g, out, principled(g, col, rr, 0.0, 0.3, normal=nrm, sheen=0.5), lin(cloth_c))
        thread = mat_surface("thread_ecru", "#e3dac8", "#cfc4ae", 60, (0.55, 0.7), 0.2)
        m, g, out = new_mat("patch_body")
        wv = g.n("ShaderNodeTexWave", {"Vector": g.coords("Object"), "Scale": 900.0}, wave_type="BANDS", bands_direction="Z")
        patch_body = finish(m, g, out, principled(g, lin("#0b100d"), 0.55, 0.0, 0.5, normal=g.bump(wv.outputs["Fac"], 0.3, 0.0005),
                                                  sheen=0.8), lin("#0b100d"))

        # ---- the hanging tote: built in the tote's own frame (x across, z up, -y toward the camera)
        tw, th = 0.38, 0.42
        TX, TZ = -0.32, 0.98                        # bottom centre of the bag on the wall
        tote = group("tote", (TX, WALL_Y - 0.0013, TZ), 0)   # the back panel rests on the wall
        ATT = (0.25, 0.75)                         # where the straps are sewn on, as u across the top

        def smooth(a, b, x):
            t = min(1.0, max(0.0, (x - a) / (b - a)))
            return t * t * (3 - 2 * t)
        PATCH = (0.5, 0.50, 0.22, 0.22)            # patch centre (u, v) and its size in metres
        pu0 = PATCH[0] - PATCH[2] / tw / 2
        pu1 = PATCH[0] + PATCH[2] / tw / 2
        pv0 = PATCH[1] - PATCH[3] / th / 2
        pv1 = PATCH[1] + PATCH[3] / th / 2

        def stiff(u, v):
            # the patch is sewn on: the canvas folds soften under it
            inside = smooth(pu0 - 0.04, pu0 + 0.02, u) * (1 - smooth(pu1 - 0.02, pu1 + 0.04, u)) * \
                smooth(pv0 - 0.04, pv0 + 0.02, v) * (1 - smooth(pv1 - 0.02, pv1 + 0.04, v))
            return 1.0 - 0.7 * inside

        def surface(u, v, side=0):
            """(x, y, z) of the cloth at (u, v); side 0 is the front panel, 1 the back against the wall."""
            wv_ = tw * (1.0 - 0.05 * v ** 2)
            x = (u - 0.5) * wv_
            # the top edge: sags between the straps, the corners outside them droop and fold forward
            if ATT[0] <= u <= ATT[1]:
                sag_top = 0.026 * math.sin(math.pi * (u - ATT[0]) / (ATT[1] - ATT[0])) ** 1.4
            else:
                d_ = (ATT[0] - u) / ATT[0] if u < ATT[0] else (u - ATT[1]) / (1 - ATT[1])
                sag_top = 0.042 * d_ ** 1.3
            z = v * th - sag_top * v ** 3
            lean_ = -0.035 * v ** 1.3          # the peg holds the top off the wall; the bottom rests against it
            if side == 1:
                return (x, lean_, z)
            # volume: an empty bag still holds a little air, fuller low down
            vol = 0.022 * math.sin(math.pi * u) ** 0.6 * math.sin(math.pi * min(0.97, v * 0.9 + 0.08)) ** 0.7 * (1 - 0.45 * v)
            # tension folds: ridges running from each strap down toward the middle
            fold = 0.0
            for k, ax in enumerate(ATT):
                tx_, tz_ = 0.5 + (0.12 if k == 0 else -0.12), 0.28
                dx_, dz_ = tx_ - ax, tz_ - 1.0
                ln = math.hypot(dx_, dz_)
                px_, pz_ = u - ax, v - 1.0
                t = (px_ * dx_ + pz_ * dz_) / (ln * ln)
                perp = (px_ * dz_ - pz_ * dx_) / ln
                if 0.0 <= t <= 1.0:
                    env = (1 - t) ** 1.4 * smooth(0.0, 0.08, t)
                    fold += 0.024 * env * math.exp(-(perp / 0.038) ** 2)
                    fold -= 0.008 * env * math.exp(-((perp - 0.07 * (1 if k == 0 else -1)) / 0.04) ** 2)
            # a vertical drape fold straight down from each handle root, fading out toward the bottom, with a soft
            # valley either side of it
            for ax in ATT:
                env_v = smooth(0.2, 0.9, v)
                fold += 0.014 * env_v * math.exp(-((u - ax) / 0.03) ** 2)
                fold -= 0.005 * env_v * (math.exp(-((u - ax - 0.075) / 0.03) ** 2) + math.exp(-((u - ax + 0.075) / 0.03) ** 2))
            # the drape between the straps: a soft valley down the middle from the sagging top
            fold -= 0.013 * math.exp(-((u - 0.5) / 0.07) ** 2) * smooth(0.55, 0.95, v)
            # two soft horizontal sag folds under the opening, where the empty bag's weight hangs
            for vz, a_ in ((0.80, 0.006), (0.66, 0.004)):
                fold += a_ * math.exp(-((v - vz - 0.03 * math.sin(6 * u + ph_[1])) / 0.025) ** 2) * math.sin(math.pi * u) ** 0.8
            # the drooping corners roll forward, the bottom rolls out where the bag stops
            corner = (smooth(0.0, 0.22, ATT[0] - u) + smooth(0.0, 0.22, u - ATT[1])) * smooth(0.82, 1.0, v)
            fold += 0.024 * corner
            fold += 0.016 * (1 - smooth(0.0, 0.16, v)) * math.sin(math.pi * u) ** 0.5
            # small, irregular creases everywhere (the bag was folded in a drawer)
            fold += 0.0032 * math.sin(17 * u + 9 * v + ph_[0]) * math.sin(5 * v + ph_[1]) + \
                0.0018 * math.sin(31 * u - 13 * v + ph_[2]) + 0.0025 * math.sin(7 * u + 3 * v + ph_[3]) * (1 - v)
            # the side seams pinch front and back together: no slab edge, the cloth meets the back panel
            seam = smooth(0.0, 0.06, u) * smooth(0.0, 0.06, 1.0 - u)
            # the weight of the empty bag settles at the bottom: it bulges forward and rounds into its base
            belly = 0.04 * (1 - smooth(0.0, 0.42, v)) ** 1.5 * math.sin(math.pi * u) ** 0.45
            y = -(vol + belly + fold * stiff(u, v)) * seam - 0.004 + lean_
            return (x, y, z)
        ph_ = [rng.uniform(0, 6.28) for _ in range(4)]

        n = 56
        verts, faces = [], []
        for side_ in (0, 1):
            for j in range(n + 1):
                for i in range(n + 1):
                    verts.append(surface(i / n, j / n, side_))
            base = side_ * (n + 1) ** 2
            for j in range(n):
                for i in range(n):
                    a_ = base + j * (n + 1) + i
                    q = (a_, a_ + 1, a_ + n + 2, a_ + n + 1)
                    faces.append(q if side_ == 1 else tuple(reversed(q)))
        for j in range(n):                      # side seams and the bottom fold close the bag
            for sidex in (0, n):
                a0 = j * (n + 1) + sidex
                a1 = (j + 1) * (n + 1) + sidex
                b0 = (n + 1) ** 2 + a0
                b1 = (n + 1) ** 2 + a1
                faces.append((a0, a1, b1, b0) if sidex == 0 else (b0, b1, a1, a0))
        for i in range(n):
            a0, a1 = i, i + 1
            b0, b1 = (n + 1) ** 2 + i, (n + 1) ** 2 + i + 1
            faces.append((a0, b0, b1, a1))
        body = mesh_obj("tote_body", verts, faces, canvas, smooth=True)
        recalc(body)
        body.parent = tote
        # the canvas is 2 mm thick and its cut edge at the opening catches the light: a lighter rim material
        rim_m = mat_surface("canvas_rim", "#efe6d4", "#e2d7c2", 40, (0.8, 0.9), 0.1)
        body.data.materials.append(rim_m)
        md = body.modifiers.new("solid", "SOLIDIFY")
        md.thickness = 0.002
        md.use_rim = True
        md.material_offset_rim = 1
        # the top hem: a folded band, 35 mm deep, two rows of stitching
        hv, hf = [], []
        for k in range(n + 1):
            u = k / n
            for vv in (1.0 - 0.035 / th, 1.0):
                x, y, z = surface(u, vv)
                hv.append((x, y - 0.0012, z))
        for k in range(n):
            hf.append((2 * k, 2 * k + 2, 2 * k + 3, 2 * k + 1))
        hb = mesh_obj("tote_hem", hv, hf, canvas, smooth=True)
        md = hb.modifiers.new("solid", "SOLIDIFY")
        md.thickness = 0.0014
        hb.parent = tote

        def stitch_row(name, pts, dash=0.004, gap=0.0025, r=0.00045, mat=None, lift=0.0):
            """Running stitches along a polyline of (x, y, z): short thread capsules, the way a seam looks up close."""
            obs = []
            seg = []
            for a, b in zip(pts[:-1], pts[1:]):
                seg.append((Vector(a), Vector(b)))
            total = sum((b - a).length for a, b in seg)
            s_ = 0.0
            k = 0
            while s_ + dash < total:
                p0 = p1 = None
                acc = 0.0
                for a, b in seg:
                    L_ = (b - a).length
                    if p0 is None and acc + L_ >= s_:
                        p0 = a.lerp(b, (s_ - acc) / L_)
                    if acc + L_ >= s_ + dash:
                        p1 = a.lerp(b, (s_ + dash - acc) / L_)
                        break
                    acc += L_
                if p0 is not None and p1 is not None:
                    p0.y -= lift
                    p1.y -= lift
                    obs.append(beam("%s_%d" % (name, k), tuple(p0), tuple(p1), r, mat or thread, 6, parent=tote))
                s_ += dash + gap
                k += 1
            return obs
        for vv in (1.0 - 0.006 / th, 1.0 - 0.029 / th):
            stitch_row("hem_st%.3f" % vv, [tuple(Vector(surface(k / 40, vv)) + Vector((0, -0.0027, 0))) for k in range(41)],
                       0.003, 0.0018, 0.0004)

        # ---- the woven patch: a 2.5 mm thick panel on the cloth, the design face on its front
        PT = 0.002

        def on_patch(x, z, lift):
            """A point on the cloth under the patch, lifted along the cloth's own normal (not just toward the
            camera), so the patch keeps its thickness where the canvas folds."""
            u = PATCH[0] + x / tw
            v = PATCH[1] + z / th
            for _ in range(3):
                wv_ = tw * (1.0 - 0.05 * v ** 2)
                u = PATCH[0] + x / wv_
            p0 = Vector(surface(u, v))
            du = Vector(surface(u + 1e-3, v)) - Vector(surface(u - 1e-3, v))
            dv = Vector(surface(u, v + 1e-3)) - Vector(surface(u, v - 1e-3))
            nrm_ = du.cross(dv).normalized()
            if nrm_.y > 0:
                nrm_ = -nrm_
            return tuple(p0 + nrm_ * lift)
        pw, ph = PATCH[2], PATCH[3]
        npg = 24
        pv, pf = [], []
        for j in range(npg + 1):
            for i in range(npg + 1):
                pv.append(on_patch(-pw / 2 + pw * i / npg, ph / 2 - ph * j / npg, 0.0003))
        for j in range(npg):
            for i in range(npg):
                a_ = j * (npg + 1) + i
                pf.append((a_ + npg + 1, a_ + npg + 2, a_ + 1, a_))
        pb = mesh_obj("patch_body", pv, pf, patch_body, smooth=True)
        md = pb.modifiers.new("solid", "SOLIDIFY")
        md.thickness = PT
        md.offset = 1.0
        md.use_even_offset = False      # even offset bulged past the face in the drape valley
        add_bevel(pb, 0.0007, 2)
        pb.parent = tote
        f = design_face("tote-front", (0, 0, 0), pw, ph, kind="printed", surface="woven",
                        label="Woven patch 220 x 220 mm, stitched", subdiv=24, parent=tote)
        me = f.data
        for vtx in me.vertices:
            x, _, z = vtx.co
            vtx.co = on_patch(x, z, PT + 0.0009)
        for poly in me.polygons:
            poly.use_smooth = True
        f["sv_corners_local"] = [list(on_patch(c[0], c[2], PT + 0.0009)) for c in f["sv_corners_local"]]
        f.location = (0, 0, 0)
        f.rotation_euler = (0, 0, 0)
        ins = 0.0055
        ring = [(-pw / 2 + ins, ph / 2 - ins), (pw / 2 - ins, ph / 2 - ins), (pw / 2 - ins, -ph / 2 + ins),
                (-pw / 2 + ins, -ph / 2 + ins), (-pw / 2 + ins, ph / 2 - ins)]
        pts = []
        for (x0, z0), (x1, z1) in zip(ring[:-1], ring[1:]):
            for k in range(12):
                t = k / 12
                pts.append(on_patch(x0 + (x1 - x0) * t, z0 + (z1 - z0) * t, PT + 0.0011))
        pts.append(pts[0])
        stitch_row("patch_st", pts, 0.0042, 0.0024, 0.00055)

        # ---- the straps run up to an oak peg on the wall; each is a 25 mm webbing band, a little twisted
        oak = mat_surface("oak", "#b48a5c", "#8a6440", 3, (0.35, 0.55), 0.06, stretch=(1, 10, 1))
        PEG = (0.0, -0.03, th + 0.24)
        cyl("peg", 0.011, 0.07, (TX + PEG[0], WALL_Y - 0.035, TZ + PEG[2]), oak, 28, rot=(math.radians(96), 0, 0))
        sphere("peg_knob", 0.016, (TX + PEG[0], WALL_Y - 0.072, TZ + PEG[2] + 0.004), oak, 24, scale=(1, 0.7, 1))
        strap_m = mat_surface("strap", "#ddd2bd", "#cbbfa8", 40, (0.88, 0.98), 0.35, stretch=(1, 1, 30))

        def strap(name, side, y_off):
            half = 0.0125
            a = Vector(surface(ATT[0], 1.0, side))
            b = Vector(surface(ATT[1], 1.0, side))
            a.y += y_off
            b.y += y_off
            top = Vector((PEG[0], PEG[1] - 0.012 + (0.006 if side else 0.0), PEG[2] + 0.011))
            N = 48
            cps = []
            for i in range(N + 1):
                t = i / N
                # a quadratic from each attachment up to the peg, then round over the peg's top
                if t <= 0.5:
                    s_ = t / 0.5
                    c_ = a.lerp(top, 0.82) + Vector((-0.02, 0, 0))
                    p = (1 - s_) ** 2 * a + 2 * (1 - s_) * s_ * c_ + s_ ** 2 * top
                else:
                    s_ = (t - 0.5) / 0.5
                    c_ = b.lerp(top, 0.82) + Vector((0.02, 0, 0))
                    p = (1 - s_) ** 2 * top + 2 * (1 - s_) * s_ * c_ + s_ ** 2 * b
                cps.append(p)
            sv_, sf_ = [], []
            for i, p in enumerate(cps):
                d = cps[min(i + 1, N)] - cps[max(i - 1, 0)]
                d.normalize()
                side_v = Vector((d.z, 0, -d.x))
                tw_ = math.radians(10 * math.sin(math.pi * i / N))
                side_v = Matrix.Rotation(tw_, 3, d) @ side_v
                sv_.append(tuple(p + side_v * half))
                sv_.append(tuple(p - side_v * half))
            for i in range(N):
                sf_.append((2 * i, 2 * i + 2, 2 * i + 3, 2 * i + 1))
            ob = mesh_obj(name, sv_, sf_, strap_m, smooth=True)
            m_ = ob.modifiers.new("solid", "SOLIDIFY")
            m_.thickness = 0.0026
            ob.parent = tote
            return ob
        strap("tote_strap_front", 0, -0.0028)
        strap("tote_strap_back", 1, 0.0)
        for ax in ATT:                           # box-and-cross stitch where each strap is sewn on
            x, y, z = surface(ax, 1.0)
            hw, z0_, z1_ = 0.0115, z - 0.032, z - 0.006
            yy = y - 0.0045
            sq = [(x - hw, yy, z1_), (x + hw, yy, z1_), (x + hw, yy, z0_), (x - hw, yy, z0_), (x - hw, yy, z1_)]
            stitch_row("att%.2f_box" % ax, sq, 0.0025, 0.0012, 0.0004)
            stitch_row("att%.2f_x1" % ax, [(x - hw, yy, z1_), (x + hw, yy, z0_)], 0.0025, 0.0012, 0.0004)
            stitch_row("att%.2f_x2" % ax, [(x + hw, yy, z1_), (x - hw, yy, z0_)], 0.0025, 0.0012, 0.0004)

        # ---- the kraft takeaway box: flared tray, hinged lid, front tuck flap, side flaps
        kraft = mat_kraft("kraft", "#a57f56")
        kraft_in = mat_kraft("kraft_in", "#bf9a6e")
        # a honed travertine block near the front edge of the table; the box stands on it, turned 25 degrees and
        # propped back on a small wedge, so its lid label faces the lens
        PL = (float(os.environ.get("TOTE_BX", "0.25")), float(os.environ.get("TOTE_BY", "-0.12")))
        PLZ = 0.23                                   # the block stands 23 cm on the table
        blk_ = boxc("trav_block", (PL[0], PL[1], 0.75 + PLZ / 2), (0.62, 0.32, PLZ), trav, rot_z=-6, bevel=0.003)
        top_z = 0.75 + PLZ
        wedge = prism_x("trav_wedge", [(0.0, 0.0), (0.05, 0.0), (0.05, 0.075)], -0.06, 0.06, trav, bevel=0.002)
        BOX = group("takeaway", (PL[0] + 0.08, PL[1] - 0.02, top_z), -25)
        BOX.rotation_euler = (math.radians(30), 0, math.radians(-25))
        wedge.parent = None
        bpy.context.view_layer.update()
        wedge.matrix_world = Matrix.Translation(BOX.matrix_world @ Vector((0, 0.055, 0.0)) - Vector((0, 0, 0.0))) @ \
            Matrix.Rotation(math.radians(-25), 4, "Z")
        wedge.location.z = top_z
        bw0, bd0, bw1, bd1, bh = 0.180, 0.120, 0.198, 0.138, 0.058      # bottom and top sizes, height
        tv = [(-bw0 / 2, -bd0 / 2, 0), (bw0 / 2, -bd0 / 2, 0), (bw0 / 2, bd0 / 2, 0), (-bw0 / 2, bd0 / 2, 0),
              (-bw1 / 2, -bd1 / 2, bh), (bw1 / 2, -bd1 / 2, bh), (bw1 / 2, bd1 / 2, bh), (-bw1 / 2, bd1 / 2, bh)]
        tf = [(0, 3, 2, 1), (0, 1, 5, 4), (1, 2, 6, 5), (2, 3, 7, 6), (3, 0, 4, 7)]
        tray = mesh_obj("box_tray", tv, tf, [kraft, kraft_in])
        md = tray.modifiers.new("solid", "SOLIDIFY")
        md.thickness = 0.0006
        md.offset = -1.0
        add_bevel(tray, 0.0004, 2)
        tray.parent = BOX
        # corner gussets: the paper folds into a triangle at each corner, a raised crease you can see
        for cx_, cy_ in ((-1, -1), (1, -1), (1, 1), (-1, 1)):
            a = Vector((cx_ * bw0 / 2, cy_ * bd0 / 2, 0.003))
            b = Vector((cx_ * bw1 / 2, cy_ * bd1 / 2, bh - 0.002))
            beam("box_crease%d%d" % (cx_, cy_), tuple(a), tuple(b), 0.0009, kraft, 6, parent=BOX)
        # the lid: slightly bowed, hinged at the back edge
        lw, ld = bw1 + 0.002, bd1 + 0.002
        lv, lf = [], []
        nl = 12
        for j in range(nl + 1):
            for i in range(nl + 1):
                u, v = i / nl, j / nl
                lv.append(((u - 0.5) * lw, (v - 0.5) * ld, bh + 0.0008 + 0.0008 * math.sin(math.pi * u) * math.sin(math.pi * v)))
        for j in range(nl):
            for i in range(nl):
                a_ = j * (nl + 1) + i
                lf.append((a_, a_ + 1, a_ + nl + 2, a_ + nl + 1))
        lid = mesh_obj("box_lid", lv, lf, kraft, smooth=True)
        md = lid.modifiers.new("solid", "SOLIDIFY")
        md.thickness = 0.0006
        lid.parent = BOX
        flare = math.degrees(math.atan2((bd1 - bd0) / 2, bh))
        fh = 0.032                                    # the front tuck flap, folded down over the front wall
        fl = boxc("box_flap_front", (0, 0, 0), (lw, 0.0006, fh), kraft, bevel=0.0002, parent=BOX)
        fr_ = math.radians(flare)
        fl.rotation_euler = (fr_, 0, 0)
        fl_c = Vector((0, -ld / 2 - 0.0009 + math.sin(fr_) * fh / 2, bh + 0.0006 - math.cos(fr_) * fh / 2))
        fl.location = fl_c
        for sx in (-1, 1):                            # side flaps
            sf = boxc("box_flap_side%d" % sx, (0, 0, 0), (0.0006, ld - 0.012, 0.022), kraft, bevel=0.0002, parent=BOX)
            sf.rotation_euler = (0, math.radians(sx * flare * 0.9), 0)
            sf.location = (sx * (lw / 2 + 0.0008), 0, bh - 0.011 + 0.0006)
        design_face("box-lid", (0, 0.004, bh + 0.0019), 0.120, 0.084, tilt=90, kind="printed", surface="paper",
                    label="Lid label 120 x 84 mm", parent=BOX, round_r=0.006)
        ff = design_face("box-front", (0, 0, 0), lw - 0.008, fh - 0.006, kind="printed", surface="kraft",
                         label="Front flap print 192 x 26 mm", parent=BOX)
        ff.rotation_euler = (fr_, 0, 0)
        ff.location = fl_c + Vector((0, -math.cos(fr_), -math.sin(fr_))) * 0.0005

        # a till receipt, 80 mm wide, folded once and lying flat in the light beside the box, an SV stamp in green
        rw_, rl_ = 0.08, 0.15
        m, g, out = new_mat("receipt")
        tc = g.n("ShaderNodeTexCoord")
        sep = g.n("ShaderNodeSeparateXYZ")
        g.set(sep.inputs[0], tc.outputs["UV"])
        wv = g.n("ShaderNodeTexWave", {"Vector": tc.outputs["UV"], "Scale": 36.0}, wave_type="BANDS", bands_direction="Y")
        ln = g.noise(g.coords("UV", (1, 36, 1)), 3.0, 1.0, 0.5)
        txt = g.math("MULTIPLY", g.math("GREATER_THAN", wv.outputs["Fac"], 0.8),
                     g.math("MULTIPLY", g.math("GREATER_THAN", sep.outputs[0], 0.1),
                            g.math("LESS_THAN", sep.outputs[0], g.maprange(ln, 0.3, 0.7, 0.35, 0.9))))
        txt = g.math("MULTIPLY", txt, g.math("MULTIPLY", g.math("GREATER_THAN", sep.outputs[1], 0.08),
                                             g.math("LESS_THAN", sep.outputs[1], 0.55)))
        pcol = g.mix(g.math("MULTIPLY", txt, 0.75), lin("#f4f2ec"), lin("#4a4843"))
        rmat = finish(m, g, out, principled(g, pcol, 0.5, 0.0, 0.4, normal=g.bump(g.noise(g.coords("Object"), 600, 4, 0.5), 0.05, 0.0004)),
                      lin("#f4f2ec"))
        # the receipt lies on the block's top and drapes over its front edge, so the half with the stamp hangs
        # square to the lens: 80 mm wide, 100 mm on the stone, a 5 mm roll at the edge, 95 mm down the front
        top_len, hang_len, br_ = 0.10, 0.095, 0.005
        hx0, hy_f, hz_t = -0.17, -0.16 - 0.0004, PLZ / 2 + 0.0004
        path = []
        n1, n2, n3 = 10, 8, 12
        for j in range(n1 + 1):
            path.append((hy_f + br_ + top_len * (1 - j / n1), hz_t))
        for j in range(1, n2 + 1):
            a_ = math.radians(90 * j / n2)
            path.append((hy_f + br_ - br_ * math.sin(a_), hz_t - br_ + br_ * math.cos(a_)))
        for j in range(1, n3 + 1):
            path.append((hy_f, hz_t - br_ - hang_len * j / n3))
        lens_ = [0.0]
        for (y0_, z0_), (y1_, z1_) in zip(path[:-1], path[1:]):
            lens_.append(lens_[-1] + math.hypot(y1_ - y0_, z1_ - z0_))
        rv, rf, ruv = [], [], []
        for (yy, zz), L__ in zip(path, lens_):
            rv += [(hx0 - rw_ / 2, yy, zz), (hx0 + rw_ / 2, yy, zz)]
            ruv += [(0, 1 - L__ / lens_[-1]), (1, 1 - L__ / lens_[-1])]
        for j in range(len(path) - 1):
            rf.append((2 * j, 2 * j + 1, 2 * j + 3, 2 * j + 2))
        rc = mesh_obj("receipt", rv, rf, rmat, smooth=True)
        recalc(rc)
        uvl = rc.data.uv_layers.new(name="UVMap")
        for poly in rc.data.polygons:
            for li in poly.loop_indices:
                uvl.data[li].uv = ruv[rc.data.loops[li].vertex_index]
        md = rc.modifiers.new("solid", "SOLIDIFY")
        md.thickness = 0.0002
        rc.parent = blk_
        ink = mat_plain("stamp_green", "#1f8a49", 0.75)
        cu = bpy.data.curves.new("stamp_ring", "CURVE")
        cu.dimensions = "3D"
        cu.bevel_depth = 0.0007
        sp = cu.splines.new("POLY")
        sp.points.add(47)
        R_ = 0.016                                   # a 32 mm stamp, square to the lens: about 50 px at frame size
        for k in range(48):
            a_ = 2 * math.pi * k / 47
            sp.points[k].co = (R_ * math.cos(a_), R_ * math.sin(a_), 0, 1)
        ring = bpy.data.objects.new("stamp_ring", cu)
        link(ring)
        cu.materials.append(ink)
        ring.parent = blk_
        sc_loc = (hx0 + 0.004, hy_f - 0.0006, hz_t - br_ - 0.024)
        ring.location = sc_loc
        ring.rotation_euler = (math.radians(90), math.radians(-10), 0)
        st = text3d("stamp_sv", "SV", 0.019, sc_loc, (90, -10, 0), ink, 0.0,
                    font=load_font([os.path.join(os.path.dirname(os.path.abspath(__file__)), "..", "example", "sv", "fonts", "Outfit-Bold.ttf")]),
                    parent=blk_)
        # light: a warm golden-hour sun from the left at 15 degrees, through a window and a tree outside, so
        # mullion bars and leaf shadows cross the bag on a diagonal; a cool bounce from the right; everything grounded
        sun_dir = Vector((0.82, 0.42, -math.tan(math.radians(15)) * math.hypot(0.82, 0.42))).normalized()
        sun = bpy.data.lights.new("window_sun", "SUN")
        sun.energy = 7.5
        sun.angle = math.radians(0.45)
        sun.color = kelvin(3300)
        so = bpy.data.objects.new("window_sun", sun)
        link(so, coll("SV_lights"))
        so.rotation_euler = sun_dir.to_track_quat("-Z", "Y").to_euler()
        win = mat_plain("gobo", "#111111", 0.9)
        gobo = []
        # the window: a frame with two mullions, 1.9 m back along the light from the bag
        cen = Vector((TX, WALL_Y - 0.1, TZ + 0.3)) - sun_dir * 1.9
        right_ = sun_dir.cross(Vector((0, 0, 1))).normalized()
        up_ = right_.cross(sun_dir).normalized()
        rot_ = sun_dir.to_track_quat("Z", "Y").to_euler()
        for k in range(3):
            o_ = boxc("gobo_m%d" % k, tuple(cen + right_ * (-0.55 + k * 0.55)), (0.05, 0.05, 2.4), win)
            o_.rotation_euler = rot_
            o_.rotation_euler.rotate_axis("X", math.radians(90))
            gobo.append(o_)
        o_ = boxc("gobo_t", tuple(cen + up_ * 0.55), (2.2, 0.05, 0.05), win)
        o_.rotation_euler = rot_
        gobo.append(o_)
        for sd in (-1, 1):
            o_ = boxc("gobo_wall%d" % sd, tuple(cen + right_ * sd * 2.0), (1.6, 0.02, 3.0), win)
            o_.rotation_euler = rot_
            o_.rotation_euler.rotate_axis("X", math.radians(90))
            gobo.append(o_)
        # a branch of leaves outside, 2.6 m back, running on a diagonal
        rl = random.Random(9)
        lv, lf = [], []
        bc = Vector((TX, WALL_Y - 0.1, TZ + 0.25)) - sun_dir * 1.4
        for k in range(260):
            t = rl.uniform(-1, 1)
            p0 = bc + right_ * (t * 0.8 + rl.gauss(0, 0.1)) + up_ * (-t * 0.7 + rl.gauss(0, 0.1))
            T = (right_ * math.cos(rl.uniform(0, 6.28)) + up_ * math.sin(rl.uniform(0, 6.28))).normalized()
            B = sun_dir.cross(T).normalized()
            ln_, wd_ = rl.uniform(0.07, 0.13), rl.uniform(0.028, 0.048)
            i0 = len(lv)
            # a real leaf outline: round at the base, widest a third along, drawn out to a point, a little curved
            curve = rl.uniform(-0.25, 0.25)
            outline = []
            for q in range(7):
                t_ = q / 6
                outline.append((t_, (math.sin(math.pi * t_ ** 0.75) ** 0.9) * wd_ / 2))
            pts_ = [p0 + T * (t_ * ln_) + B * (w_ + curve * wd_ * t_ * t_) for t_, w_ in outline] + \
                   [p0 + T * (t_ * ln_) + B * (-w_ + curve * wd_ * t_ * t_) for t_, w_ in reversed(outline[1:-1])]
            lv += [tuple(v_) for v_ in pts_]
            lf.append(tuple(range(i0, i0 + len(pts_))))
        gobo.append(mesh_obj("gobo_leaves", lv, lf, win))
        # the box sits in the same sun: a mullion's shadow and two leaves fall across its lid and label
        bpy.context.view_layer.update()
        lidc = bpy.data.objects["DESIGN_box-lid"].matrix_world.translation.copy() if "DESIGN_box-lid" in bpy.data.objects else BOX.matrix_world.translation
        gc = lidc - sun_dir * 0.9
        o_ = boxc("gobo_box_mullion", tuple(gc + right_ * 0.035), (0.045, 0.045, 1.2), win)
        o_.rotation_euler = rot_
        o_.rotation_euler.rotate_axis("X", math.radians(90))
        o_.rotation_euler.rotate_axis("Y", math.radians(-14))
        gobo.append(o_)
        lv2, lf2 = [], []
        for k, (dr, du, ang_, ln_, wd_) in enumerate(((-0.075, 0.03, 0.5, 0.1, 0.04), (-0.05, -0.045, -0.9, 0.085, 0.034),
                                                     (0.11, 0.075, 2.2, 0.09, 0.036))):
            p0 = gc + right_ * dr + up_ * du
            T = (right_ * math.cos(ang_) + up_ * math.sin(ang_)).normalized()
            B = sun_dir.cross(T).normalized()
            outline = [(q / 6, (math.sin(math.pi * (q / 6) ** 0.75) ** 0.9) * wd_ / 2) for q in range(7)]
            pts_ = [p0 + T * (t_ * ln_) + B * w_ for t_, w_ in outline] + [p0 + T * (t_ * ln_) - B * w_ for t_, w_ in reversed(outline[1:-1])]
            i0 = len(lv2)
            lv2 += [tuple(v_) for v_ in pts_]
            lf2.append(tuple(range(i0, i0 + len(pts_))))
        gobo.append(mesh_obj("gobo_box_leaves", lv2, lf2, win))
        for o in gobo:
            o.visible_camera = False
            o.visible_glossy = False
            o.visible_diffuse = False
            o["sv_mask_ignore"] = True
        light("bounce", "AREA", (1.6, -1.6, 1.1), 18, temp=7600, target=(TX, WALL_Y, TZ + 0.2), size=1.6, size_y=1.6)
        light("fill", "AREA", (0.2, -3.2, 1.4), 6, temp=6000, target=(TX, WALL_Y, TZ + 0.2), size=2.0, size_y=1.2)
        world_color(lin("#cfc9bf"), 0.07)
        # 50 mm, level, about 20 percent closer than before: the bag and its straps fill about 65 percent of the
        # height on the left third, the box on its travertine block overlaps the lower right third
        cxc = float(os.environ.get("TOTE_CX", "-0.01"))
        cyc = float(os.environ.get("TOTE_CY", "-1.96"))
        czc = float(os.environ.get("TOTE_CZ", "1.2"))
        cam = camera("SV_camera", (cxc, cyc, czc), (cxc, cyc + 10, czc), lens=50.0, shift_y=float(os.environ.get("TOTE_SY", "0.05")),
                     level=True, fstop=8.0, focus=2.2)
        over = camera("SV_overview", (-2.6, -3.1, 2.4), (0.0, 0.0, 0.95), lens=32.0)
        return {"camera": cam, "overview": over, "exposure": 0.3, "samples": 768, "proxy_scale": 0.35,
                "look": "AgX - Base Contrast",
                "finish": {"k": 0.008, "ca": 0.0008, "vignette": 0.2, "grain": 0.008, "split": 0.4},
                "glare": {"threshold": 3.0, "strength": 0.15, "size": 0.6}}

    # ---------------------------------------------------------------- the logo, in 3D
    LOGO_PARTS = [
        # file, name, thickness (m), material key, explode offset along the axis (m). Seven layers, each at least
        # 120 mm from the next: the tile plate (with the drawn SV on its face), the ghost outline, the word SV (S and
        # V share one layer, so the word keeps its real kerning), the prompt glyph, the three dots
        ("tile.svg", "Tile", 0.020, "smoke", 0.00),
        ("tile-edge.svg", "Tile edge", 0.004, "anodised", 0.13),
        ("letter-s.svg", "Letter S", 0.018, "satin", 0.92),
        ("letter-v.svg", "Letter V", 0.018, "satin", 0.92),
        ("prompt-arrow.svg", "Prompt arrow", 0.008, "acrylic_green", 1.16),
        ("prompt-cursor.svg", "Prompt cursor", 0.008, "acrylic_green", 1.16),
        ("dot-red.svg", "Dot red", 0.009, "dot_red", 1.42),
        ("dot-yellow.svg", "Dot yellow", 0.009, "dot_yellow", 1.42),
        ("dot-green.svg", "Dot green", 0.009, "dot_green", 1.42),
    ]
    LOGO_SIZE = 0.5        # the tile is 50 cm across
    TILE_T = 0.020
    PLATE_T = 0.003        # the inset face plate stands 3 mm proud of the tile
    TILE_HEX = "#0C0C14"
    # the explode axis: out of the tile's face (-Y), rising a little, so the parts climb from the tile at lower left to
    # the dots at upper right
    LOGO_AXIS = (0.0, -1.0, float(os.environ.get("SV_K", "0.26")))

    def logo_materials():
        mats = {}
        m, g, out = new_mat("smoked_acrylic")
        sh = principled(g, (0.006, 0.008, 0.007), 0.14, 0.0, 0.5, transmission=0.15, ior=1.49, coat=0.6, coat_rough=0.1)
        mats["smoke"] = finish(m, g, out, sh, (0.03, 0.04, 0.035))
        m, g, out = new_mat("anodised_navy")
        nz = g.noise(g.coords("Object", (1, 1, 40)), 60, 4, 0.5)
        sh = principled(g, lin("#b9c7dc"), g.maprange(nz, 0.3, 0.7, 0.22, 0.3), 1.0, 0.5, normal=g.bump(nz, 0.02, 0.001))
        mats["anodised"] = finish(m, g, out, sh, lin("#0f1b2d"))
        # the letters: satin off-white lacquer, roughness 0.35, no emission, no coat: they take the key, never clip
        m, g, out = new_mat("satin_offwhite")
        nz = g.noise(g.coords("Object"), 900, 4, 0.5)
        sh = principled(g, lin("#e4e2dc"), g.maprange(nz, 0.3, 0.7, 0.32, 0.38), 0.0, 0.5, normal=g.bump(nz, 0.01, 0.0003))
        mats["satin"] = finish(m, g, out, sh, (0.85, 0.85, 0.83))
        m, g, out = new_mat("acrylic_green")
        c = lin("#16a34a")
        sh = principled(g, c, 0.08, 0.0, 0.5, coat=1.0, coat_rough=0.03, transmission=0.2, ior=1.49)
        mats["acrylic_green"] = finish(m, g, out, sh, c)
        # the dots are the only things that glow: lit acrylic buttons
        for key, hexc, st in (("dot_red", "#ff5f57", 5.0), ("dot_yellow", "#febc2e", 3.6), ("dot_green", "#28c840", 4.4)):
            m, g, out = new_mat(key)
            c = lin(hexc)
            sh = principled(g, c, 0.1, 0.0, 0.5, coat=1.0, coat_rough=0.02, emit=c, emit_strength=st)
            mats[key] = finish(m, g, out, sh, c)
        return mats

    def import_logo_parts(parts_dir):
        """Import each named part SVG as a curve, all on the shared 512 artboard,
        then extrude and bevel it into a real solid. Returns {name: (pivot, obj, info)}."""
        mats = logo_materials()
        out = {}
        scale = None
        center = None
        for fn, nm, thick, mk, off in LOGO_PARTS:
            p = os.path.join(parts_dir, fn)
            before = set(bpy.data.objects)
            bpy.ops.import_curve.svg(filepath=p)
            new = [o for o in bpy.data.objects if o not in before and o.type == "CURVE"]
            if not new:
                print("SV_WARN no curve in", p)
                continue
            if len(new) > 1:
                bpy.ops.object.select_all(action="DESELECT")
                for o in new:
                    o.select_set(True)
                bpy.context.view_layer.objects.active = new[0]
                bpy.ops.object.join()
                new = [bpy.context.view_layer.objects.active]
            ob = new[0]
            for c in list(ob.users_collection):
                c.objects.unlink(ob)
            link(ob, coll("SV_logo"))
            bpy.context.view_layer.update()
            pts = [ob.matrix_world @ Vector(b) for b in ob.bound_box]
            if scale is None:
                # the tile defines the artboard: 512 units become LOGO_SIZE meters
                w = max(p_.x for p_ in pts) - min(p_.x for p_ in pts)
                scale = LOGO_SIZE / w
                center = Vector(((max(p_.x for p_ in pts) + min(p_.x for p_ in pts)) / 2,
                                 (max(p_.y for p_ in pts) + min(p_.y for p_ in pts)) / 2, 0))
            cu = ob.data
            cu.dimensions = "2D"
            cu.fill_mode = "BOTH"
            bev = min(0.0025, thick * 0.3)
            cu.extrude = max(0.0, thick / 2 - bev) / scale
            cu.bevel_depth = bev / scale
            cu.bevel_resolution = 4
            cu.offset = -bev / scale if hasattr(cu, "offset") else 0
            cu.resolution_u = 24
            cu.materials.clear()
            cu.materials.append(mats[mk])
            piv = bpy.data.objects.new("PART_" + nm, None)
            link(piv, coll("SV_logo"))
            piv.rotation_euler = (math.radians(90), 0, 0)
            piv.scale = (scale, scale, scale)
            mw0 = ob.matrix_world.copy()
            ob.parent = piv
            ob.matrix_parent_inverse = Matrix.Identity(4)
            ob.matrix_basis = Matrix.Translation(-center) @ mw0
            ob.name = "LOGO_" + nm
            for poly in getattr(ob.data, "polygons", []):
                pass
            out[nm] = {"pivot": piv, "obj": ob, "thick": thick, "off": off, "mat": mk}
        # bounds in the 512 frame -> world, for guide lines
        return out, scale

    def logo_axis():
        return Vector(LOGO_AXIS).normalized()

    def part_y0(nm, t):
        if nm == "Tile":
            return 0.0
        if nm == "Tile edge":
            return -TILE_T / 2 - t / 2 + 0.0005
        return -TILE_T / 2 - PLATE_T - t / 2 + 0.0002     # the parts sit on the face plate

    def logo_layout(parts, e, base=(0.0, 0.0, 0.42)):
        """Place every part: e = 0 assembled, e = 1 exploded along the axis."""
        bx, by, bz = base
        A = logo_axis()
        for nm, d in parts.items():
            p = Vector((bx, by + part_y0(nm, d["thick"]), bz)) + A * (d["off"] * e)
            d["pivot"].location = tuple(p)
        bpy.context.view_layer.update()

    def dashed_line_mat(name, strength=0.9, dash=0.012):
        """Fine dashed construction line: dashes of a fixed length in metres (the object's sv_len scales them)."""
        m, g, out = new_mat(name)
        gen = g.n("ShaderNodeTexCoord")
        sep = g.n("ShaderNodeSeparateXYZ")
        g.set(sep.inputs[0], gen.outputs["Generated"])
        at = g.n("ShaderNodeAttribute")
        at.attribute_type = "OBJECT"
        at.attribute_name = "sv_len"
        w = g.math("FRACT", g.math("DIVIDE", g.math("MULTIPLY", sep.outputs[2], at.outputs["Fac"]), dash))
        on = g.math("LESS_THAN", w, 0.55)
        em = g.n("ShaderNodeEmission", {"Color": (0.86, 0.93, 0.89), "Strength": strength})
        tr = g.n("ShaderNodeBsdfTransparent")
        mx = g.n("ShaderNodeMixShader")
        g.set(mx.inputs[0], on)
        g.set(mx.inputs[1], tr)
        g.set(mx.inputs[2], em)
        return finish(m, g, out, mx.outputs[0], (0.9, 0.95, 0.92))

    def part_nodes(ob):
        """The part's real anchor nodes (its Bezier points), in the part's own pivot frame."""
        pts = []
        for sp in ob.data.splines:
            for bp in sp.bezier_points:
                pts.append(ob.matrix_basis @ Vector(bp.co))
            for pp in sp.points:
                pts.append(ob.matrix_basis @ Vector(pp.co[:3]))
        return pts

    def build_guides(parts, base, scale):
        """Dashed construction lines, each from a real node on the tile's face to the same node on its part, ending
        exactly on the part's back face. Unit cylinders along the axis, stretched per frame by update_guides()."""
        gm = dashed_line_mat("guide_dash")
        guides = []
        A = logo_axis()
        rot = A.to_track_quat("Z", "Y")
        picks = {"Tile edge": ("tl", "br"), "Letter S": ("top", "bottom"), "Letter V": ("tl", "tr"),
                 "Prompt arrow": ("right",), "Prompt cursor": ("right",),
                 "Dot red": ("top",), "Dot green": ("bottom",)}
        for nm, d in parts.items():
            if nm not in picks:
                continue
            piv = d["pivot"]
            nodes = part_nodes(d["obj"])
            # world positions relative to the pivot: (x, z) in metres relative to the tile centre
            mw = piv.matrix_world
            P = [((mw @ n).x - mw.translation.x, (mw @ n).z - mw.translation.z) for n in nodes]
            chosen = []
            for how in picks[nm]:
                if how == "top":
                    q = max(P, key=lambda v: v[1])
                elif how == "bottom":
                    q = min(P, key=lambda v: v[1])
                elif how == "right":
                    q = max(P, key=lambda v: v[0])
                elif how == "tl":
                    q = max(P, key=lambda v: v[1] - v[0])
                elif how == "tr":
                    q = max(P, key=lambda v: v[1] + v[0])
                elif how == "br":
                    q = max(P, key=lambda v: -v[1] + v[0])
                chosen.append(q)
            for j, (x, z) in enumerate(chosen):
                ob = cyl("GUIDE_%s_%d" % (nm.replace(" ", "_"), j), 0.0006, 1.0, (0, 0, 0), gm, 8, collection=coll("SV_logo"))
                for v in ob.data.vertices:
                    v.co.z += 0.5      # unit cylinder from 0 to 1 along local Z
                ob.rotation_mode = "QUATERNION"
                ob.rotation_quaternion = rot
                ob["sv_mask_ignore"] = True
                ob["sv_len"] = 0.0
                guides.append({"obj": ob, "x": x, "z": z, "part": nm, "thick": d["thick"], "off": d["off"]})
        return guides

    FACE_K = 0.96      # the drawing on the face plate is the 512 artboard at 480 mm: 0.96 of the parts' scale
    # the face plate draws the '>_' a little in from its place in the logo (make-scene-psd.py FACE_SHIFT, artboard units)
    FACE_SHIFT = {"Prompt arrow": (24, 16), "Prompt cursor": (24, 16)}

    def update_guides(guides, e, base):
        """Each line starts at the node as DRAWN on the face plate (0.96 scale) and ends at the same node on its part's
        back face, wherever the part has floated to."""
        bx, by, bz = base
        A = logo_axis()
        y_face = by - TILE_T / 2 - PLATE_T - 0.0004
        for gd in guides:
            sx_, sz_ = FACE_SHIFT.get(gd["part"], (0, 0))
            u_ = LOGO_SIZE * FACE_K / 512.0
            start = Vector((bx + gd["x"] * FACE_K + sx_ * u_, y_face, bz + gd["z"] * FACE_K - sz_ * u_))
            mid = Vector((bx + gd["x"], by + part_y0(gd["part"], gd["thick"]), bz + gd["z"])) + A * (gd["off"] * e)
            end = mid + Vector((0, gd["thick"] / 2, 0))
            d = end - start
            ln = d.length if d.y < 0 else 0.0
            ob = gd["obj"]
            ob.location = tuple(start)
            if ln > 1e-4:
                ob.rotation_quaternion = d.normalized().to_track_quat("Z", "Y")
            ob.scale = (1, 1, max(1e-4, ln))
            ob["sv_len"] = ln
            ob.hide_render = ln < 0.03
            gd["end"] = end
            gd["start"] = start
        bpy.context.view_layer.update()

    LOGO_SHOT = {"az": -35.0, "el": float(os.environ.get("SV_EL", "6")), "dist": float(os.environ.get("SV_D", "2.05")),
                 "lens": 50.0, "shift": (float(os.environ.get("SV_SX", "0")), float(os.environ.get("SV_SY", "-0.03"))),
                 "aim_e": float(os.environ.get("SV_AE", "0.5")), "roll": float(os.environ.get("SV_ROLL", "-9"))}

    def logo_cam_set(cam, az, el, dist, aim, lens=50.0, shift=(0.0, 0.0)):
        """Put the camera on an orbit round the aim point: az degrees off the explode axis (negative = from the left),
        el degrees up, dist metres away."""
        a, e_ = math.radians(az), math.radians(el)
        d = Vector((math.sin(a) * math.cos(e_), -math.cos(a) * math.cos(e_), math.sin(e_)))
        cam.location = tuple(Vector(aim) + d * dist)
        cam.rotation_euler = (-d).to_track_quat("-Z", "Y").to_euler()
        roll = LOGO_SHOT.get("roll", 0.0)
        if roll:
            cam.rotation_euler.rotate_axis("Z", math.radians(roll))
        cam.data.lens = lens
        cam.data.shift_x, cam.data.shift_y = shift
        if cam.data.dof.use_dof:
            cam.data.dof.focus_distance = dist
        bpy.context.view_layer.update()

    def logo_aim(base, e):
        A = logo_axis()
        return Vector(base) + A * (LOGO_SHOT["aim_e"] * 1.42 * e) + Vector((0, 0, 0.0))

    def scene_logo_exploded():
        """The SV logo as a real object, a three-quarter hero: each named vector part extruded, bevelled and given its
        own material, floated apart along one rising axis, on an infinite dark cyc with a soft floor reflection."""
        args = C.get("args")
        parts_dir = getattr(args, "parts", None) or os.path.join(LAUNCH, "sv-logo-parts")
        t0 = time.time()
        while not os.path.exists(os.path.join(parts_dir, "letter-v.svg")):
            if time.time() - t0 > 40 * 60:
                raise SystemExit("SV_ERROR logo parts not found in %s" % parts_dir)
            print("SV_WAIT logo parts not there yet, checking again in a minute")
            time.sleep(60)
        # an infinite cyc: a polished dark floor (roughness 0.15) that sweeps up into a wall far behind, lit only
        # round the logo, so it falls off to near black in every direction
        m, g, out = new_mat("cyc_floor")
        nz = g.noise(g.coords("Object"), 2.5, 6, 0.6)
        fine = g.noise(g.coords("Object"), 140, 6, 0.6)
        sh = principled(g, g.ramp(nz, lin("#0d0f0e"), lin("#0a0c0b"), 0.35, 0.65), g.maprange(nz, 0.3, 0.7, 0.12, 0.18),
                        0.0, 0.5, normal=g.bump(fine, 0.015, 0.001))
        # the cyc's sweep and back wall are hidden from glossy rays: the polished floor must not mirror the patch the
        # key light throws on the wall far behind (it read as a rectangle of light at the bottom of the frame)
        tc_ = g.n("ShaderNodeTexCoord")
        sp_ = g.n("ShaderNodeSeparateXYZ")
        g.set(sp_.inputs[0], tc_.outputs["Object"])
        wall_ = g.math("GREATER_THAN", sp_.outputs[2], 0.02)
        gl_ = g.n("ShaderNodeLightPath").outputs["Is Glossy Ray"]
        hide_ = g.math("MULTIPLY", wall_, gl_)
        blk_ = g.n("ShaderNodeEmission", {"Color": (0, 0, 0), "Strength": 0.0})
        mxc = g.n("ShaderNodeMixShader")
        g.set(mxc.inputs[0], hide_)
        g.set(mxc.inputs[1], sh)
        g.set(mxc.inputs[2], blk_)
        cyc = finish(m, g, out, mxc.outputs[0], lin("#0b0d0c"))
        prof = [(-12.0, 0.0), (5.0, 0.0)]
        for i in range(1, 17):
            a_ = math.radians(90 * i / 16)
            prof.append((5.0 + 2.5 * math.sin(a_), 2.5 - 2.5 * math.cos(a_)))
        prof += [(7.5, 8.0), (7.55, 8.0), (7.55, -0.05), (-12.0, -0.05)]
        prism_x("cyclorama", prof, -14, 14, cyc)
        base = (0.0, 0.0, float(os.environ.get("SV_BZ", "0.30")))
        parts, scale = import_logo_parts(parts_dir)
        for nm, d in parts.items():
            d["obj"]["sv_probe"] = nm
        C["logo_parts"] = parts
        logo_layout(parts, 1.0, base)
        guides = build_guides(parts, base, scale)
        C["guides"] = guides
        C["logo_base"] = base
        update_guides(guides, 1.0, base)
        # the design face: the front of the smoked tile, an inset face plate with the logo's own corner ratio
        fs = LOGO_SIZE * 0.96
        fr = fs * 102.43 / 512
        y_front = base[1] - TILE_T / 2 - PLATE_T
        prof = []
        for (cx_, cz_, a0) in ((fs / 2 - fr, fs / 2 - fr, 0), (-fs / 2 + fr, fs / 2 - fr, 90),
                               (-fs / 2 + fr, -fs / 2 + fr, 180), (fs / 2 - fr, -fs / 2 + fr, 270)):
            for k in range(17):
                a_ = math.radians(a0 + 90 * k / 16)
                prof.append((base[0] + cx_ + fr * math.cos(a_), base[2] + cz_ + fr * math.sin(a_)))
        m, g, out = new_mat("face_plate")
        sh = principled(g, lin(TILE_HEX), 0.62, 0.0, 0.25)
        plate_ob = prism_y("tile_face_plate", prof, y_front, base[1] - TILE_T / 2 + 0.002,
                           finish(m, g, out, sh, lin(TILE_HEX)), bevel=0.0009)
        plate_ob.modifiers["bevel"].segments = 3
        link_logo = coll("SV_logo")
        for c_ in list(plate_ob.users_collection):
            c_.objects.unlink(plate_ob)
        link_logo.objects.link(plate_ob)
        C["face_plate"] = plate_ob
        f = design_face("tile-face", (0, 0, 0), fs, fs, kind="printed", surface="vinyl",
                        label="Tile face 480 x 480 mm, R 96 mm", round_r=fr)
        f.parent = None
        f.location = (base[0], y_front - 0.0002, base[2])
        f["sv_beauty_hide"] = True
        # the polished floor must not mirror the face's grey placeholder (it showed as a light rectangle at the
        # bottom of the frame): glossy rays see the dark face plate behind it instead
        f.visible_glossy = False
        # light: one hard key from the upper right, a green rim at a fifth of its power from behind left, a weak cool
        # fill so the tile's face reads, and a soft overhead pool that lights the floor under the logo only
        aim = logo_aim(base, 1.0)
        light("key", "AREA", (1.9, -2.1, 2.3), 140, temp=5400, target=tuple(aim), size=0.22, size_y=0.22)
        light("rim", "AREA", (-1.2, 1.4, 1.1), 28, color=lin("#22C55E"), target=tuple(aim + Vector((0.1, 0, 0.1))),
              size=0.15, size_y=1.2, spread=30)
        light("fill", "AREA", (-2.2, -2.4, 0.9), 14, temp=7000, target=tuple(aim), size=1.6, size_y=1.6)
        # the floor pool: a round soft source with a wide spread, so it falls off without an edge (a square, narrow
        # one drew a rectangle of light on the floor at the bottom of the frame)
        pool = light("pool", "AREA", tuple(aim + Vector((0.0, 0.0, 2.2))), 45, temp=5200, target=tuple(aim * Vector((1, 1, 0))),
                     size=1.6, size_y=1.6, spread=150)
        pool.data.shape = "DISK"
        for nm_ in ("rim", "key", "fill", "pool"):
            bpy.data.objects[nm_].visible_camera = False
            bpy.data.objects[nm_].visible_glossy = False
        try:
            bpy.data.objects["rim"].light_linking.receiver_collection = coll("SV_logo")
        except Exception as e_:
            print("SV_WARN light linking", e_)
        world_color((0.0015, 0.0018, 0.0017), 1.0)
        cam = camera("SV_camera", (0, -2, 0.5), (0, 0, 0.4), lens=50.0, fstop=9.0, focus=2.0)

        def import_view():
            """01 Import: the nine curves as they come in, laid out on a 3 x 3 grid in front of an orthographic camera,
            each at its true size, so every part can carry its name."""
            hid = []
            for o in bpy.data.objects:
                if (o.name.startswith(("GUIDE_", "DESIGN_")) or o.name in ("tile_face_plate", "cyclorama")) and not o.hide_render:
                    o.hide_render = True
                    hid.append(o)
            cell = 0.62
            for i, (fn_, nm_, *_r) in enumerate(LOGO_PARTS):
                d_ = parts.get(nm_)
                if not d_:
                    continue
                cx_, cz_ = (i % 3 - 1) * cell, 1.95 - (i // 3) * cell
                bpy.context.view_layer.update()
                pts_ = [d_["obj"].matrix_world @ Vector(c_) for c_ in d_["obj"].bound_box]
                c0 = sum(pts_, Vector()) / 8
                d_["pivot"].location = d_["pivot"].location + Vector((cx_ - c0.x, -c0.y, cz_ - c0.z))
            bpy.context.view_layer.update()
            ci = camera("SV_import", (0, -4.0, 1.33), (0, 0, 1.33), lens=50.0)
            ci.data.type = "ORTHO"
            ci.data.ortho_scale = 3.4

            def restore():
                for o in hid:
                    o.hide_render = False
                logo_layout(parts, 1.0, base)
                update_guides(guides, 1.0, base)
            return ci, restore
        sh_ = LOGO_SHOT
        logo_cam_set(cam, sh_["az"], sh_["el"], sh_["dist"], aim, sh_["lens"], sh_["shift"])
        over = camera("SV_overview", (-2.8, -3.6, 1.9), (0.2, -0.45, 0.5), lens=32.0)
        return {"camera": cam, "overview": over, "exposure": 0.2, "samples": 512, "proxy_scale": 0.6, "overlay": ("guides", "GUIDE_"),
                "blockout_cam": "shot", "import_view": import_view,
                "look": "AgX - Medium High Contrast",
                "finish": {"k": 0.006, "ca": 0.0009, "vignette": 0.26, "grain": 0.008, "split": 0.3},
                "glare": {"threshold": 2.6, "strength": 0.35, "size": 0.6},
                "extra": logo_extra}

    def logo_extra(args, out, cfg, samples):
        """Beauty renders of the logo with the real tile artwork on its face: the hero (exploded), the assembled
        state, the LED framings, and the explode frames (a 40 degree orbit and a dolly in while the parts separate)."""
        sc = bpy.context.scene
        parts = C["logo_parts"]
        guides = C["guides"]
        base = C["logo_base"]
        fin = cfg.get("finish", {})
        cam = sc.camera
        art = os.path.join(LAUNCH, "scene-art", "logo-exploded-3d--tile-a.png")
        rep = os.path.join(HERE, ".build", "scene-psd", "report.json")
        if os.path.exists(art):
            try:
                tf_ = json.load(open(rep))["logo-exploded-3d"]["faces"]["tile-face"]
                C["design_bleed"] = tf_["bleed_px"] / tf_["art_px"][0]
            except Exception:
                C["design_bleed"] = 0.04 / 1.08
            set_face_mode("design", art)
            print("SV_BEAUTY tile art on the face, bleed %.4f" % C["design_bleed"])
        else:
            print("SV_WARN no tile art yet (run make-scene-psd.py first): the beauty face shows the bare plate")
            for f in FACES:
                f["obj"].hide_render = True
        setup_cycles(sc, samples)
        set_view(sc, True, cfg.get("exposure", 0.0), cfg.get("look"))
        setup_compositor(sc, cfg.get("glare"))
        tmp = os.path.join(out, "stages", "_beauty_raw.png")
        res = {}
        sh_ = LOGO_SHOT
        aim1 = logo_aim(base, 1.0)
        # the LED framing: the same camera, a wider lens and a shift, so the parts sit in the screen's left half
        led = dict(lens=33.0, shift=(0.225, 0.02))
        for label, e, lens, shift in (("exploded", 1.0, sh_["lens"], sh_["shift"]), ("assembled", 0.0, sh_["lens"], sh_["shift"]),
                                      ("led", 1.0, led["lens"], led["shift"]), ("led-assembled", 0.0, led["lens"], led["shift"])):
            logo_layout(parts, e, base)
            update_guides(guides, e, base)
            logo_cam_set(cam, sh_["az"], sh_["el"], sh_["dist"], aim1, lens, shift)
            res[label] = render_to(tmp)
            write_png(os.path.join(out, "beauty-%s.png" % label), lens_finish(read_png(tmp)[..., :3], fin, "color"))
        n = args.nframes
        if n and args.frames:
            os.makedirs(args.frames, exist_ok=True)
            setup_cycles(sc, max(128, samples // 3))
            t1 = time.time()
            for i in range(n):
                t = i / (n - 1)
                ease = 0.5 - 0.5 * math.cos(math.pi * t)                  # ease in-out on the explode
                e = 0.5 - 0.5 * math.cos(math.pi * min(1.0, t * 1.08))
                logo_layout(parts, e, base)
                update_guides(guides, e, base)
                # a 40 degree orbit from near square-on to the hero angle, dollying in, the aim following the parts
                az = sh_["az"] + 40.0 * (1 - ease)
                dist = sh_["dist"] * (1.0 + 0.32 * (1 - ease))
                el = sh_["el"] + 4.0 * (1 - ease)
                aim = logo_aim(base, 0.0).lerp(aim1, ease)
                sx = sh_["shift"][0] * ease
                sy = sh_["shift"][1] * ease
                logo_cam_set(cam, az, el, dist, aim, sh_["lens"], (sx, sy))
                render_to(tmp)
                write_png(os.path.join(args.frames, "logo-explode-%03d.png" % i),
                          lens_finish(read_png(tmp)[..., :3], fin, "color", seed=100 + i))
            res["frames"] = time.time() - t1
        os.remove(tmp)
        logo_layout(parts, 1.0, base)
        update_guides(guides, 1.0, base)
        logo_cam_set(cam, sh_["az"], sh_["el"], sh_["dist"], aim1, sh_["lens"], sh_["shift"])
        for f in FACES:
            f["obj"].hide_render = False
        set_face_mode("plate")
        C.pop("design_bleed", None)
        return res

    # ---------------------------------------------------------------- print still life
    def find_panels(arr, min_frac=0.6):
        """Find the big rectangles (artboards) on a page preview: rows and columns
        where most pixels differ from the page colour. Returns [(x0, y0, x1, y1)]."""
        h, w = arr.shape[:2]
        bg = np.median(arr[2:12, 2:12, :3].reshape(-1, 3), axis=0)
        diff = np.abs(arr[..., :3] - bg).sum(axis=2) > 0.07
        if diff.mean() > 0.9:
            return [(0, 0, w, h)]          # full-bleed artwork, nothing around it
        rows = diff.mean(axis=1) > min_frac * 0.5
        bands = []
        y = 0
        while y < h:
            if rows[y]:
                y0 = y
                while y < h and rows[y]:
                    y += 1
                if y - y0 > h * 0.08:
                    bands.append((y0, y))
            y += 1
        out = []
        for y0, y1 in bands:
            cols = diff[y0:y1].mean(axis=0) > min_frac
            x = 0
            while x < w:
                if cols[x]:
                    x0 = x
                    while x < w and cols[x]:
                        x += 1
                    if x - x0 > w * 0.15:
                        out.append((x0, y0, x, y1))
                x += 1
        return out

    def crop_to_trim(arr, box_, trim_w, trim_h, bleed):
        x0, y0, x1, y1 = box_
        r = (x1 - x0) / (y1 - y0)
        r_trim = trim_w / trim_h
        r_bleed = (trim_w + 2 * bleed) / (trim_h + 2 * bleed)
        if abs(r - r_bleed) < abs(r - r_trim):
            ix = int(round((x1 - x0) * bleed / (trim_w + 2 * bleed)))
            iy = int(round((y1 - y0) * bleed / (trim_h + 2 * bleed)))
            x0, x1, y0, y1 = x0 + ix, x1 - ix, y0 + iy, y1 - iy
        return arr[y0:y1, x0:x1].copy()

    def prep_print_textures(card_png, banner_png, outdir):
        os.makedirs(outdir, exist_ok=True)
        res = {}
        if os.path.exists(card_png):
            a = read_png(card_png)[..., :3]
            panels = find_panels(a)
            if len(panels) >= 2:
                for nm, pb in zip(("card-front", "card-back"), panels[:2]):
                    c = crop_to_trim(a, pb, 90, 54, 3)
                    p = os.path.join(outdir, "_%s.png" % nm)
                    write_png(p, c)
                    res[nm] = p
            print("SV_CARD panels", panels)
        if os.path.exists(banner_png):
            a = read_png(banner_png)[..., :3]
            panels = find_panels(a)
            pb = max(panels, key=lambda b: (b[2] - b[0]) * (b[3] - b[1])) if panels else (0, 0, a.shape[1], a.shape[0])
            c = crop_to_trim(a, pb, 850, 2000, 10)
            p = os.path.join(outdir, "_banner.png")
            write_png(p, c)
            res["banner"] = p
            print("SV_BANNER panels", panels)
        return res

    def print_material(name, image_path, kind="card", foil=False):
        """Printed stock with the real artwork: uncoated card with a satin feel; green
        ink becomes metallic foil (foil=True) or a glossy spot varnish (foil=False)."""
        m, g, out = new_mat(name)
        img = bpy.data.images.load(image_path, check_existing=False)
        tex = g.n("ShaderNodeTexImage")
        tex.image = img
        tex.interpolation = "Cubic"
        uv = g.n("ShaderNodeTexCoord").outputs["UV"]
        g.set(tex.inputs["Vector"], uv)
        col = tex.outputs["Color"]
        sep = g.n("ShaderNodeSeparateColor")
        g.set(sep.inputs[0], col)
        # SV green mask: green clearly above red and blue
        gr = g.math("SUBTRACT", sep.outputs[1], g.math("MAXIMUM", sep.outputs[0], sep.outputs[2]))
        mask = g.maprange(gr, 0.08, 0.16, 0.0, 1.0)
        paper = g.noise(g.coords("Object", (1, 1, 1)), 900.0, 6, 0.6)
        nrm = g.bump(paper, 0.03 if kind == "card" else 0.015, 0.0005)
        rough = g.mix(mask, (0.62, 0.62, 0.62), (0.16, 0.16, 0.16) if foil else (0.06, 0.06, 0.06))
        rr = g.n("ShaderNodeSeparateColor")
        g.set(rr.inputs[0], rough)
        metal = g.math("MULTIPLY", mask, 1.0 if foil else 0.0)
        coat = g.math("MULTIPLY", mask, 0.0 if foil else 1.0)
        base = g.mix(g.math("MULTIPLY", mask, 1.0 if foil else 0.0), col, lin("#33d27a"))
        sh = principled(g, base, rr.outputs[0], 0.0, 0.45, normal=nrm, coat=0.001)
        g.set(g.sock(sh.inputs, "Metallic"), metal)
        g.set(g.sock(sh.inputs, "Coat Weight"), coat)
        return finish(m, g, out, sh, (0.8, 0.8, 0.8))

    def card_object(name, center, rot_z, face_front_up=True, parent=None, thick=0.0006):
        """A 90 x 54 mm card blank (the white core shows on the edges)."""
        core = mat_plain("card_core", "#f1efe9", 0.8)
        ob = boxc(name, center, (0.09, 0.054, thick), core, rot_z=rot_z, bevel=0.0001, parent=parent)
        return ob

    def card_mesh(name, center, rot_z, mat, parent=None, bow=0.0006, thick=0.00035, tilt=0.0):
        """A 90 x 54 mm card, 0.35 mm thick, slightly bowed, printed on top (mat) with a white core on its edges."""
        nu, nv = 10, 6
        verts, faces, uvs = [], [], []
        for j in range(nv + 1):
            for i in range(nu + 1):
                u, v = i / nu, j / nv
                verts.append(((u - 0.5) * 0.09, (v - 0.5) * 0.054, bow * math.sin(math.pi * u) * (0.6 + 0.4 * math.sin(math.pi * v))))
        for j in range(nv):
            for i in range(nu):
                a_ = j * (nu + 1) + i
                faces.append((a_, a_ + 1, a_ + nu + 2, a_ + nu + 1))
        ob = mesh_obj(name, verts, faces, [mat, mat_plain("card_core", "#f4f2ec", 0.8)], smooth=True)
        uvl = ob.data.uv_layers.new(name="UVMap")
        for poly in ob.data.polygons:
            for li in poly.loop_indices:
                x, y, _ = verts[ob.data.loops[li].vertex_index]
                uvl.data[li].uv = (x / 0.09 + 0.5, y / 0.054 + 0.5)
        md = ob.modifiers.new("solid", "SOLIDIFY")
        md.thickness = thick
        md.offset = -1.0
        md.use_rim = True
        md.material_offset_rim = 1
        md.material_offset = 1
        if parent is not None:
            ob.parent = parent
        ob.location = center
        ob.rotation_euler = (math.radians(tilt), 0, math.radians(rot_z))
        return ob

    def proof_sheet_image(name, front_png, back_png, w=1240, h=1754):
        """An A4 proof at 150 dpi: the card's front and back imposed three times with crop marks, a colour bar."""
        img = np.ones((h, w, 3), np.float32) * np.array(lin("#f6f5f1"), np.float32)
        k = w / 210.0                                     # px per mm
        cw, ch = int(90 * k), int(54 * k)
        arts = []
        for pth in (front_png, back_png):
            if pth and os.path.exists(pth):
                a_ = read_png(pth)[..., :3]
                yy = (np.linspace(0, a_.shape[0] - 1, ch)).astype(int)
                xx = (np.linspace(0, a_.shape[1] - 1, cw)).astype(int)
                arts.append(np.power(np.clip(a_[yy][:, xx], 0, 1), 2.2))
            else:
                arts.append(np.ones((ch, cw, 3), np.float32) * 0.5)
        ink = np.array(lin("#1a1a1a"), np.float32)
        for r in range(3):
            for c in range(2):
                x0 = int((12 + c * 96) * k)
                y0 = int((22 + r * 66) * k)
                img[y0:y0 + ch, x0:x0 + cw] = arts[c]
                for (cx_, cy_) in ((x0, y0), (x0 + cw, y0), (x0, y0 + ch), (x0 + cw, y0 + ch)):
                    sx = -1 if cx_ == x0 else 1
                    sy = -1 if cy_ == y0 else 1
                    a0, a1 = sorted((cx_ + sx * int(2 * k), cx_ + sx * int(7 * k)))
                    img[cy_:cy_ + 1, a0:a1] = ink
                    b0, b1 = sorted((cy_ + sy * int(2 * k), cy_ + sy * int(7 * k)))
                    img[b0:b1, cx_:cx_ + 1] = ink
        bars = ["#00a0e9", "#e4007f", "#fff100", "#1a1a1a", "#7fcff4", "#f19ec2", "#fff799", "#7d7d7d", "#c7c7c7", "#22c55e", "#197b40", "#ffffff"]
        bw_ = int(14 * k)
        for i, c in enumerate(bars):
            x0 = int(12 * k) + i * bw_
            img[int(222 * k):int(232 * k), x0:x0 + bw_ - 2] = np.array(lin(c), np.float32)
        im = bpy.data.images.new(name, w, h, alpha=False, float_buffer=True)
        im.pixels.foreach_set(np.concatenate([img, np.ones((h, w, 1), np.float32)], axis=2)[::-1].astype(np.float32).ravel())
        im.pack()
        return im

    def scene_print_still_life():
        """A print shop's proof table in hard window light: the business card standing in a slotted brass stand, a fan
        of five cards (three backs, two fronts) on the printed proof sheet with its crop marks and colour bar, a linen
        tester standing open over a crop mark. Blind slats break the sun across the table. The standing back and the top front are the
        design faces; the other cards and the proof wear the real Act 3 print file."""
        args = C.get("args")
        card_png = getattr(args, "card", None) or os.path.join(LAUNCH, "sv-business-card.png")
        tex = prep_print_textures(card_png, card_png, os.path.join(args.out if args else ".", "print-still-life", "stages"))
        trav = mat_surface("proof_table", "#d8d2c6", "#cbc4b6", 2.0, (0.5, 0.7), 0.05, stretch=(1, 5, 1), fine=160)
        box("table", -1.2, 1.2, -0.8, 0.8, 0.70, 0.75, trav, 0.004)
        box("table_base", -0.5, 0.5, -0.4, 0.4, 0.0, 0.70, trav)
        box("floor", -6, 6, -6, 6, -0.02, 0.0, mat_surface("floor_dark", "#4d4a45", "#3e3b37", 2, (0.6, 0.8), 0.05))
        box("back_wall", -6, 6, 2.2, 2.3, 0, 4, mat_surface("wall_warm", "#d9d3c8", "#cfc8bb", 1.2, (0.8, 0.95), 0.04))
        top = 0.75
        P = group("proof_g", (0.0, 0.0, top), 0)
        pf = tex.get("card-front")
        pb = tex.get("card-back")
        # the proof sheet, A4, a little askew, slightly curled at one corner
        im = proof_sheet_image("proof_img", pf, pb)
        m, g, out = new_mat("proof_paper")
        t_ = g.n("ShaderNodeTexImage")
        t_.image = im
        g.set(t_.inputs["Vector"], g.n("ShaderNodeTexCoord").outputs["UV"])
        sh = principled(g, t_.outputs["Color"], 0.62, 0.0, 0.4, normal=g.bump(g.noise(g.coords("Object"), 700, 6, 0.6), 0.04, 0.0004))
        paper = finish(m, g, out, sh, (0.9, 0.9, 0.9))
        nu, nv = 12, 16
        verts, faces = [], []
        for j in range(nv + 1):
            for i in range(nu + 1):
                u, v = i / nu, j / nv
                curl = 0.006 * max(0.0, u + v - 1.55) ** 2 / 0.2
                verts.append(((u - 0.5) * 0.21, (v - 0.5) * 0.297, 0.0003 + curl))
        for j in range(nv):
            for i in range(nu):
                a_ = j * (nu + 1) + i
                faces.append((a_, a_ + 1, a_ + nu + 2, a_ + nu + 1))
        sheet = mesh_obj("proof_sheet", verts, faces, paper, smooth=True)
        uvl = sheet.data.uv_layers.new(name="UVMap")
        for poly in sheet.data.polygons:
            for li in poly.loop_indices:
                x, y, _ = verts[sheet.data.loops[li].vertex_index]
                uvl.data[li].uv = (x / 0.21 + 0.5, y / 0.297 + 0.5)
        sheet.parent = P
        sheet.location = (0.02, 0.0, 0.0)
        sheet.rotation_euler = (0, 0, math.radians(-8))
        # the fan of five: three backs and two fronts showing, the top one a design face
        mf = print_material("PRINT_card-front_stack", pf, "card") if pf else mat_plain("cardf", "#eeeeea", 0.6)
        mb = print_material("PRINT_card-back_stack", pb, "card", foil=True) if pb else mat_plain("cardb", "#151515", 0.6)
        piv = Vector((-0.02, -0.06, 0.0))
        order = [mb, mb, mf, mb]
        zc = 0.0006
        for k, mm in enumerate(order):
            ang = -38 + k * 8.5
            off = Matrix.Rotation(math.radians(ang), 3, "Z") @ Vector((0.045 - 0.008, 0.027 - 0.006, 0))
            card_mesh("fan%d" % k, tuple(piv + off + Vector((0, 0, zc))), ang, mm, P, bow=0.00012)
            zc += 0.0004
        ang = -38 + 4 * 8.5
        off = Matrix.Rotation(math.radians(ang), 3, "Z") @ Vector((0.045 - 0.008, 0.027 - 0.006, 0))
        cpos = piv + off + Vector((0, 0, zc))
        card_mesh("fan_top_blank", tuple(cpos), ang, mat_plain("card_core", "#f4f2ec", 0.8), P, bow=0.0)
        design_face("card-front", tuple(cpos + Vector((0, 0, 0.00006))), 0.09, 0.054, tilt=90, yaw=ang, kind="printed",
                    surface="card", label="Business card front 90 x 54 mm", parent=P, subdiv=6, bulge=0.0)
        # the brass stand with a slot; the card's bottom edge sits 3 mm down inside it, square to the slot
        brass = mat_brushed_brass("brass")
        S = group("stand", (0.075, 0.075, 0.0), -14, P)
        lean = 14.0
        for (y0_, y1_) in ((-0.016, -0.0007), (0.0036, 0.016)):
            box("stand_jaw%.3f" % y0_, -0.055, 0.055, y0_, y1_, 0.0, 0.014, brass, 0.0012, parent=S)
        box("stand_base", -0.055, 0.055, -0.016, 0.016, 0.0, 0.006, brass, 0.0012, parent=S)
        SLOT_DEPTH = 0.003
        ccz = 0.014 - SLOT_DEPTH + 0.027 * math.cos(math.radians(lean))
        ccy = 0.027 * math.sin(math.radians(lean))
        card_mesh("stand_card_blank", (0, ccy, ccz), 0, mat_plain("card_core", "#f4f2ec", 0.8), S, bow=0.0, tilt=90 - lean)
        design_face("card-back", (0, ccy - 0.0002, ccz), 0.09, 0.054, tilt=lean, kind="printed", surface="card",
                    label="Business card back 90 x 54 mm", parent=S)
        # a printer's linen tester standing open on the proof over a crop mark: black metal base frame with a
        # 25 mm opening, a back panel hinged to it, the lens plate on top; the lens is real glass (IOR 1.5, biconvex,
        # f about 45 mm) 34 mm above the paper, so it shows the crop mark and the card's edge magnified
        cm_local = Vector((0.012 - 0.105, 0.1485 - 0.088, 0.0))         # col 0, row 1, top-left crop corner (sheet space)
        cm = Matrix.Rotation(math.radians(-8), 3, "Z") @ cm_local + Vector((0.02, 0.0, 0.0))
        # turned side-on to the lens, so the sight line through the lens leaves through the open far side and
        # lands on the crop mark 45 mm beyond the tester: the lens shows it magnified
        LT = group("linen_tester", (cm.x + 0.004, cm.y - 0.045, 0.0006), -8 + 90, P)
        blk_m = mat_metal("tester_black", "#141414", 0.32)
        ap, fw, ft = 0.025, 0.0035, 0.0016               # aperture, frame bar width, plate thickness
        o2 = ap / 2 + fw
        for nm_, x0_, x1_, y0_, y1_ in (("l", -o2, -ap / 2, -o2, o2), ("r", ap / 2, o2, -o2, o2),
                                        ("f", -ap / 2, ap / 2, -o2, -ap / 2), ("b", -ap / 2, ap / 2, ap / 2, o2)):
            box("tester_base_" + nm_, x0_, x1_, y0_, y1_, 0.0, ft, blk_m, 0.0003, parent=LT)
        HZ = 0.034
        box("tester_back", -o2, o2, o2 - 0.0012, o2, ft, HZ, blk_m, 0.0003, parent=LT)
        for sx in (-1, 1):
            cyl("tester_hinge%d" % sx, 0.0011, 0.004, (sx * (o2 - 0.002), o2 + 0.0004, ft + 0.0005), blk_m, 12,
                rot=(0, math.radians(90), 0), parent=LT)
            cyl("tester_hinge_t%d" % sx, 0.0011, 0.004, (sx * (o2 - 0.002), o2 + 0.0004, HZ - 0.0005), blk_m, 12,
                rot=(0, math.radians(90), 0), parent=LT)
        # the lens plate: a square plate with a round seat, a chrome ring round the lens
        plate_r = 0.0128
        pv_, pf_ = [], []
        nseg_ = 48
        for i in range(nseg_):
            a_ = 2 * math.pi * i / nseg_
            pv_.append((plate_r * math.cos(a_), plate_r * math.sin(a_), 0.0))
            # matching point on the square
            c_, s_ = math.cos(a_), math.sin(a_)
            t_ = o2 / max(abs(c_), abs(s_))
            pv_.append((t_ * c_, t_ * s_, 0.0))
        for i in range(nseg_):
            j_ = (i + 1) % nseg_
            pf_.append((2 * i, 2 * i + 1, 2 * j_ + 1, 2 * j_))
        lp = mesh_obj("tester_lensplate", pv_, pf_, blk_m)
        recalc(lp)
        md_ = lp.modifiers.new("solid", "SOLIDIFY")
        md_.thickness = ft
        lp.parent = LT
        lp.location = (0, 0, HZ)
        torus("tester_ring", plate_r, 0.0007, (0, 0, HZ + 0.0002), mat_chrome("chrome"), 48, 8, axis="Z", parent=LT)
        # the biconvex lens: two spherical caps, radius 45 mm, 12.5 mm aperture
        Rl, rl_ = 0.045, 0.0125
        sag = Rl - math.sqrt(Rl * Rl - rl_ * rl_)
        lv_, lf_ = [], []
        nr, na = 8, 48
        for side in (1, -1):
            base_i = len(lv_)
            lv_.append((0.0, 0.0, side * (sag + 0.0004)))
            for r_i in range(1, nr + 1):
                rr_ = rl_ * r_i / nr
                zz_ = side * (math.sqrt(Rl * Rl - rr_ * rr_) - (Rl - sag) + 0.0004)
                for a_i in range(na):
                    a_ = 2 * math.pi * a_i / na
                    lv_.append((rr_ * math.cos(a_), rr_ * math.sin(a_), zz_))
            for a_i in range(na):
                b_ = (a_i + 1) % na
                f_ = (base_i, base_i + 1 + a_i, base_i + 1 + b_)
                lf_.append(f_ if side == 1 else tuple(reversed(f_)))
            for r_i in range(nr - 1):
                for a_i in range(na):
                    b_ = (a_i + 1) % na
                    i0 = base_i + 1 + r_i * na
                    i1 = base_i + 1 + (r_i + 1) * na
                    q_ = (i0 + a_i, i1 + a_i, i1 + b_, i0 + b_)
                    lf_.append(q_ if side == 1 else tuple(reversed(q_)))
        top0 = 1 + (nr - 1) * na
        bot0 = len(lv_) - na
        for a_i in range(na):                             # the rim joins the two caps
            b_ = (a_i + 1) % na
            lf_.append((top0 + a_i, top0 + b_, bot0 + b_, bot0 + a_i))
        m, g, out = new_mat("tester_glass")
        glass_ = principled(g, (1, 1, 1), 0.0, 0.0, 0.5, transmission=1.0, ior=1.5)
        lens_ob = mesh_obj("tester_lens", lv_, lf_, finish(m, g, out, glass_, (0.85, 0.9, 0.9)), smooth=True)
        recalc(lens_ob)
        lens_ob.parent = LT
        lens_ob.location = (0, 0, HZ + ft / 2)
        beam("pen", (-0.2, -0.12, 0.0045), (-0.07, -0.17, 0.0045), 0.0045, mat_paint("pen", "#1b1d1c", 0.35, 0.6), 20, parent=P)
        # light: one hard sun from the left at 20 degrees, broken by blind slats; a weak cool fill
        sun_dir = Vector((0.9, 0.25, -math.tan(math.radians(20)) * math.hypot(0.9, 0.25))).normalized()
        sun = bpy.data.lights.new("window_sun", "SUN")
        sun.energy = 6.0
        sun.angle = math.radians(0.6)
        sun.color = kelvin(4800)
        so = bpy.data.objects.new("window_sun", sun)
        link(so, coll("SV_lights"))
        so.rotation_euler = sun_dir.to_track_quat("-Z", "Y").to_euler()
        right_ = sun_dir.cross(Vector((0, 0, 1))).normalized()
        up_ = right_.cross(sun_dir).normalized()
        cen = Vector((0.0, 0.0, top)) - sun_dir * 1.4
        blind = mat_plain("blind", "#222222", 0.8)
        gob = []
        for k in range(-12, 13):
            o_ = boxc("slat%d" % k, tuple(cen + up_ * (k * 0.045)), (2.4, 0.03, 0.002), blind)
            o_.rotation_euler = sun_dir.to_track_quat("Z", "Y").to_euler()
            o_.rotation_euler.rotate_axis("Z", math.radians(0))
            gob.append(o_)
        for o in gob:
            o.visible_camera = False
            o.visible_glossy = False
            o.visible_diffuse = False
            o["sv_mask_ignore"] = True
        light("fill", "AREA", (1.2, -1.0, 1.4), 9, temp=7500, target=(0.0, 0.0, top), size=1.6, size_y=1.6)
        light("bounce", "AREA", (0.0, -1.6, 0.9), 4, temp=6000, target=(0.0, 0.0, top), size=1.6, size_y=0.8)
        world_color(lin("#d9d3c8"), 0.08)
        # 85 mm, 35 degrees down, the cards at about half the frame
        d_ = float(os.environ.get("PRINT_D", "0.8"))
        el = math.radians(35)
        aim = Vector((0.045, 0.02, top + 0.012))
        az = math.radians(float(os.environ.get("PRINT_AZ", "-8")))
        loc = aim + Vector((math.sin(az) * math.cos(el), -math.cos(az) * math.cos(el), math.sin(el))) * d_
        cam = camera("SV_camera", tuple(loc), tuple(aim), lens=85.0, fstop=8.0, focus=d_)
        over = camera("SV_overview", (-1.4, -1.9, 1.7), (0.0, 0.0, 0.75), lens=35.0)
        for f in FACES:
            if f["name"] in tex:
                f["image"] = tex[f["name"]]
        return {"camera": cam, "overview": over, "exposure": -0.2, "samples": 640, "proxy_scale": 0.3,
                "look": "AgX - Medium High Contrast",
                "finish": {"k": 0.006, "ca": 0.0007, "vignette": 0.18, "grain": 0.008, "split": 0.3},
                "glare": {"threshold": 4.0, "strength": 0.1, "size": 0.5},
                "extra": print_extra}

    def print_extra(args, out, cfg, samples):
        """Beauty renders with the real print files on the cards: the hero shot, and a wide shot of the whole set."""
        sc = bpy.context.scene
        fin = cfg.get("finish", {})
        res = {}
        for f in FACES:
            me = f["obj"].data
            me.materials.clear()
            if f.get("image"):
                me.materials.append(print_material("PRINT_" + f["name"], f["image"], "card", foil=(f["name"] == "card-back")))
            else:
                me.materials.append(face_material(f, "plate"))
        setup_cycles(sc, samples)
        set_view(sc, True, cfg.get("exposure", 0.0), cfg.get("look"))
        setup_compositor(sc, cfg.get("glare"))
        tmp = os.path.join(out, "stages", "_beauty_raw.png")
        main = sc.camera
        for label, cam_ in (("hero", main),):
            sc.camera = cam_
            res[label] = render_to(tmp)
            write_png(os.path.join(out, "beauty-%s.png" % label), lens_finish(read_png(tmp)[..., :3], fin, "color"))
        sc.camera = main
        os.remove(tmp)
        return res

    # ---------------------------------------------------------------- your own .blend
    def own_scene_size():
        global W, H
        sc = bpy.context.scene
        W, H = sc.render.resolution_x, sc.render.resolution_y

    def own_camera(args):
        sc = bpy.context.scene
        cam = bpy.data.objects.get(args.camera) if getattr(args, "camera", None) else sc.camera
        if cam is None or cam.type != "CAMERA":
            cams = [o.name for o in bpy.data.objects if o.type == "CAMERA"]
            raise SystemExit("SV_ERROR no camera to render from: pass --camera with one of %s" % cams)
        return cam

    def own_face_corners(ob, cam):
        """The four corners of a flat object, in its own space, ordered TL, TR, BR, BL as the camera sees them.
        Four vertices: those. Otherwise the side of its bounding box that faces the camera (thinnest axis)."""
        sc = bpy.context.scene
        me = ob.data
        if len(me.vertices) == 4:
            loc = [Vector(v.co) for v in me.vertices]
        else:
            bb = [Vector(c) for c in ob.bound_box]
            lo = Vector((min(c.x for c in bb), min(c.y for c in bb), min(c.z for c in bb)))
            hi = Vector((max(c.x for c in bb), max(c.y for c in bb), max(c.z for c in bb)))
            dims = [(hi - lo)[i] * ob.matrix_world.col[i].to_3d().length for i in range(3)]   # in meters, scale included
            a = min(range(3), key=lambda i: dims[i])
            u, v = [i for i in range(3) if i != a]
            best = None
            for side in (lo[a], hi[a]):
                pts = []
                for pu, pv in ((lo[u], lo[v]), (hi[u], lo[v]), (hi[u], hi[v]), (lo[u], hi[v])):
                    p_ = Vector((0.0, 0.0, 0.0))
                    p_[a], p_[u], p_[v] = side, pu, pv
                    pts.append(p_)
                cen = ob.matrix_world @ (sum(pts, Vector()) / 4)
                d_ = (cen - cam.matrix_world.translation).length
                if best is None or d_ < best[0]:
                    best = (d_, pts)
            loc = best[1]
        scr = []
        for p_ in loc:
            ndc = world_to_camera_view(sc, cam, ob.matrix_world @ p_)
            scr.append((ndc.x * W, (1.0 - ndc.y) * H))
        cx = sum(x for x, _ in scr) / 4
        cy = sum(y for _, y in scr) / 4
        order = sorted(range(4), key=lambda i: math.atan2(scr[i][1] - cy, scr[i][0] - cx))   # clockwise on screen
        k = min(range(4), key=lambda j: scr[order[j]][0] + scr[order[j]][1])                    # top-left first
        order = order[k:] + order[:k]
        return [loc[i] for i in order]

    def scene_own_blend():
        args = C["args"]
        own_scene_size()
        cam = own_camera(args)
        if not getattr(args, "face", None):
            raise SystemExit("SV_ERROR name the design face with --face (list the candidates with --objects)")
        for nm in [x.strip() for x in args.face.split(",") if x.strip()]:
            ob = bpy.data.objects.get(nm)
            if ob is None or ob.type != "MESH":
                raise SystemExit("SV_ERROR no mesh object named %r (list them with --objects)" % nm)
            loc = own_face_corners(ob, cam)
            mw = ob.matrix_world
            w_ = (mw @ loc[1] - mw @ loc[0]).length
            h_ = (mw @ loc[3] - mw @ loc[0]).length
            ob["sv_corners_local"] = [list(p_) for p_ in loc]
            ob["sv_kind"] = args.kind
            ob["sv_size_m"] = [w_, h_]
            safe = "".join(ch if ch.isalnum() else "-" for ch in nm.lower()).strip("-") or "face"
            FACES.append({"name": safe, "obj": ob, "kind": args.kind,
                          "surface": "lightbox" if args.kind == "emissive" else "paper",
                          "emit": 6.0, "label": nm, "size": (w_, h_)})
        sc = bpy.context.scene
        return {"camera": cam, "finish": {}, "samples": args.samples or 128, "exposure": sc.view_settings.exposure,
                "look": sc.view_settings.look}

    def list_own_objects(args):
        """Every mesh the camera sees, largest on screen first: the candidates for a design face."""
        bpy.ops.wm.open_mainfile(filepath=os.path.abspath(args.blend), load_ui=False)
        own_scene_size()
        cam = own_camera(args)
        rows = []
        for o in bpy.data.objects:
            if o.type != "MESH" or o.hide_render:
                continue
            b = px_bbox(o, cam)
            if not b:
                continue
            x0, y0, x1, y1 = max(0, b[0]), max(0, b[1]), min(W, b[2]), min(H, b[3])
            if x1 <= x0 or y1 <= y0:
                continue
            d = o.dimensions
            flat = min(d) <= 0.02 * max(d) if max(d) > 0 else False
            rows.append({"name": o.name, "screen_box_px": [x0, y0, x1, y1],
                         "screen_share": round((x1 - x0) * (y1 - y0) / float(W * H), 4),
                         "size_m": [round(v, 3) for v in d], "vertices": len(o.data.vertices), "flat": flat,
                         "materials": [m.name for m in o.data.materials if m]})
        rows.sort(key=lambda r: -r["screen_share"])
        for r in rows:
            print("SV_OBJECT " + json.dumps(r, ensure_ascii=False))
        print("SV_CAMERA %s %dx%d" % (cam.name, W, H))

    PRESETS = {
        "skytrain-billboard": scene_skytrain,
        "shopfront-sign": scene_shopfront,
        "mall-led": scene_mall,
        "tote-and-box": scene_tote_box,
        "logo-exploded-3d": scene_logo_exploded,
        "print-still-life": scene_print_still_life,
    }

    # =========================================================================
    # Rendering
    # =========================================================================
    def set_view(scene, film=True, exposure=0.0, look="AgX - Medium High Contrast"):
        vs = scene.view_settings
        if film:
            for vt in ("AgX", "Filmic"):
                try:
                    vs.view_transform = vt
                    break
                except TypeError:
                    continue
            for lk in (look, "AgX - Medium High Contrast", "Medium High Contrast", "None"):
                try:
                    vs.look = lk
                    break
                except TypeError:
                    continue
            vs.exposure = exposure
        else:
            vs.view_transform = "Standard"
            vs.look = "None"
            vs.exposure = 0.0
        vs.gamma = 1.0

    def setup_cycles(scene, samples):
        scene.render.engine = "CYCLES"
        prefs = bpy.context.preferences.addons["cycles"].preferences
        try:
            prefs.compute_device_type = "METAL"
            prefs.get_devices()
            for d in prefs.devices:
                d.use = d.type == "METAL"
            scene.cycles.device = "GPU"
        except Exception:
            scene.cycles.device = "CPU"
        c = scene.cycles
        c.samples = samples
        c.use_adaptive_sampling = True
        c.adaptive_threshold = 0.01
        c.use_denoising = True
        for attr, val in (("denoiser", "OPENIMAGEDENOISE"), ("denoising_use_gpu", True),
                          ("denoising_input_passes", "RGB_ALBEDO_NORMAL"), ("denoising_prefilter", "ACCURATE")):
            try:
                setattr(c, attr, val)
            except Exception:
                pass
        c.max_bounces = 12
        c.diffuse_bounces = 4
        c.glossy_bounces = 6
        c.transmission_bounces = 12
        c.volume_bounces = 1
        c.transparent_max_bounces = 32
        c.caustics_reflective = False
        c.caustics_refractive = False
        c.sample_clamp_indirect = 6.0
        c.blur_glossy = 0.5
        try:
            c.use_light_tree = True
        except Exception:
            pass
        scene.render.film_transparent = False
        mb = C.get("motion_blur")
        scene.render.use_motion_blur = bool(mb)
        if mb:
            scene.render.motion_blur_shutter = mb
            scene.frame_set(1)

    def setup_workbench(scene, color_type="OBJECT", light_="STUDIO", cavity=True, shadows=True, outline=True):
        scene.render.engine = "BLENDER_WORKBENCH"
        sh = scene.display.shading
        sh.light = light_
        sh.color_type = color_type
        sh.show_cavity = cavity
        if cavity:
            try:
                sh.cavity_type = "BOTH"
                sh.cavity_ridge_factor = 1.0
                sh.cavity_valley_factor = 1.0
            except Exception:
                pass
        sh.show_shadows = shadows
        if shadows:
            scene.display.shadow_focus = 0.3
            scene.display.light_direction = (0.45, -0.35, 0.82)
        sh.show_object_outline = outline
        sh.show_specular_highlight = light_ == "STUDIO"
        try:
            scene.display.render_aa = "16"
        except Exception:
            pass
        scene.render.film_transparent = False

    def setup_eevee(scene, samples=48):
        for eng in ("BLENDER_EEVEE", "BLENDER_EEVEE_NEXT"):
            try:
                scene.render.engine = eng
                break
            except TypeError:
                continue
        e = scene.eevee
        e.taa_render_samples = samples
        for attr, val in (("use_raytracing", True), ("use_shadows", True), ("use_gtao", True)):
            try:
                setattr(e, attr, val)
            except Exception:
                pass
        scene.render.film_transparent = False

    def setup_compositor(scene, glare=None):
        """Bloom on the scene-linear image, before the view transform."""
        ng = bpy.data.node_groups.get("SV_finish")
        if ng is None:
            ng = bpy.data.node_groups.new("SV_finish", "CompositorNodeTree")
            ng.interface.new_socket("Image", in_out="OUTPUT", socket_type="NodeSocketColor")
            rl = ng.nodes.new("CompositorNodeRLayers")
            out = ng.nodes.new("NodeGroupOutput")
            gl = ng.nodes.new("CompositorNodeGlare")
            gl.name = "SV_glare"
            ng.links.new(rl.outputs["Image"], gl.inputs["Image"])
            ng.links.new(gl.outputs["Image"], out.inputs[0])
        gl = ng.nodes["SV_glare"]
        p = dict(threshold=1.5, strength=0.4, size=0.7)
        p.update(glare or {})
        for k, v in (("Type", "Bloom"), ("Quality", "High"), ("Threshold", p["threshold"]),
                     ("Strength", p["strength"]), ("Size", p["size"]), ("Smoothness", 0.4)):
            try:
                gl.inputs[k].default_value = v
            except Exception as e:
                print("SV_WARN glare", k, e)
        try:
            scene.compositing_node_group = ng
        except Exception:
            pass
        scene.render.use_compositing = True

    def no_compositor(scene):
        scene.render.use_compositing = False

    def render_to(path):
        sc = bpy.context.scene
        sc.render.filepath = path
        sc.render.image_settings.file_format = "PNG"
        sc.render.image_settings.color_mode = "RGB"
        sc.render.image_settings.color_depth = "8"
        t = time.time()
        bpy.ops.render.render(write_still=True)
        return time.time() - t

    def read_png(path):
        """Image as a top-down float array (H, W, 4), values as stored (sRGB)."""
        img = bpy.data.images.load(path, check_existing=False)
        w, h = img.size
        a = np.empty(w * h * 4, np.float32)
        img.pixels.foreach_get(a)
        bpy.data.images.remove(img)
        return a.reshape(h, w, 4)[::-1].copy()

    def write_png(path, arr):
        arr = np.clip(arr, 0, 1)
        h, w = arr.shape[:2]
        if arr.shape[2] == 3:
            arr = np.concatenate([arr, np.ones((h, w, 1), np.float32)], axis=2)
        img = bpy.data.images.new("tmp_out", w, h, alpha=True)
        img.pixels.foreach_set(arr[::-1].astype(np.float32).ravel())
        img.filepath_raw = path
        img.file_format = "PNG"
        img.save()
        bpy.data.images.remove(img)

    # ---------------------------------------------------------------- camera finish
    def lens_grid(h, w, k, scale=1.0):
        yy, xx = np.mgrid[0:h, 0:w].astype(np.float32)
        cx, cy = w / 2.0, h / 2.0
        R = math.hypot(cx, cy)
        dx = (xx + 0.5 - cx) / R
        dy = (yy + 0.5 - cy) / R
        r2 = dx * dx + dy * dy
        f = (1.0 + k * r2) / (1.0 + k) * scale
        return cx + dx * R * f - 0.5, cy + dy * R * f - 0.5, np.sqrt(r2)

    def sample(ch, sx, sy):
        h, w = ch.shape
        x0 = np.floor(sx).astype(np.int32)
        y0 = np.floor(sy).astype(np.int32)
        fx = sx - x0
        fy = sy - y0
        x0c = np.clip(x0, 0, w - 1)
        x1c = np.clip(x0 + 1, 0, w - 1)
        y0c = np.clip(y0, 0, h - 1)
        y1c = np.clip(y0 + 1, 0, h - 1)
        a = ch[y0c, x0c] * (1 - fx) + ch[y0c, x1c] * fx
        b = ch[y1c, x0c] * (1 - fx) + ch[y1c, x1c] * fx
        return a * (1 - fy) + b * fy

    def lens_finish(arr, fin, mode="color", seed=7):
        """Barrel distortion (k), lateral chromatic aberration (ca), vignette, film grain.
        mode: color (everything), light (geometry and vignette), mask (geometry only)."""
        k = fin.get("k", 0.0)
        ca = fin.get("ca", 0.0) if mode == "color" else 0.0
        h, w = arr.shape[:2]
        out = np.empty_like(arr)
        sx, sy, r = lens_grid(h, w, k)
        for c in range(arr.shape[2]):
            if ca and c in (0, 2):
                s = 1.0 + (ca if c == 0 else -ca)
                sx2, sy2, _ = lens_grid(h, w, k, s)
                out[..., c] = sample(arr[..., c], sx2, sy2)
            else:
                out[..., c] = sample(arr[..., c], sx, sy)
        if mode in ("color", "light") and fin.get("vignette"):
            v = 1.0 - fin["vignette"] * np.clip(r, 0, 1.2) ** 2.4
            out[..., :3] *= v[..., None]
        if mode == "color" and fin.get("split"):
            # split toning: cool shadows, warm highlights (a film grade)
            sp = fin["split"]
            lum = np.clip(out[..., :3].mean(axis=2), 0, 1)[..., None]
            sh = (1 - lum) ** 2
            hi = lum ** 2
            out[..., :3] *= 1 + sp * (sh * np.array([-0.10, 0.0, 0.12], np.float32) + hi * np.array([0.06, 0.015, -0.07], np.float32))
        if mode == "color" and fin.get("grain"):
            rng = np.random.default_rng(seed)
            n = rng.standard_normal((h, w)).astype(np.float32)
            n = (n + np.roll(n, 1, 0) * 0.5 + np.roll(n, 1, 1) * 0.5) / 1.5
            lum = out[..., :3].mean(axis=2)
            amp = fin["grain"] * (0.35 + 1.6 * np.sqrt(np.clip(lum * (1 - lum), 0, 0.25)))
            out[..., :3] += (n * amp)[..., None]
            chroma = rng.standard_normal((h, w, 3)).astype(np.float32) * fin["grain"] * 0.25
            out[..., :3] += chroma
        return np.clip(out, 0, 1)

    def lens_point(px, py, fin, w=None, h=None):
        """Where an undistorted pixel lands after lens_finish (inverse of the sampling map)."""
        w = w or W
        h = h or H
        k = fin.get("k", 0.0)
        if not k:
            return px, py
        cx, cy = w / 2.0, h / 2.0
        R = math.hypot(cx, cy)
        qx, qy = (px - cx) / R, (py - cy) / R
        ux, uy = qx, qy
        for _ in range(30):
            f = (1.0 + k * (ux * ux + uy * uy)) / (1.0 + k)
            ux, uy = qx / f, qy / f
        return cx + ux * R, cy + uy * R

    # ---------------------------------------------------------------- helpers
    def hide_lights(hide):
        for o in bpy.data.objects:
            if o.type == "LIGHT":
                o.hide_render = hide

    def hide_atmosphere(hide):
        c = bpy.data.collections.get("SV_atmosphere")
        if c:
            for o in c.objects:
                o.hide_render = hide

    def set_proxy(visible):
        for o in coll("SV_helpers").objects:
            o.hide_render = not visible

    def build_proxy(cam, dist, scale):
        """An orange camera body plus its view frustum, for the overview shots."""
        sc = bpy.context.scene
        hc = coll("SV_helpers")
        mat = mat_emit("proxy_orange", PROXY_ORANGE, 8.0)
        frame = cam.data.view_frame(scene=sc)
        mw = cam.matrix_world
        apex = mw.translation.copy()
        pts = [mw @ (v * (dist / -v.z)) for v in frame]
        r = max(0.003, scale * 0.004)
        objs = []
        for i, p in enumerate(pts):
            objs.append(beam("proxy_ray%d" % i, apex, p, r, mat, 6, hc))
            objs.append(beam("proxy_edge%d" % i, p, pts[(i + 1) % 4], r * 1.4, mat, 6, hc))
        body = boxc("proxy_body", (0, 0, 0), (scale * 0.09, scale * 0.06, scale * 0.12), mat, collection=hc)
        body.matrix_world = mw @ Matrix.Translation((0, 0, scale * 0.06))
        objs.append(body)
        for o in objs:
            o.color = rgba(PROXY_ORANGE)
            o.visible_shadow = False
            o["sv_mask_ignore"] = True
        return objs

    def corners_for(face, cam):
        sc = bpy.context.scene
        ob = face["obj"]
        mw = ob.matrix_world
        out_px, out_w = [], []
        for c in ob["sv_corners_local"]:
            wco = mw @ Vector(c)
            ndc = world_to_camera_view(sc, cam, wco)
            out_px.append([ndc.x * W, (1.0 - ndc.y) * H])
            out_w.append([round(wco.x, 4), round(wco.y, 4), round(wco.z, 4)])
        return out_px, out_w

    def ratio_label(a):
        for d in range(1, 11):
            n_ = round(a * d)
            if n_ and n_ <= 20 and abs(n_ / d - a) / a < 0.002:
                return "%d:%d" % (n_, d)
        return ("1:%.2f" % (1 / a)) if a < 1 else ("%.2f:1" % a)

    def neutral_world():
        neutral = bpy.data.worlds.get("SV_neutral") or bpy.data.worlds.new("SV_neutral")
        try:
            neutral.use_nodes = True
        except Exception:
            pass
        bg = next(n for n in neutral.node_tree.nodes if n.type == "BACKGROUND")
        bg.inputs[0].default_value = (0.75, 0.76, 0.78, 1)
        bg.inputs[1].default_value = 1.0
        return neutral

    def keep_in_clay(m):
        """Glass, volumes and pure emitters (lamps, tubes, trails) keep their own material in the clay light study."""
        if m is None or not m.node_tree:
            return False
        types = {n.type for n in m.node_tree.nodes}
        if "BSDF_PRINCIPLED" not in types and ("EMISSION" in types or "BSDF_TRANSPARENT" in types):
            return True
        out = next((n for n in m.node_tree.nodes if n.type == "OUTPUT_MATERIAL"), None)
        return bool(out and out.inputs["Volume"].is_linked and not out.inputs["Surface"].is_linked)

    def clay_on():
        clay, cg, cout = new_mat("SV_clay")
        finish(clay, cg, cout, principled(cg, (0.62, 0.62, 0.6), 0.55, 0.0, 0.4), (0.62, 0.62, 0.6))
        saved = {}
        for d in list(bpy.data.meshes) + list(bpy.data.curves):
            mats = list(d.materials)
            if not mats or all(keep_in_clay(m) for m in mats):
                continue
            saved[d] = mats
            for i, m in enumerate(mats):
                if not keep_in_clay(m):
                    d.materials[i] = clay
        return saved

    def clay_off(saved):
        for d, mats in saved.items():
            for i, m in enumerate(mats):
                d.materials[i] = m

    def px_bbox(ob, cam):
        """An object's screen box in pixels (from its evaluated bounding box corners)."""
        sc = bpy.context.scene
        pts = []
        for c in ob.bound_box:
            ndc = world_to_camera_view(sc, cam, ob.matrix_world @ Vector(c))
            if ndc.z > 0:
                pts.append((ndc.x * W, (1 - ndc.y) * H))
        if not pts:
            return None
        xs = [p[0] for p in pts]
        ys = [p[1] for p in pts]
        return [round(min(xs)), round(min(ys)), round(max(xs)), round(max(ys))]

    def probe_report(cfg, fin):
        cam = cfg["camera"]
        bpy.context.view_layer.update()
        out = {"faces": {}, "probe": {}}
        for f in FACES:
            px, _ = corners_for(f, cam)
            xs = [p[0] for p in px]
            ys = [p[1] for p in px]
            out["faces"][f["name"]] = {"bbox": [round(min(xs)), round(min(ys)), round(max(xs)), round(max(ys))],
                                       "w": round(max(xs) - min(xs)), "h": round(max(ys) - min(ys))}
        groups = {}
        for o in bpy.data.objects:
            k = o.get("sv_probe")
            if k and o.type in ("MESH", "CURVE", "FONT"):
                b = px_bbox(o, cam)
                if b:
                    g = groups.setdefault(k, list(b))
                    g[0], g[1], g[2], g[3] = min(g[0], b[0]), min(g[1], b[1]), max(g[2], b[2]), max(g[3], b[3])
        out["probe"] = groups
        if os.environ.get("SV_LIGHT"):
            lo = bpy.data.objects.get(os.environ["SV_LIGHT"])
            out["light"] = None if lo is None else {"loc": list(lo.matrix_world.translation), "dir": list(lo.matrix_world.to_3x3() @ Vector((0, 0, -1))),
                                                    "energy": lo.data.energy, "hide": lo.hide_render, "colls": [c.name for c in lo.users_collection]}
        if os.environ.get("SV_RAY"):
            a_, b_ = os.environ["SV_RAY"].split(">")
            o_ = Vector([float(v) for v in a_.split(",")])
            t_ = Vector([float(v) for v in b_.split(",")])
            dg = bpy.context.evaluated_depsgraph_get()
            hits = []
            org = o_
            for _ in range(6):
                ok, loc_, nrm_, idx_, ob_, mx_ = bpy.context.scene.ray_cast(dg, org, (t_ - o_).normalized(), distance=(t_ - org).length)
                if not ok:
                    break
                hits.append([ob_.name, [round(v, 3) for v in loc_]])
                org = loc_ + (t_ - o_).normalized() * 1e-4
            out["ray"] = hits
        if os.environ.get("SV_PICK"):
            # what sits under a pixel: rays from the camera through each "x,y" (pixels, before the lens finish)
            sc_ = bpy.context.scene
            dg = bpy.context.evaluated_depsgraph_get()
            fr = [cam.matrix_world @ v for v in cam.data.view_frame(scene=sc_)]   # TR, BR, BL, TL in world
            org = cam.matrix_world.translation
            picks = []
            for pt in os.environ["SV_PICK"].split(";"):
                px_, py_ = [float(v) for v in pt.split(",")]
                u, v = px_ / W, py_ / H
                top = fr[3].lerp(fr[0], u)
                bot = fr[2].lerp(fr[1], u)
                p_ = top.lerp(bot, v)
                d_ = (p_ - org).normalized()
                ok, loc_, nrm_, idx_, ob_, mx_ = sc_.ray_cast(dg, org, d_)
                chain = []
                o2 = ob_
                while o2 is not None:
                    chain.append(o2.name)
                    o2 = o2.parent
                refl = None
                if ok:
                    # what that surface mirrors: one bounce along the reflected ray
                    r_ = d_ - 2 * d_.dot(nrm_) * nrm_
                    ok2, loc2, _, _, ob2, _ = sc_.ray_cast(dg, loc_ + nrm_ * 1e-4, r_)
                    refl = [ob2.name if ok2 else "world", [round(c_, 2) for c_ in loc2] if ok2 else None]
                picks.append([pt, chain, [round(c_, 2) for c_ in loc_] if ok else None, refl])
            out["pick"] = picks
        if C.get("guides"):
            sc_ = bpy.context.scene
            gl = []
            for gd in C["guides"]:
                a_ = world_to_camera_view(sc_, cam, gd["start"])
                b_ = world_to_camera_view(sc_, cam, gd["end"])
                gl.append([gd["part"], round(a_.x * W), round((1 - a_.y) * H), round(b_.x * W), round((1 - b_.y) * H)])
            out["guides"] = gl
        out["camera"] = {"loc": [round(v, 3) for v in cam.matrix_world.translation],
                         "rot_deg": [round(math.degrees(a), 2) for a in cam.matrix_world.to_euler()], "lens": cam.data.lens}
        print("SV_PROBE " + json.dumps(out))

    def run_scene(name, args):
        t0 = time.time()
        if getattr(args, "blend", None):
            bpy.ops.wm.open_mainfile(filepath=os.path.abspath(args.blend), load_ui=False)
        else:
            bpy.ops.wm.read_factory_settings(use_empty=True)
        MATS.clear()
        FACES.clear()
        C.clear()
        FONTS.clear()
        C["args"] = args
        sc = bpy.context.scene
        sc.name = name
        C["coll"] = coll("SV_scene")
        coll("SV_design_faces")
        coll("SV_lights")
        coll("SV_cameras")
        hc = coll("SV_helpers")
        cfg = PRESETS[name]()
        bpy.context.view_layer.update()
        cam = cfg["camera"]
        sc.camera = cam
        scale_ = 0.5 if args.quick else 1.0
        sc.render.resolution_x = W
        sc.render.resolution_y = H
        sc.render.resolution_percentage = int(100 * scale_)
        exposure = cfg.get("exposure", 0.0)
        look = cfg.get("look", "AgX - Medium High Contrast")
        samples = args.samples or cfg.get("samples", 256)
        if args.quick:
            samples = min(samples, 64)
        fin = dict(cfg.get("finish", {}))
        if getattr(args, "probe", False):
            probe_report(cfg, fin)
            return
        out = os.path.join(args.out, name)
        stages = os.path.join(out, "stages")
        os.makedirs(stages, exist_ok=True)
        times = {}

        set_face_mode("plate")
        if args.preview:
            for f in FACES:
                if f["obj"].get("sv_beauty_hide") and not args.design:
                    f["obj"].hide_render = True
            setup_cycles(sc, samples)
            set_view(sc, True, exposure, look)
            setup_compositor(sc, cfg.get("glare"))
            p = os.path.join(out, "preview_raw.png")
            times["preview"] = render_to(p)
            a = read_png(p)
            write_png(os.path.join(out, "preview.png"), lens_finish(a[..., :3], fin, "color"))
            if args.design:
                set_face_mode("design", os.path.abspath(args.design))
                render_to(p)
                a = read_png(p)
                write_png(os.path.join(out, "preview-with-design.png"), lens_finish(a[..., :3], fin, "color"))
            if cfg.get("extra") and name != "logo-exploded-3d":
                cfg["extra"](args, out, cfg, samples)
            print("SV_PREVIEW %s %.1fs" % (name, time.time() - t0))
            return

        dist = (Vector(FACES[0]["obj"].matrix_world.translation) - cam.matrix_world.translation).length
        build_proxy(cam, dist, scale=cfg.get("proxy_scale", max(1.0, dist / 4.0)))
        set_proxy(False)
        hc.hide_render = False

        tb = bpy.data.texts.new("SV_README")
        tb.write("SV Blender scene: %s\nMade by make-scenes-blender.py (SV Edits), headless.\n"
                 "Design faces live in the SV_design_faces collection; their corners are in corners.json.\n"
                 "SV_camera is the shot. SV_overview and the orange SV_helpers show the set from outside.\n" % name)

        def hide_helpers_only(hide):
            # gobos and other camera-invisible helpers would block the clay and viewport views
            for o in bpy.data.objects:
                if o.type == "MESH" and not o.visible_camera:
                    o.hide_render = hide

        def cutaway(hide):
            # interiors: lift the roof and the near walls off for the overview shots
            for o in bpy.data.objects:
                if o.get("sv_cutaway"):
                    o.hide_render = hide

        if not args.no_captures:
            over = cfg["overview"]
            hide_helpers_only(True)
            cutaway(True)
            # 01 blockout: Workbench clay from the overview camera, camera proxy on (the logo: from the shot camera,
            # tight on its nine imported curves, with their part names)
            tight = cfg.get("blockout_cam") == "shot"
            set_proxy(not tight)
            hide_atmosphere(True)
            sc.camera = cam if tight else over
            restore_import = None
            if tight and cfg.get("import_view"):
                cam_i, restore_import = cfg["import_view"]()
                sc.camera = cam_i
            if tight:
                cam_l = sc.camera
                lab = {}
                for o in bpy.data.objects:
                    k_ = o.get("sv_probe")
                    if k_ and o.type in ("MESH", "CURVE", "FONT") and not o.name.startswith("GUIDE_"):
                        b_ = px_bbox(o, cam_l)
                        if b_:
                            g_ = lab.setdefault(k_, list(b_))
                            g_[0], g_[1], g_[2], g_[3] = min(g_[0], b_[0]), min(g_[1], b_[1]), max(g_[2], b_[2]), max(g_[3], b_[3])
                with open(os.path.join(stages, "01-labels.json"), "w") as fh:
                    json.dump(lab, fh)
            no_compositor(sc)
            setup_workbench(sc, "OBJECT", "STUDIO", True, True, True)
            sc.display.shading.single_color = (0.72, 0.72, 0.70)
            set_view(sc, film=False)
            real_world = sc.world
            sc.world = neutral_world()
            for f in FACES:
                f["obj"].color = rgba(ACCENT)
            times["01"] = render_to(os.path.join(stages, "01-blockout.png"))
            if restore_import:
                restore_import()
            # 02 materials: EEVEE from the shot camera in a neutral studio light, lamps off (like Material Preview)
            cutaway(False)
            set_proxy(False)
            hide_atmosphere(True)
            sc.camera = cam
            hide_lights(True)
            setup_eevee(sc, 32)
            set_view(sc, film=True, exposure=0.0, look="AgX - Base Contrast")
            times["02"] = render_to(os.path.join(stages, "02-materials.png"))
            sc.world = real_world
            hide_lights(False)
            # 03 light: Cycles from the shot camera with every surface in one clay, so only the light reads:
            # the sky, the lamps, the haze and the shadows (emissive signs go out: they are materials)
            hide_atmosphere(False)
            hide_helpers_only(False)
            saved_mats = clay_on()
            setup_cycles(sc, 128 if not args.quick else 24)
            set_view(sc, True, exposure + cfg.get("clay_exposure", 0.0), look)
            setup_compositor(sc, cfg.get("glare"))
            times["03"] = render_to(os.path.join(stages, "03-light.png"))
            clay_off(saved_mats)
            # 04 camera: the real lens view is drawn by the annotate step on the final plate (thirds, frame guide)
            hide_helpers_only(False)

        # 05 / plate: final Cycles render, faces grey, bloom, then the camera finish
        set_proxy(False)
        sc.camera = cam
        setup_cycles(sc, samples)
        set_view(sc, True, exposure, look)
        setup_compositor(sc, cfg.get("glare"))
        set_face_mode("plate")
        raw_plate = os.path.join(stages, "_plate_raw.png")
        times["plate"] = render_to(raw_plate)
        plate_view = (sc.view_settings.view_transform, sc.view_settings.look)

        # light pass: faces pure white, same light, no bloom
        set_face_mode("light")
        no_compositor(sc)
        sc.cycles.samples = max(64, samples // 2)
        raw_light = os.path.join(stages, "_light_raw.png")
        ov_hidden = []
        if cfg.get("overlay"):
            # the overlay (the logo's guides) is not light on the face: keep it out of the Multiply pass
            for o in bpy.data.objects:
                if o.name.startswith(cfg["overlay"][1]) and not o.hide_render:
                    o.hide_render = True
                    ov_hidden.append(o)
        times["light"] = render_to(raw_light)
        for o in ov_hidden:
            o.hide_render = False

        # overlay pass (logo guides): only the named objects, everything else held out, on transparent film, so
        # PhotoCraft can put them on their own layer above the design and the swap can hide them
        raw_overlay = None
        if cfg.get("overlay"):
            ov_name, ov_prefix = cfg["overlay"]
            held = []
            for o in bpy.data.objects:
                if o.type in ("MESH", "CURVE", "FONT") and not o.name.startswith(ov_prefix) and not o.is_holdout:
                    o.is_holdout = True
                    held.append(o)
            sc.render.film_transparent = True
            raw_overlay = os.path.join(stages, "_overlay_raw.png")
            sc.render.image_settings.color_mode = "RGBA"
            sc.render.filepath = raw_overlay
            bpy.ops.render.render(write_still=True)
            sc.render.image_settings.color_mode = "RGB"
            sc.render.film_transparent = False
            for o in held:
                o.is_holdout = False

        # masks: Workbench flat, faces white, everything else black (occluders stay black)
        setup_workbench(sc, "OBJECT", "FLAT", False, False, False)
        set_view(sc, film=False)
        mask_world = bpy.data.worlds.new("SV_black")
        mask_world.color = (0, 0, 0)
        real_world = sc.world
        sc.world = mask_world
        hidden = []
        for o in bpy.data.objects:
            if o.get("sv_mask_ignore") or o.type == "LIGHT":
                if not o.hide_render:
                    o.hide_render = True
                    hidden.append(o)
        saved_colors = {o.name: tuple(o.color) for o in bpy.data.objects}
        for o in bpy.data.objects:
            o.color = (0, 0, 0, 1)
        for f in FACES:
            f["obj"].color = (1, 1, 1, 1)
        raw_mask = os.path.join(stages, "_mask_raw.png")
        times["mask"] = render_to(raw_mask)
        mask_paths = {}
        per_face_raw = {}
        if len(FACES) > 1:
            for f in FACES:
                for g_ in FACES:
                    g_["obj"].color = (1, 1, 1, 1) if g_ is f else (0, 0, 0, 1)
                p = os.path.join(stages, "_mask_%s_raw.png" % f["name"])
                render_to(p)
                per_face_raw[f["name"]] = p
        for o in bpy.data.objects:
            o.color = saved_colors.get(o.name, (0.78, 0.78, 0.78, 1))
        for o in hidden:
            o.hide_render = False
        sc.world = real_world

        # the camera finish, identical geometry on all passes
        tf = time.time()
        plate = lens_finish(read_png(raw_plate)[..., :3], fin, "color")
        write_png(os.path.join(out, "plate.png"), plate)
        mk = lens_finish(read_png(raw_mask)[..., :3], fin, "mask")
        mk = np.repeat(mk[..., :1], 3, axis=2)
        write_png(os.path.join(out, "mask.png"), mk)
        lt = lens_finish(read_png(raw_light)[..., :3], fin, "light")
        alpha = mk[..., :1]
        write_png(os.path.join(out, "light.png"), np.concatenate([lt * (alpha > 0.002), alpha], axis=2))
        if raw_overlay:
            ov = read_png(raw_overlay)
            rgb_ = lens_finish(ov[..., :3], fin, "light")
            a_ = lens_finish(ov[..., 3:4], fin, "mask")
            write_png(os.path.join(out, "overlay-%s.png" % cfg["overlay"][0]), np.concatenate([rgb_, a_], axis=2))
            os.remove(raw_overlay)
        for fname, p in per_face_raw.items():
            m1 = lens_finish(read_png(p)[..., :3], fin, "mask")
            write_png(os.path.join(out, "mask-%s.png" % fname), np.repeat(m1[..., :1], 3, axis=2))
            mask_paths[fname] = "mask-%s.png" % fname
            os.remove(p)
        if not per_face_raw:
            mask_paths[FACES[0]["name"]] = "mask.png"
        times["finish"] = time.time() - tf
        for p in (raw_plate, raw_light, raw_mask):
            if os.path.exists(p):
                os.remove(p)

        # optional ground truth with a design mapped on every face
        if args.design:
            setup_cycles(sc, samples)
            set_view(sc, True, exposure, look)
            setup_compositor(sc, cfg.get("glare"))
            set_face_mode("design", os.path.abspath(args.design))
            p = os.path.join(stages, "_design_raw.png")
            times["design"] = render_to(p)
            write_png(os.path.join(out, "preview-with-design.png"), lens_finish(read_png(p)[..., :3], fin, "color"))
            os.remove(p)

        # scene-specific extras (beauty renders, frame sequences)
        if cfg.get("extra"):
            for k, v in (cfg["extra"](args, out, cfg, samples) or {}).items():
                times["extra_" + k] = v

        # corners.json
        set_face_mode("plate")
        faces_out = []
        for f in FACES:
            px_u, wco = corners_for(f, cam)
            px = [list(lens_point(x, y, fin)) for x, y in px_u]
            px = [[round(x, 2), round(y, 2)] for x, y in px]
            wdt, hgt = f["size"]
            xs = [p[0] for p in px]
            ys = [p[1] for p in px]
            faces_out.append({
                "name": f["name"],
                "label": f["label"],
                "kind": f["kind"],
                "surface": f["surface"],
                "corners_px": px,
                "corners": {"top_left": px[0], "top_right": px[1], "bottom_right": px[2], "bottom_left": px[3]},
                "corners_px_before_lens": [[round(x, 2), round(y, 2)] for x, y in px_u],
                "corners_world_m": wco,
                "size_m": [round(wdt, 4), round(hgt, 4)],
                "aspect": round(wdt / hgt, 4),
                "aspect_label": ratio_label(wdt / hgt),
                "bbox_px": [round(min(xs), 1), round(min(ys), 1), round(max(xs), 1), round(max(ys), 1)],
                "in_frame": all(0 <= p[0] <= W and 0 <= p[1] <= H for p in px),
                "mask": mask_paths[f["name"]],
                "suggested_design_px": [int(round(2400 * min(1, wdt / hgt))), int(round(2400 * min(1, hgt / wdt)))],
            })
        cd = cam.data
        info = {
            "scene": name,
            "title": SCENE_TITLES.get(name, (name, ""))[0],
            "title_th": SCENE_TITLES.get(name, (name, ""))[1],
            "image": {"width": W, "height": H},
            "corner_order": ["top-left", "top-right", "bottom-right", "bottom-left"],
            "coordinates": "pixels, origin at the top-left of plate.png, x right, y down",
            "method": "bpy_extras.object_utils.world_to_camera_view on the face's own vertices, "
                      "then the same lens distortion as plate.png",
            "camera": {"lens_mm": cd.lens, "sensor_mm": cd.sensor_width, "shift": [round(cd.shift_x, 4), round(cd.shift_y, 4)],
                       "location_m": [round(v, 3) for v in cam.matrix_world.translation],
                       "eye_height_m": round(cam.matrix_world.translation.z, 2),
                       "pitch_deg": round(math.degrees(cam.matrix_world.to_euler().x) - 90.0, 1),
                       "fstop": cd.dof.aperture_fstop if cd.dof.use_dof else None},
            "render": {"engine": "Cycles", "device": sc.cycles.device, "samples": samples, "denoise": "OpenImageDenoise",
                       "view_transform": plate_view[0], "look": plate_view[1],
                       "exposure": exposure, "sky_model": C.get("sky_model"), "bloom": cfg.get("glare", {}),
                       "lens_finish": fin},
            "files": {"blend": name + ".blend", "plate": "plate.png", "light": "light.png", "mask": "mask.png"},
            "how_to_composite": ["plate.png as the base layer",
                                 "the design as a smart object, distorted to corners_px (four-corner perspective)",
                                 "masked by the face mask",
                                 "light.png above it in Multiply, clipped to the design"],
            "faces": faces_out,
            "render_seconds": {k: round(v, 1) for k, v in times.items()},
            "total_seconds": round(time.time() - t0, 1),
        }
        with open(os.path.join(out, "corners.json"), "w") as fh:
            json.dump(info, fh, indent=2, ensure_ascii=False)

        # leave the .blend in a clean, ready-to-render state
        sc.camera = cam
        setup_cycles(sc, samples)
        set_view(sc, True, exposure, look)
        setup_compositor(sc, cfg.get("glare"))
        set_proxy(False)
        hide_atmosphere(False)
        sc.render.resolution_percentage = 100
        bpy.ops.wm.save_as_mainfile(filepath=os.path.join(out, name + ".blend"), compress=True)
        total = time.time() - t0
        print("SV_DONE %s %.1fs %s" % (name, total, json.dumps(info["render_seconds"])))
        return total

    def blender_main():
        import argparse
        argv = sys.argv[sys.argv.index("--") + 1:] if "--" in sys.argv else []
        ap = argparse.ArgumentParser(prog="make-scenes-blender.py")
        ap.add_argument("--scene", default="all")
        ap.add_argument("--out", default=DEFAULT_OUT)
        ap.add_argument("--captures", default=None)
        ap.add_argument("--samples", type=int, default=None)
        ap.add_argument("--quick", action="store_true")
        ap.add_argument("--preview", action="store_true")
        ap.add_argument("--no-captures", action="store_true")
        ap.add_argument("--design", default=None)
        ap.add_argument("--parts", default=os.path.join(LAUNCH, "sv-logo-parts"))
        ap.add_argument("--card", default=os.path.join(LAUNCH, "sv-business-card.png"))
        ap.add_argument("--banner", default=os.path.join(LAUNCH, "sv-rollup-banner.png"))
        ap.add_argument("--frames", default=None, help="logo-exploded-3d: folder for the explode frame sequence")
        ap.add_argument("--nframes", type=int, default=0)
        ap.add_argument("--list", action="store_true")
        ap.add_argument("--probe", action="store_true", help="build the scene, print face and probe boxes in pixels, no render")
        ap.add_argument("--blend", default=None, help="your own .blend (only read; a copy is saved in --out)")
        ap.add_argument("--face", default=None, help="with --blend: the design face object name(s), comma separated")
        ap.add_argument("--camera", default=None, help="with --blend: the camera object (default: the scene camera)")
        ap.add_argument("--kind", default="printed", choices=["printed", "emissive"], help="with --blend: the face kind")
        ap.add_argument("--objects", action="store_true", help="with --blend: list the meshes the camera sees")
        args = ap.parse_args(argv)
        if args.blend:
            if not os.path.isfile(args.blend):
                print("SV_ERROR no such .blend: %s" % args.blend)
                return
            if args.objects:
                list_own_objects(args)
                return
            stem = "".join(ch if ch.isalnum() else "-"
                           for ch in os.path.splitext(os.path.basename(args.blend))[0].lower()).strip("-") or "own"
            if os.path.abspath(os.path.join(args.out, stem, stem + ".blend")) == os.path.abspath(args.blend):
                print("SV_ERROR --out would write over the .blend you gave: pick another --out folder")
                return
            PRESETS[stem] = scene_own_blend
            SCENE_TITLES.setdefault(stem, (os.path.basename(args.blend), ""))
            args.no_captures = True
            run_scene(stem, args)
            return
        if args.list:
            for k in PRESETS:
                print("SV_PRESET", k, "|", SCENE_TITLES.get(k, ("", ""))[0])
            return
        names = list(PRESETS) if args.scene == "all" else args.scene.split(",")
        for nm in names:
            if nm not in PRESETS:
                print("SV_ERROR unknown scene", nm)
                continue
            run_scene(nm, args)
            if args.captures and not args.no_captures and not args.preview:
                cap = os.path.join(args.captures, nm)
                r = subprocess.run(["python3", os.path.abspath(__file__), "annotate",
                                    "--scene-dir", os.path.join(args.out, nm), "--captures", cap],
                                   capture_output=True, text=True)
                print(r.stdout[-2000:], r.stderr[-2000:])


# =============================================================================
# Annotation side (plain python3 + Pillow): the assembly captures
# =============================================================================
# Every caption is written for its own scene, from what the script above actually builds. {lens}, {eye} and {fstop}
# come from corners.json, so the numbers match the render.
SCENE_STEPS = {
    "skytrain-billboard": [
        ("Blockout", "A cross road meets the main road under the viaduct, all at real size", "ถนนซอยออกถนนใหญ่ใต้รางรถไฟฟ้า ทุกชิ้นขนาดจริง"),
        ("Materials", "Damp asphalt with soft-edged puddles, stained concrete, chrome, steel", "ถนนชื้นกับแอ่งน้ำขอบนุ่ม คอนกรีตมีคราบ โครเมียม เหล็ก"),
        ("Light", "Blue-hour sky, six floodlights, open shopfronts, haze", "ฟ้าหัวค่ำ ไฟส่องป้ายหกดวง แสงจากหน้าร้าน หมอกบางๆ"),
        ("Camera", "{lens} mm at {eye} m, low over the wet road, tilted up {pitch} degrees", "เลนส์ {lens} มม. สูง {eye} ม. ตั้งต่ำเหนือถนนเปียก เงยกล้อง {pitch} องศา"),
        ("Render", "Cycles, the board left grey under six floodlights", "เรนเดอร์ด้วย Cycles เว้นป้ายเป็นสีเทา ใต้ไฟส่องหกดวง"),
        ("Passes", "Plate, light and mask of the 14 x 5.25 m board", "ภาพฉาก แสง และมาสก์ ของป้ายขนาด 14 x 5.25 ม."),
        ("Corners", "The board's four corners in pixels, from its real vertices", "พิกัดสี่มุมของป้ายเป็นพิกเซล จากจุดจริงในฉาก"),
    ],
    "shopfront-sign": [
        ("Blockout", "A soi of shophouses, SV's unit 4.2 m wide", "ซอยตึกแถว คูหาของ SV กว้าง 4.2 ม."),
        ("Materials", "Plaster, terrazzo, oak, aluminium, glass with a film of dirt", "ปูนฉาบ หินขัด ไม้โอ๊ก อะลูมิเนียม กระจกมีคราบบางๆ"),
        ("Light", "Dusk sky, pendants over an empty bench, a warm spot on the poster", "ฟ้าหัวค่ำ ไฟห้อยเหนือโต๊ะว่าง สปอตไลต์อุ่นส่องโปสเตอร์"),
        ("Camera", "{lens} mm at {eye} m, turned 4 degrees to SV's unit", "เลนส์ {lens} มม. สูง {eye} ม. หันเข้าหาคูหาของ SV 4 องศา"),
        ("Render", "Cycles, the lightbox and the poster left grey", "เรนเดอร์ด้วย Cycles เว้นป้ายไฟกับโปสเตอร์เป็นสีเทา"),
        ("Passes", "Plate, light, and a mask for each face", "ภาพฉาก แสง และมาสก์แยกของแต่ละหน้า"),
        ("Corners", "Lightbox 3.50 x 0.74 m and the A1 poster, in pixels", "ป้ายไฟ 3.50 x 0.74 ม. กับโปสเตอร์ A1 เป็นพิกเซล"),
    ],
    "mall-led": [
        ("Blockout", "A four-level atrium, the LED 12.8 x 7.2 m in a walnut niche", "โถงห้างสี่ชั้น จอ LED 12.8 x 7.2 ม. ฝังในผนังระแนงไม้"),
        ("Materials", "Polished stone, walnut fins, glass rails, rubber handrails, books", "พื้นหินขัดเงา ระแนงวอลนัต ราวกระจก ราวยาง ชั้นหนังสือ"),
        ("Light", "Evening sky, spot downlights, the screen's own spill", "ฟ้าค่ำ ไฟดาวน์ไลต์ และแสงที่จอสาดออกมาเอง"),
        ("Camera", "{lens} mm at {eye} m, square to the screen", "เลนส์ {lens} มม. สูง {eye} ม. ตั้งตรงกับจอ"),
        ("Render", "Cycles, a book-fair banner in front, the screen left grey", "เรนเดอร์ด้วย Cycles ป้ายงานหนังสือด้านหน้า เว้นจอเป็นสีเทา"),
        ("Passes", "Plate, light and mask of the screen", "ภาพฉาก แสง และมาสก์ของจอ"),
        ("Corners", "The screen's four corners in pixels", "พิกัดสี่มุมของจอเป็นพิกเซล"),
    ],
    "tote-and-box": [
        ("Blockout", "A tote on an oak peg, a kraft box on a travertine block", "ถุงผ้าแขวนหมุดไม้ กล่องกระดาษบนก้อนหินทราเวอร์ทีน"),
        ("Materials", "Cotton canvas, a woven patch, kraft paper, travertine, a receipt", "ผ้าแคนวาส แผ่นผ้าทอ กระดาษคราฟท์ หินทราเวอร์ทีน ใบเสร็จ"),
        ("Light", "Golden-hour sun at 15 degrees, through a window and leaves", "แดดเย็น 15 องศา ผ่านหน้าต่างและใบไม้"),
        ("Camera", "{lens} mm at {eye} m, f/{fstop}, the bag on the left third", "เลนส์ {lens} มม. สูง {eye} ม. f/{fstop} ถุงอยู่ช่องซ้าย"),
        ("Render", "Cycles, the patch, the label and the flap left grey", "เรนเดอร์ด้วย Cycles เว้นแผ่นผ้า ฉลาก และฝากล่องเป็นสีเทา"),
        ("Passes", "Plate, light, and a mask for each face", "ภาพฉาก แสง และมาสก์แยกของแต่ละหน้า"),
        ("Corners", "Patch, lid label and front flap, in pixels", "แผ่นผ้า ฉลากบนฝา และฝาด้านหน้า เป็นพิกเซล"),
    ],
    "logo-exploded-3d": [
        ("Import", "Nine named SVG parts become 3D curves", "นำเข้าชิ้นส่วน SVG ทั้งเก้าชิ้นเป็นเส้นโค้ง 3D"),
        ("Materials", "Smoked acrylic, anodised metal, satin lacquer, lit acrylic", "อะคริลิกรมควัน โลหะชุบ แล็กเกอร์ด้าน อะคริลิกเรืองแสง"),
        ("Light", "One hard key, a green rim at a fifth, the far wall kept out of the floor", "ไฟหลักแข็งหนึ่งดวง ไฟขอบสีเขียว ผนังไกลไม่สะท้อนบนพื้น"),
        ("Camera", "{lens} mm, 35 degrees off the axis: the parts climb clear", "เลนส์ {lens} มม. เยื้องแกน 35 องศา ชิ้นส่วนไม่บังกัน"),
        ("Render", "Cycles on the GPU, glow only on the three dots", "เรนเดอร์ด้วย Cycles มีแสงเรืองเฉพาะจุดสามจุด"),
        ("Passes", "Plate, light and the rounded face mask", "ภาพฉาก แสง และมาสก์หน้าแผ่นมุมมน"),
        ("Corners", "The 480 mm face plate, R 96 mm corners, in pixels", "หน้าแผ่น 480 มม. มุมมน R 96 มม. เป็นพิกเซล"),
    ],
    "print-still-life": [
        ("Blockout", "A proof table: a card stand, a fan of cards, the proof", "โต๊ะตรวจงานพิมพ์ ที่ตั้งนามบัตร นามบัตรเรียงพัด ปรู๊ฟ"),
        ("Materials", "Card with a white core, brushed brass, a proof print, a linen tester", "การ์ดไส้ขาว ทองเหลืองขัดลาย ปรู๊ฟ แว่นส่องงานพิมพ์"),
        ("Light", "One hard window sun at 20 degrees, cut by blind slats", "แดดแข็งจากหน้าต่าง 20 องศา ลอดมู่ลี่"),
        ("Camera", "{lens} mm at f/{fstop}, 35 degrees down", "เลนส์ {lens} มม. f/{fstop} มองลง 35 องศา"),
        ("Render", "Cycles, the card in the stand and the top card left grey", "เรนเดอร์ด้วย Cycles เว้นนามบัตรบนขาตั้งและใบบนสุดเป็นสีเทา"),
        ("Passes", "Plate, light, and a mask for each card", "ภาพฉาก แสง และมาสก์แยกของนามบัตรแต่ละใบ"),
        ("Corners", "Two 90 x 54 mm cards, corners in pixels", "นามบัตรสองใบ 90 x 54 มม. พิกัดมุมเป็นพิกเซล"),
    ],
}


def find_chrome():
    for p in ("/Applications/Google Chrome.app/Contents/MacOS/Google Chrome",
              "/Applications/Chromium.app/Contents/MacOS/Chromium",
              "/usr/bin/google-chrome", "/usr/bin/chromium", "/usr/bin/chromium-browser",
              r"C:\Program Files\Google\Chrome\Application\chrome.exe",
              r"C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe"):
        if os.path.exists(p):
            return p
    return None


def html_shot(chrome, html, png, w=1920, h=1080):
    """Render an HTML page to PNG with headless Chrome (no window opens)."""
    hp = png[:-4] + ".html"
    with open(hp, "w") as fh:
        fh.write(html)
    import pathlib
    subprocess.run([chrome, "--headless=new", "--disable-gpu", "--hide-scrollbars", "--force-device-scale-factor=1",
                    "--allow-file-access-from-files", "--window-size=%d,%d" % (w, h), "--screenshot=%s" % png,
                    pathlib.Path(hp).as_uri()], capture_output=True, text=True, timeout=120)
    os.remove(hp)


def esc(t):
    return (str(t).replace("&", "&amp;").replace("<", "&lt;").replace(">", "&gt;"))


def annotate_main(argv):
    import argparse
    import pathlib
    ap = argparse.ArgumentParser()
    ap.add_argument("--scene-dir", required=True)
    ap.add_argument("--captures", required=True)
    a = ap.parse_args(argv)
    sd = os.path.abspath(a.scene_dir)
    cap = os.path.abspath(a.captures)
    os.makedirs(cap, exist_ok=True)
    chrome = find_chrome()
    if chrome is None:
        print("SV_WARN no Chrome or Edge found: captures need a browser to set the Thai type correctly")
        return
    info = json.load(open(os.path.join(sd, "corners.json")))
    name = info["scene"]
    title_en, title_th = SCENE_TITLES.get(name, (info.get("title", name), info.get("title_th", "")))
    cam0 = info["camera"]
    fill = {"lens": "%g" % cam0["lens_mm"], "eye": "%.2f" % cam0["eye_height_m"], "pitch": "%g" % cam0.get("pitch_deg", 0),
            "fstop": "%g" % cam0["fstop"] if cam0.get("fstop") else "8"}
    steps = []
    for (code, t, en, th), o in zip(STEPS, SCENE_STEPS.get(name, [None] * 7)):
        steps.append((code, o[0], o[1].format(**fill), o[2].format(**fill)) if o else (code, t, en, th))
    faces = info["faces"]
    stages = os.path.join(sd, "stages")
    uri = lambda p: pathlib.Path(p).as_uri()
    fonts_dir = os.path.expanduser("~/Desktop/SV Academy/SV_Logo_v2/fonts")
    wall_fonts = os.path.normpath(os.path.join(HERE, "..", "..", "day2-motion", "wall", "fonts"))
    ff = []
    for fam, p in (("Outfit", os.path.join(fonts_dir, "Outfit-var.ttf")),
                   ("SVThai", os.path.join(wall_fonts, "noto-sans-thai-var-thai.woff2")),
                   ("SVMono", os.path.join(fonts_dir, "JetBrainsMono-var.ttf"))):
        if os.path.exists(p):
            ff.append('@font-face{font-family:%s;src:url("%s");font-weight:100 900}' % (fam, uri(p)))
    CSS = """
%s
:root{--bg:#070C09;--ink:#F1F6F2;--acc:#22C55E;--soft:#A9BAAE}
*{box-sizing:border-box}
html,body{margin:0;width:1920px;height:1080px;overflow:hidden;background:var(--bg);color:var(--ink);
 font-family:Outfit,SVThai,"Noto Sans Thai","Thonburi",sans-serif;-webkit-font-smoothing:antialiased}
.th{font-family:Outfit,SVThai,"Noto Sans Thai","Thonburi",sans-serif}
.mono{font-family:SVMono,Menlo,monospace}
.full{position:absolute;inset:0;width:1920px;height:1080px;object-fit:cover}
.top{position:absolute;left:72px;top:40px;font:500 19px SVMono,monospace;letter-spacing:.06em;color:var(--soft);white-space:pre}
.top b{color:var(--ink);font-weight:600;margin-right:18px}
.scrim{position:absolute;left:0;right:0;bottom:0;height:420px;
 background:linear-gradient(180deg,rgba(4,7,5,0) 0%%,rgba(4,7,5,.55) 45%%,rgba(4,7,5,.9) 100%%)}
.cap{position:absolute;left:72px;right:72px;bottom:66px;display:grid;grid-template-columns:auto 1fr;column-gap:56px;align-items:end}
.num{font:600 22px SVMono,monospace;color:var(--acc);letter-spacing:.08em;display:flex;align-items:center;gap:14px;margin-bottom:10px}
.num:after{content:"";display:block;width:64px;height:2px;background:var(--acc)}
.cap h2{margin:0;font-weight:600;font-size:104px;line-height:.86;letter-spacing:-.035em}
.desc{padding-bottom:8px;max-width:820px}
.desc .en{font-size:30px;line-height:1.2;font-weight:400}
.desc .thl{font-size:27px;line-height:1.45;color:var(--soft);font-weight:400;margin-top:6px}
.tag{position:absolute;background:rgba(4,7,5,.88);border-left:5px solid var(--acc);padding:10px 18px 11px 16px;white-space:nowrap}
.tag .t1{font:600 23px SVMono,monospace;color:var(--ink)}
.tag .t2{font:400 18px SVMono,monospace;color:var(--soft);margin-top:4px;letter-spacing:.03em}
.coord{position:absolute;background:rgba(4,7,5,.86);padding:5px 11px;font:600 21px SVMono,monospace;color:var(--ink);white-space:nowrap}
.coord i{font-style:normal;color:var(--acc);margin-right:10px}
""" % "\n".join(ff)

    def page(body):
        return '<!doctype html><html><head><meta charset="utf-8"><style>%s</style></head><body>%s</body></html>' % (CSS, body)

    def caption(step):
        code, title, en, th = step
        return ('<div class="scrim"></div><div class="cap"><div><div class="num">%s</div><h2>%s</h2></div>'
                '<div class="desc"><div class="en">%s</div><div class="thl th">%s</div></div></div>'
                % (code, esc(title), esc(en), esc(th)))

    def top(right, scrim_=True):
        sc_ = ('<div style="position:absolute;left:0;right:0;top:0;height:170px;'
               'background:linear-gradient(180deg,rgba(4,7,5,.7),rgba(4,7,5,0))"></div>') if scrim_ else ""
        return sc_ + '<div class="top"><b>SV BLENDER</b>%s</div>' % esc(right)

    right = "%s  /  %s" % (name, title_en)
    shots = {}

    def shot(fname, body):
        p = os.path.join(cap, fname)
        html_shot(chrome, page(body), p)
        shots[fname] = p

    def crop_view(src, box, w, h, inner="", margin=0.08, cover_w=False):
        """The image scaled so 'box' (x0, y0, x1, y1 in frame pixels) fits a w x h cell with a margin, centred on it,
        and the picture always covers the cell. 'inner' is HTML in frame pixel space (an SVG or tags) laid over it."""
        x0, y0, x1, y1 = box
        bw, bh = (x1 - x0) * (1 + 2 * margin), (y1 - y0) * (1 + 2 * margin)
        k = w / 1920 if cover_w else min(w / max(bw, 1), h / max(bh, 1))
        k = max(k, w / 1920, h / 1080)
        cx, cy = (x0 + x1) / 2, (y0 + y1) / 2
        ox = min(0, max(w - 1920 * k, w / 2 - cx * k))
        oy = min(0, max(h - 1080 * k, h / 2 - cy * k))
        return ('<div style="position:absolute;left:0;top:0;width:%dpx;height:%dpx;overflow:hidden;background:#070C09">'
                '<div style="position:absolute;left:%.1fpx;top:%.1fpx;width:1920px;height:1080px;transform:scale(%.5f);transform-origin:0 0">'
                '<img src="%s" style="position:absolute;inset:0;width:1920px;height:1080px">%s</div></div>'
                % (w, h, ox, oy, k, uri(src), inner))

    # 01 labels (the logo: the nine imported curves, named where they are)
    labels01 = {}
    lp_ = os.path.join(stages, "01-labels.json")
    if os.path.exists(lp_):
        labels01 = json.load(open(lp_))

    def part_tags(scale_font=1.0):
        out_ = []
        for nm_, (a0, b0, a1, b1) in sorted(labels01.items(), key=lambda kv: kv[1][1]):
            x_, y_ = (a0 + a1) / 2, b0
            out_.append('<div style="position:absolute;left:%.0fpx;top:%.0fpx;transform:translate(-50%%,-100%%);'
                        'background:rgba(4,7,5,.86);border-bottom:3px solid #22C55E;padding:4px 10px;white-space:nowrap;'
                        'font:600 %dpx SVMono,monospace;color:#F1F6F2">%s</div>' % (x_, y_ - 6, round(19 * scale_font), esc(nm_)))
        return "".join(out_)

    for idx, fname in ((0, "01-blockout.png"), (1, "02-materials.png"), (2, "03-light.png")):
        p = os.path.join(stages, fname)
        if not os.path.exists(p):
            continue
        extra = ""
        if idx == 0 and labels01:
            extra = part_tags(1.2)
        elif idx == 0:
            extra = ('<div class="top" style="left:auto;right:72px"><span style="display:inline-block;width:14px;height:14px;'
                     'background:#ff7a1a;margin-right:10px;vertical-align:-1px"></span>camera and its view'
                     '<span style="display:inline-block;width:14px;height:14px;background:#22C55E;margin:0 10px 0 34px;'
                     'vertical-align:-1px"></span>design face</div>')
        shot(fname, '<img class="full" src="%s">%s%s%s' % (uri(p), caption(steps[idx]), top(right), extra))

    # 04: the real lens view: the final frame, a thin frame guide with the focal length, rule-of-thirds lines and the
    # design faces outlined in a hairline
    plate_ = os.path.join(sd, "plate.png")
    cam0_ = info["camera"]

    def lens_view(src, labels=True):
        svg = ['<svg class="full" viewBox="0 0 1920 1080" xmlns="http://www.w3.org/2000/svg">']
        for k in (1, 2):
            svg.append('<line x1="%d" y1="0" x2="%d" y2="1080" stroke="rgba(255,255,255,.42)" stroke-width="1.2"/>' % (640 * k, 640 * k))
            svg.append('<line x1="0" y1="%d" x2="1920" y2="%d" stroke="rgba(255,255,255,.42)" stroke-width="1.2"/>' % (360 * k, 360 * k))
        svg.append('<rect x="48" y="48" width="1824" height="984" fill="none" stroke="rgba(255,255,255,.75)" stroke-width="1.5"/>')
        for (x, y, sx, sy) in ((48, 48, 1, 1), (1872, 48, -1, 1), (48, 1032, 1, -1), (1872, 1032, -1, -1)):
            svg.append('<path d="M%d %d L%d %d L%d %d" fill="none" stroke="#F1F6F2" stroke-width="4"/>' % (x + sx * 46, y, x, y, x, y + sy * 46))
        svg.append('<circle cx="960" cy="540" r="10" fill="none" stroke="rgba(255,255,255,.7)" stroke-width="1.5"/>')
        for f in faces:
            pts = " ".join("%.1f,%.1f" % (c[0], c[1]) for c in f["corners_px"])
            svg.append('<polygon points="%s" fill="none" stroke="#22C55E" stroke-width="2.5" stroke-linejoin="round"/>' % pts)
        if labels:
            svg.append('<text x="68" y="1012" fill="#F1F6F2" style="font:600 22px SVMono,monospace;letter-spacing:.08em">%g MM</text>' % cam0_["lens_mm"])
        svg.append("</svg>")
        return '<img class="full" src="%s">%s' % (uri(src), "".join(svg))
    if os.path.exists(plate_):
        tl = "SV_camera  %g mm  eye %.2f m%s" % (cam0_["lens_mm"], cam0_["eye_height_m"], ("  f/%g" % cam0_["fstop"]) if cam0_.get("fstop") else "")
        shot("04-camera.png", lens_view(plate_) + caption(steps[3]) + top(tl))

    plate = os.path.join(sd, "plate.png")
    rd = info["render"]
    beauty = os.path.join(sd, {"logo-exploded-3d": "beauty-exploded.png"}.get(name, "plate.png"))
    if not os.path.exists(beauty):
        beauty = plate
    shot("05-render.png", '<img class="full" src="%s">%s%s' % (uri(beauty), caption(steps[4]),
         top("Cycles  %d samples  %s %s  bloom, lens, grain" % (rd["samples"], rd["view_transform"], (rd["look"] or "").replace("AgX - ", "")))))

    # 06: three files, each cropped to the hero face plus 15 percent: the plate, the light pass over the dimmed plate,
    # and the mask shown as white over the dimmed plate (so no tile is a black void)
    code, title, en, th = steps[5]
    hero = max(faces, key=lambda f: (f["bbox_px"][2] - f["bbox_px"][0]) * (f["bbox_px"][3] - f["bbox_px"][1]))
    bx0, by0, bx1, by1 = hero["bbox_px"]
    pw_, ph_ = (bx1 - bx0) * 0.15, (by1 - by0) * 0.15
    cx0, cy0, cx1, cy1 = max(0, bx0 - pw_), max(0, by0 - ph_), min(1920, bx1 + pw_), min(1080, by1 + ph_)
    cw_, ch_ = cx1 - cx0, cy1 - cy0
    mask_f = os.path.join(sd, hero.get("mask", "mask.png"))

    # the light pass as luminance only, levels stretched (1st to 99th percentile inside the face), black elsewhere
    from PIL import Image as _Im
    import numpy as _np
    lt_ = _np.asarray(_Im.open(os.path.join(sd, "light.png")).convert("RGBA"), float)
    lum_ = lt_[..., :3] @ _np.array([0.2126, 0.7152, 0.0722])
    al_ = lt_[..., 3] > 128
    if al_.any():
        lo_, hi_ = _np.percentile(lum_[al_], 1), _np.percentile(lum_[al_], 99)
        lv_ = _np.clip((lum_ - lo_) / max(hi_ - lo_, 1), 0, 1) * 255 * al_
    else:
        lv_ = lum_ * 0
    light_levels = os.path.join(stages, "_light_levels.png")
    _Im.fromarray(lv_.astype(_np.uint8)).save(light_levels)

    def crop_tile(kind, w, h):
        # cover: the face plus 15 percent always fits one way; the other way widens into the plate around it
        k = max(w / cw_, h / ch_)
        k = max(k, w / 1920, h / 1080)
        iw, ih = 1920 * k, 1080 * k
        mx_, my_ = (cx0 + cx1) / 2 * k, (cy0 + cy1) / 2 * k
        ox = min(0, max(w - iw, w / 2 - mx_))
        oy = min(0, max(h - ih, h / 2 - my_))
        st = 'position:absolute;left:%.1fpx;top:%.1fpx;width:%.1fpx;height:%.1fpx' % (ox, oy, iw, ih)
        if kind == "plate":
            inner = '<img src="%s" style="%s">' % (uri(plate_), st)
        elif kind == "light":
            inner = '<img src="%s" style="%s">' % (uri(light_levels), st)
        else:
            inner = '<img src="%s" style="%s">' % (uri(mask_f), st)
        lab_ = ('<div style="position:absolute;left:12px;top:10px;font:600 %dpx SVMono,monospace;letter-spacing:.12em;'
                'color:#F1F6F2;background:rgba(4,7,5,.82);padding:4px 9px 5px;border-left:3px solid #22C55E">%s</div>'
                % (max(15, min(22, h // 9)), kind.upper()))
        return ('<div style="position:relative;width:%dpx;height:%dpx;overflow:hidden;background:#000;outline:1px solid #26332b">%s%s</div>'
                % (w, h, inner, lab_))
    panels = [("plate", "plate.png", "BASE LAYER", "The scene, the design face left grey", "ภาพฉาก เว้นหน้างานเป็นสีเทา"),
              ("light", "light.png", "MULTIPLY", "The face's light and shade only (shown in grey, levels stretched)", "เฉพาะแสงเงาบนหน้างาน แสดงเป็นสีเทา ยืดช่วงให้เห็นชัด"),
              ("mask", os.path.basename(mask_f), "LAYER MASK", "White where the design goes", "สีขาวคือพื้นที่วางงานดีไซน์")]
    cells = []
    for i, (kind, fn, role, pen, pth) in enumerate(panels):
        x = 72 + i * (568 + 36)
        cells.append('<div style="position:absolute;left:%dpx;top:352px;width:568px">%s'
                     '<div class="num" style="margin-top:28px">%s</div>'
                     '<div class="mono" style="font-size:40px;font-weight:500;margin-top:2px">%s</div>'
                     '<div style="font-size:25px;margin-top:14px;line-height:1.25">%s</div>'
                     '<div class="th" style="font-size:23px;color:var(--soft);margin-top:6px;line-height:1.45">%s</div></div>'
                     % (x, crop_tile(kind, 568, 360), role, fn, esc(pen), esc(pth)))
    body = (top(right, False) +
            '<div style="position:absolute;left:72px;top:132px;font-weight:600;font-size:96px;letter-spacing:-.035em;line-height:.95">Three files go to PhotoCraft</div>'
            '<div class="th" style="position:absolute;left:76px;top:252px;font-size:30px;color:var(--soft)">ไฟล์สามชิ้นที่ Blender ส่งต่อให้ PhotoCraft วางงานดีไซน์</div>'
            '<div style="position:absolute;right:72px;top:140px;text-align:right"><div class="num" style="justify-content:flex-end">%s</div>'
            '<div style="font-size:40px;font-weight:600">%s</div></div>' % (code, esc(title)) + "".join(cells))
    shot("06-passes.png", body)

    # 07: corners, hairline outline, crosshair rings, coordinates in mono
    svg = ['<svg class="full" viewBox="0 0 1920 1080" xmlns="http://www.w3.org/2000/svg">']
    labels = []
    names = ["TL", "TR", "BR", "BL"]
    for f in faces:
        pts = f["corners_px"]
        svg.append('<polygon points="%s" fill="none" stroke="rgba(255,255,255,.95)" stroke-width="2"/>'
                   % " ".join("%.1f,%.1f" % (c[0], c[1]) for c in pts))
        cx_ = sum(c[0] for c in pts) / 4
        cy_ = sum(c[1] for c in pts) / 4
        for k, (x, y) in enumerate(pts):
            svg.append('<circle cx="%.1f" cy="%.1f" r="13" fill="none" stroke="#22C55E" stroke-width="3"/>' % (x, y))
            for (dx1, dy1, dx2, dy2) in ((-24, 0, -7, 0), (7, 0, 24, 0), (0, -24, 0, -7), (0, 7, 0, 24)):
                svg.append('<line x1="%.1f" y1="%.1f" x2="%.1f" y2="%.1f" stroke="#22C55E" stroke-width="2"/>' % (x + dx1, y + dy1, x + dx2, y + dy2))
            tw = 15 * len("%s %d, %d" % (names[k], round(x), round(y))) + 24
            lx = x + 26 if x >= cx_ else x - 26 - tw
            ly = y - 54 if y <= cy_ else y + 20
            if ly < 96:
                ly = y + 20
            lx = min(max(lx, 8), 1912 - tw)
            ly = min(max(ly, 96), 1036)
            labels.append('<div class="coord" style="left:%dpx;top:%dpx"><i>%s</i>%d, %d</div>' % (lx, ly, names[k], round(x), round(y)))
    svg.append("</svg>")
    shot("07-corners.png", '<img class="full" src="%s" style="filter:brightness(.8)">%s%s%s%s'
         % (uri(plate), "".join(svg), "".join(labels), caption(steps[6]),
            top("corners.json  %d face%s  pixels from the top left" % (len(faces), "" if len(faces) == 1 else "s"))))

    # clean thumbnails for the storyboard: the picture only, no caption or tags (the storyboard sets its own)
    def clean(fname, body, w=1920, h=1080):
        p_ = os.path.join(stages, fname)
        html_shot(chrome, page(body).replace("width:1920px;height:1080px;overflow:hidden", "width:%dpx;height:%dpx;overflow:hidden" % (w, h)), p_, w, h)
        return p_
    thumbs = {}
    if os.path.exists(plate_):
        thumbs["04-camera.png"] = clean("_thumb-04.png", lens_view(plate_, labels=True))
    # 06: the three passes as three equal tiles, each the hero face plus 15 percent
    CW, CH, G = 852, 640, 14            # the storyboard's 06 cell at twice its size
    if cw_ / ch_ > 1.15:                # a wide face: three strips stacked
        hh_ = (CH - 2 * G) // 3
        tiles = "".join('<div style="position:absolute;left:0;top:%dpx">%s</div>' % (i * (hh_ + G), crop_tile(k_, CW, hh_))
                        for i, k_ in enumerate(("plate", "light", "mask")))
    else:                               # a tall or square face: three columns
        ww_ = (CW - 2 * G) // 3
        tiles = "".join('<div style="position:absolute;left:%dpx;top:0">%s</div>' % (i * (ww_ + G), crop_tile(k_, ww_, CH))
                        for i, k_ in enumerate(("plate", "light", "mask")))
    thumbs["06-passes.png"] = clean("_thumb-06.png", '<div style="position:absolute;inset:0;background:#070C09">%s</div>' % tiles, CW, CH)
    rings = "".join('<polygon points="%s" fill="none" stroke="#fff" stroke-width="4"/>' % " ".join("%.1f,%.1f" % (c[0], c[1]) for c in f["corners_px"])
                    + "".join('<circle cx="%.1f" cy="%.1f" r="22" fill="none" stroke="#22C55E" stroke-width="6"/>' % (c[0], c[1]) for c in f["corners_px"])
                    for f in faces)
    allx = [c[0] for f in faces for c in f["corners_px"]]
    ally = [c[1] for f in faces for c in f["corners_px"]]
    ubox = (min(allx), min(ally), max(allx), max(ally))
    # 07: cropped wide enough that every corner of every face sits inside the tile
    thumbs["07-corners.png"] = clean("_thumb-07.png", crop_view(plate_, ubox, 852, 640, '<svg style="position:absolute;inset:0" width="1920" height="1080" viewBox="0 0 1920 1080">%s</svg>' % rings, 0.1),
                                     852, 640)
    # 05: the render at full width, the band chosen so the focus face (or all faces) sits inside it
    focus = {"print-still-life": "card-back"}.get(name)
    fb_ = next((f["bbox_px"] for f in faces if f["name"] == focus), None) or list(ubox)
    if fb_[3] - fb_[1] > 1080 * 880 / 1920 * 0.9:
        fb_ = max(faces, key=lambda f: (f["bbox_px"][2] - f["bbox_px"][0]) * (f["bbox_px"][3] - f["bbox_px"][1]))["bbox_px"]
    thumbs["05-render.png"] = clean("_thumb-05.png", crop_view(beauty, fb_, 1760, 640, "", 0.0, cover_w=True), 1760, 640)
    # 01 with the part names (the logo), tight on the parts
    if labels01:
        lb = (min(v[0] for v in labels01.values()), min(v[1] for v in labels01.values()) - 40,
              max(v[2] for v in labels01.values()), max(v[3] for v in labels01.values()))
        thumbs["01-blockout.png"] = clean("_thumb-01.png", crop_view(os.path.join(stages, "01-blockout.png"), lb, 852, 464, part_tags(2.6), 0.04),
                                          852, 464)
    # storyboard: four small steps on top, then 05 Render twice the width as the climax, 06 and 07 beside it
    raw = {"01-blockout.png": os.path.join(stages, "01-blockout.png"), "02-materials.png": os.path.join(stages, "02-materials.png"),
           "03-light.png": os.path.join(stages, "03-light.png"), "05-render.png": beauty, **thumbs}
    files = ["01-blockout.png", "02-materials.png", "03-light.png", "04-camera.png", "05-render.png", "06-passes.png", "07-corners.png"]
    tw_, gx = 426, 28
    th_ = 232
    cells = []
    cam = info["camera"]
    rs = info.get("render_seconds", {})
    total = info.get("total_seconds") or sum(v for k, v in rs.items())

    def cell(i, x, y, w, h, fit="cover"):
        fn = files[i]
        src = raw.get(fn) if os.path.exists(raw.get(fn, "")) else os.path.join(cap, fn)
        code, title, en, th = steps[i]
        big = w > tw_ + 10
        return ('<div style="position:absolute;left:%dpx;top:%dpx;width:%dpx">'
                '<div style="width:%dpx;height:%dpx;background:#0d120f;outline:1px solid #1d2a22;overflow:hidden">'
                '<img src="%s" style="width:100%%;height:100%%;object-fit:%s;display:block"></div>'
                '<div style="display:flex;align-items:baseline;gap:12px;margin-top:14px"><span class="mono" style="color:var(--acc);font-size:%dpx;font-weight:600">%s</span>'
                '<span style="font-size:%dpx;font-weight:600;letter-spacing:-.02em">%s</span></div>'
                '<div style="font-size:19px;margin-top:5px;opacity:.88">%s</div>'
                '<div class="th" style="font-size:18px;color:var(--soft);margin-top:2px">%s</div></div>'
                % (x, y, w, w, h, uri(src), fit, 22 if big else 18, code, 40 if big else 30, esc(title), esc(en), esc(th)))
    y1 = 214
    for i in range(4):
        cells.append(cell(i, 72 + i * (tw_ + gx), y1, tw_, th_))
    y2 = y1 + th_ + 142
    bw2 = 2 * tw_ + gx
    bh2 = 320
    cells.append(cell(4, 72, y2, bw2, bh2))
    cells.append(cell(5, 72 + 2 * (tw_ + gx), y2, tw_, bh2, "fill"))
    cells.append(cell(6, 72 + 3 * (tw_ + gx), y2, tw_, bh2))
    facts = [("LENS", "%g mm, eye %.2f m" % (cam["lens_mm"], cam["eye_height_m"])),
             ("RENDER", "Cycles, %d samples, Metal" % rd["samples"]),
             ("TIME", ("%d min %02d s, headless" % (int(total // 60), int(round(total % 60)))) if total >= 60 else "%d s, headless" % round(total))]
    fact_html = "".join('<div style="text-align:right;margin-left:44px"><div class="mono" style="color:var(--acc);font-size:14px;font-weight:600;letter-spacing:.1em">%s</div>'
                        '<div style="font-size:21px;margin-top:4px;white-space:nowrap">%s</div></div>' % (k, esc(v)) for k, v in facts)
    body = (top(name, False) +
            '<div style="position:absolute;left:72px;top:84px;font-size:64px;font-weight:600;letter-spacing:-.03em;line-height:1">%s</div>'
            '<div class="th" style="position:absolute;left:74px;top:158px;font-size:24px;color:var(--soft)">%s ประกอบฉากใน Blender ทีละขั้น</div>'
            '<div style="position:absolute;right:72px;top:30px;display:flex">%s</div>'
            % (esc(title_en), esc(title_th), fact_html) + "".join(cells))
    shot("storyboard.png", body)
    print("SV_ANNOTATED", cap)


CODE_LINES = [
    (0, "// sv-academy: seat booking, cohort 01"),
    (0, "import { db } from \"./db\";"),
    (0, "import { notify } from \"./line\";"),
    (0, ""),
    (0, "export async function bookSeat(name, phone) {"),
    (1, "const seats = await db.seats.open(\"entrepreneurs\");"),
    (1, "if (seats.left === 0) {"),
    (2, "return { ok: false, waitlist: true };"),
    (1, "}"),
    (1, "const seat = await db.seats.claim(name, phone);"),
    (1, "await notify(phone, seat.number);"),
    (1, "return { ok: true, seat };"),
    (0, "}"),
    (0, ""),
    (0, "export function Ticket({ seat }) {"),
    (1, "return <Card title={seat.number} />;"),
    (0, "}"),
]
CODE_KEYWORDS = {"import", "from", "export", "async", "function", "const", "if", "return", "await"}


def code_texture(out, w=1820, h=1020):
    """A wall screen's code, as an image: monospace lines, two indent levels, mostly 40 percent grey, green only on
    the keywords, a comment line darker. Rendered with Pillow (plain python3), loaded by Blender as an emission map."""
    from PIL import Image, ImageDraw, ImageFont
    fonts = [os.path.expanduser("~/Desktop/SV Academy/SV_Logo_v2/fonts/JetBrainsMono-var.ttf"), "/System/Library/Fonts/Menlo.ttc"]
    lh = h / (len(CODE_LINES) + 1.5)
    size = int(lh * 0.62)
    font = None
    for f in fonts:
        if os.path.exists(f):
            font = ImageFont.truetype(f, size)
            break
    im = Image.new("RGB", (w, h), (8, 11, 10))
    d = ImageDraw.Draw(im)
    grey, green, dim, num = (102, 106, 104), (34, 197, 94), (58, 64, 61), (46, 52, 49)
    cw = d.textlength("m", font=font)
    x0 = int(cw * 4.2)
    for i, (ind, line) in enumerate(CODE_LINES):
        y = int(lh * (i + 0.8))
        d.text((int(cw * 0.8), y), "%2d" % (i + 1), font=font, fill=num)
        x = x0 + ind * cw * 2
        if line.startswith("//"):
            d.text((x, y), line, font=font, fill=dim)
            continue
        for tok in __import__("re").split(r"(\W)", line):
            if not tok:
                continue
            d.text((x, y), tok, font=font, fill=green if tok in CODE_KEYWORDS else grey)
            x += d.textlength(tok, font=font)
    # the cursor on the last line
    d.rectangle([x0, int(lh * (len(CODE_LINES) + 0.85)), x0 + int(cw * 0.9), int(lh * (len(CODE_LINES) + 0.85) + size)], fill=green)
    im.save(out)
    return out


if __name__ == "__main__":
    if IN_BLENDER:
        blender_main()
    elif len(sys.argv) > 1 and sys.argv[1] == "codetex":
        code_texture(sys.argv[2])
    elif len(sys.argv) > 1 and sys.argv[1] == "annotate":
        annotate_main(sys.argv[2:])
    else:
        print(__doc__)
```
