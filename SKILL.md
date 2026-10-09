---
name: sv-edits
description: SV Academy's own editing skill. Make and edit real design files by prompting in Claude Code or Codex, free, without opening an editing app: layered PSD with live type, .ai with SVG and PDF for the print shop, mockups (an app screen in a phone, a screen swapped in one call, a logo or post on a product shot), vectors from words (logo lockup, icon set, die-cut sticker with a cut line), and clips you already have, trimmed, cut together, made vertical and exported as MP4. The agent installs free command-line tools once, shows every result in the chat and saves it as a new file next to the original. Trigger on "SV Edits", "make a PSD", "the printer wants an .ai", "logo in every format", "Thai text on this photo", "phone mockup", "logo lockup", "icon set", "die-cut sticker", "trim my video", "ทำไฟล์ PSD", "ไฟล์โลโก้สำหรับโรงพิมพ์", "ทำม็อกอัป", "ม็อกอัพ", "ม็อคอัพ", "ม็อคอัป", "สติกเกอร์ไดคัท", "สติ๊กเกอร์ไดคัท", "ไดคัต", "ตัดคลิป". Not for new video from words, titles, captions or motion graphics (SV Motion), AI-generated images, or Office files.
---

# SV Edits. Real design files, by prompting.

ไฟล์งานออกแบบตัวจริง สั่งด้วยการพิมพ์

SV Academy's own working method for design files with an agent. The files are the ones designers and print shops use every day: a layered PSD, an .ai with its SVG and PDF, an MP4. You make and edit them by prompting in Claude Code or Codex, for free. You never open an editing app: the agent drives three free command-line tools on your own computer, shows you every result as a picture in the chat, and saves it as a new file next to the original.

Your files stay on your computer, and no file goes to an editing service or account. Your AI provider (Anthropic or OpenAI) sees what the agent reads and the previews it looks at, like any image in a chat. Other network use: the one-time download of the tools, the macOS notarization check, `doctor`'s reachability check of github.com, and, only after a yes, a font or desktop app download (in Codex, also the one fetch of this file).

The tools are three open source editors from storytold (Apache-2.0 or MIT): one for photos and PSD files, one for logos and vector files, one for footage. The agent installs only their command-line builds, pinned below, and runs them headless as `photocraft-cli`, `vectorcraft-cli` and `filmcraft-cli`. Their own `--help` and `commands` lists are the authority on flags.

This is one self-contained file. Everything the skill needs is in here, including the two helper scripts at the end, under "Helper scripts". Pre-flight writes the one for this computer first.

**How to read this file.** It is long, and more than half of it is the two helper scripts. Every rule comes before the heading "Helper scripts": read all of that part before the first step, in pieces if your tool cuts long output (for example `sed -n '1,400p'`, then the next 400 lines, up to "Helper scripts"). The helper blocks after it are code: Pre-flight step 2 extracts them and holds their sha256 pins, so there is no need to read past "Helper scripts", and the Windows block is last.

## Prompt only: the person never needs to open an app

This is the core of SV Edits. Hold to it on every step.

- The person installs SV Edits once, with one prompt, and from then on only types what they want. They never need to open an editing app, a timeline, a layers panel or an installer.
- The skill installs command-line tools and registers their MCP servers. The helper never keeps, installs or launches an app: no DMG, no MSI, no `.app`, no `.exe` other than the `*-cli.exe` tools. On Windows the release zip also holds the desktop app; the helper deletes it with the download, unrun. The one exception is "Want to see it in the app?": a person who asks to see a file in the free desktop app gets it installed by those separate steps, and only then.
- Never start anything that opens a window: no `open`, `start`, `Invoke-Item`, `xdg-open`, no `--bridge`, `--connect` or `--control`, and none of the GUI-only MCP tools listed under "Working inside a document". The only exception: the person asks the agent to open a file in an app they installed (see "Want to see it in the app?").
- **Every result comes back to the chat as a picture.** The agent makes a preview with `look` (or `frames` for a clip) and opens it itself: Claude Code with the Read tool, Codex with its image viewer. Never ask the person to open a file to check it.
- **Every result is a new file next to the original.** The original is fingerprinted first and checked after. See "Where files go".
- Anything that needs hands in an app is out of scope: tracking a moving subject to reframe, editing on a visual timeline, and hand-off files for a human editor (no FCPXML, EDL or OTIO). If a job cannot be finished by prompting alone, say so in one sentence and stop. Never suggest the person open an editor to finish it: the optional app is for looking and small changes by hand, never a step a job depends on.
- **Tool advice that points to an app is not passed on.** Some warnings are written for the desktop apps, for example the vector tool's "install them, or replace them with Find Font" (Find Font is a menu in the app). Report the warning word for word, then give the prompt-only fix instead: an installed face named in the prompt, `--outline-text`, or installing the font file after a yes (see "Fonts this computer does not have").

**Status tags.** Every step carries one:

- **tested**: ran on macOS on the pinned builds, on real files, on 8 or 9 Oct 2026.
- **documented**: in the tool's own `--help` or command list, not run.
- **untested**: not run. Before an untested step, say so to the person in one sentence. Every Windows line is untested until a Windows laptop runs it: on Windows, say once per job that the Windows route is untested, and name the untested outputs at hand-over.

## When to use

Load SV Edits when the person wants to make or change a PSD, AI, SVG, PDF, EPS, photo or existing clip by prompting, without opening an editor. Typical requests: "make a PSD", "layered PSD from my brand", "edit this PSD", "change the words in my PSD", "what layers are in this PSD", "export every layer", "PSD to PNG", "make an .ai", "save my logo as .ai", "the printer wants an .ai", "logo in every format", "logo as PDF for the printer", "logo to SVG", "white version of my logo", "one colour logo", "brand board", "every size from one design", "Instagram sizes", "Story size of this PSD", "put Thai text on this photo", "export for LINE", "export for print", "trim my video", "cut these parts together", "vertical version for Reels", "9:16 from my footage", "grab a frame from this video", "cover from my video", "edit without Photoshop", "no Adobe", "photocraft", "vectorcraft", "filmcraft", "ทำไฟล์ PSD", "แก้ไฟล์ PSD", "แก้ข้อความใน PSD", "แยกเลเยอร์", "ทำไฟล์ .ai", "ไฟล์โลโก้สำหรับโรงพิมพ์", "โลโก้สีขาว", "ทำรูปทุกขนาด", "ทำขนาดสตอรี่", "ใส่ข้อความภาษาไทยบนรูป", "ตัดคลิป", "ต่อคลิป", "ทำคลิปแนวตั้ง", "แคปภาพจากวิดีโอ", "ทำปกจากวิดีโอ", "phone mockup", "app screen in a mockup", "swap the screen in my mockup", "logo on my product photo", "post on a product shot", "logo lockup", "icon set", "icons for my features", "die-cut sticker", "sticker with a cut line", "ทำม็อกอัป", "ม็อกอัพ", "ม็อคอัพ", "ม็อคอัป", "ใส่หน้าจอแอปในมือถือ", "เปลี่ยนหน้าจอในม็อกอัป", "วางโลโก้บนรูปสินค้า", "ทำไอคอน", "สติกเกอร์ไดคัท", "สติ๊กเกอร์ไดคัท", "สติ๊กเกอร์ไดคัต", "ไดคัต", "open it in PhotoCraft", "อยากเปิดดูในโปรแกรม".

Not for: new video from words, titles, captions, subtitles or motion graphics on video (SV Motion); AI-generated images; Office files.

## Three ways in

Pick the way from what the person has. Ask only if you cannot tell.

| You have | Way | What you do |
| --- | --- | --- |
| The Claude Code or Codex session where you built your product (**Easiest**) | 1. From your product | Read the product's code without changing it: logo, colours, fonts, name and promise, each with `file:line`. Say the brand back. Then make a layered PSD post where every line is live type, and a brand .ai with SVG and PDF. |
| A PSD, AI, SVG, PDF or photo | 2. From a file you have | Open it and report what is inside (size, layers, live text, fonts, warnings). Then edit it by the person's words: change words, colours or sizes, export for print or for LINE. Save new files next to it; never touch the original. |
| Phone or camera footage, or a reel made earlier | 3. From a clip you already have | Probe it, show frames and a cut list in seconds, and wait for a yes. Then trim, cut together, reorder, make a vertical version, grab frames (they become PSD covers) and export MP4. Titles and captions go to SV Motion. |

All three end in the same place: new files next to the source, a preview of each one shown in the chat, the original proven unchanged by `check`, and a short list of what was done, what the tools warned and what is untested. From then on the three are the same: you look, you fix it in words, and the original never changes. (จากนั้นทั้งสามทางเหมือนกัน: ดูก่อน แก้ด้วยคำพูด และไฟล์ต้นฉบับไม่เปลี่ยน)

**Design jobs on top of ways 1 and 2.** Mockups (an app screen in a phone, a screen swapped in one call, a logo or post on a product shot) and vectors from words (a logo lockup, an icon set, a die-cut sticker with a cut line) start from the product or from a file: see "Mockups and vectors from words".

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

## Pin the versions: photo 0.3.0, vector 0.5.0, film 0.2.1

Run every tool through the helper, which runs only the pinned build. Never latest, never a redirect URL, never a copy found on PATH. A class on one Wi-Fi must get identical behaviour, and these tools ship almost daily (the vector tool had three releases on 8 Oct alone). Upgrade on purpose: change the pin table in both helpers and the stamp on their first line, run every preset again, then publish.

URL form: `https://github.com/storytold/<app>/releases/download/v<version>/<asset>`, checked against the `SHA256SUMS.txt` in the same release. Sizes are bytes; digests are GitHub's asset digests, re-read on 9 Oct 2026.

| Tool | Helper verb | macOS (universal CLI zip) | Windows x64 | Windows arm64 | Windows x86 |
| --- | --- | --- | --- | --- | --- |
| Photos and PSD, 0.3.0 | `photo` | `photocraft-cli-0.3.0-macos-universal.zip`, 55,961,526, `29346d31cc054622b89f510addee70f598b65a1758f1a08010f47e18249346db` | `photocraft-0.3.0-windows-x64-portable.zip`, 65,251,740, `9997f7df3a6f0717b46bb0f503b3d920c3beedeb9e9c4e1ff2ef3b8671acd47e` | `photocraft-0.3.0-windows-arm64-portable.zip`, 62,702,095, `48778db8360577807f9ff875c80dfec007ba5d07a848d1b72af4653cd20ad28e` | `photocraft-0.3.0-windows-x86-portable.zip`, 63,078,284, `8a4e8aabb7537806729bba4dad53518c3b9c43d22fdb0e65a9bab5db7e9b9a75` |
| Logos and vectors, 0.5.0 | `vector` | `vectorcraft-cli-0.5.0-macos-universal.zip`, 64,607,865, `64307411bc17988827dd5ce417798b3b5c20450a1ce5a4c7738202b24030b3d0` | `vectorcraft-0.5.0-windows-x64-portable.zip`, 77,469,769, `98929a4a49383a16cc2e12da92281efc0bd2c7dc44112adf11859689b2c2e4c1` | `vectorcraft-0.5.0-windows-arm64-portable.zip`, 73,123,015, `2ba33f1c3e6a0bca2ac06386bbfb5472d82004522d2e6aceb93a71c9aab28e71` | `vectorcraft-0.5.0-windows-x86-portable.zip`, 74,188,285, `72df3fc76ac2a2db8d5cfee0d67664126e4b59699b668bae69ced845352efb65` |
| Footage, 0.2.1 (way 3 only) | `film` | `filmcraft-cli-0.2.1-macos-universal.zip`, 24,675,100, `18535c3c82e4de9156251d73cf4d8f3626638ee3979063dcf4c966e811b666f6` | `filmcraft-0.2.1-windows-x64-portable.zip`, 33,724,271, `5306251e23de051523895dfa791ad3e3324c9865944df3e6a4ad590521657942` | no build: x64 under emulation on Windows 11, x86 on Windows 10 (untested) | `filmcraft-0.2.1-windows-x86-portable.zip`, 32,902,572, `e401205d3b4dd124398660b33bdd890364832c90704e1fdc9a5995037f27e1d0` |

What is inside each zip (read from each repository's `.github/workflows/release.yml` and the `packaging/windows/package.ps1` it calls, at the pinned tags, on 9 Oct): the macOS zip holds the one signed `<app>-cli` binary. The Windows portable zip holds one folder, `<app>-<version>-windows-<arch>-portable\`, with `<app>.exe` (the desktop app) and `<app>-cli.exe` (the command-line tool, a console program), plus licences. The helper keeps only `<app>-cli.exe` and the licences. The desktop app is deleted with the download and never runs.

Download sizes: photo and vector together, 121 MB on a Mac, 143 MB on Windows x64, 136 MB on arm64, 137 MB on x86. The footage tool adds 25 MB (Mac) to 34 MB (Windows).

On disk after install: on a Mac, about 240 MB for photo and vector (109 MB and 131 MB unpacked) plus 62 MB for footage, and about 330 MB free at the peak of the install while a tool unpacks; on Windows x64, about 130 MB for photo and vector, because only the command-line tools are kept.

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
curl -fsSL --proto '=https' --create-dirs https://raw.githubusercontent.com/sva-admin/sv-edits/v1.0.0/SKILL.md -o ~/.sv-edits/SKILL.md
```

```powershell
# PowerShell on Windows (untested): curl.exe, because plain curl is Invoke-WebRequest in PowerShell 5.1, and $HOME, because 5.1 passes ~ to curl.exe as a folder named ~
curl.exe -fsSL --proto "=https" --create-dirs "https://raw.githubusercontent.com/sva-admin/sv-edits/v1.0.0/SKILL.md" -o "$HOME\.sv-edits\SKILL.md"
```

That URL is the same one the person pasted (see "Codex and other agents"), so the copy on disk is the file being followed. The macOS helper is the first block that starts with `# sv-edits helper v1:`, the Windows helper the second.

**Helper pins.** The sha256 of each helper block, extracted as below (line endings LF, ending with the block's last newline):

- macOS `edits.sh`: `d67a33d5d58e290d7e59701045f2a41b6c993fa194ba6a9250472ead24b81224`
- Windows `edits.ps1`: `a3c5cdace6ff7c76f4089ba51f1c82b6077aa197542e143d77549ae741781eac`

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

> SV Edits needs two free tools for PSD and .ai files (about 121 MB to download from github.com/storytold and about 240 MB on disk, checked against this file and kept in ~/.sv-edits; no app is installed). It will also add two editing servers to Claude Code, for this project folder only: the photo server can only reach this project folder; the vector server can reach any path, and SV Edits only gives it paths inside this project. OK?
>
> SV Edits ต้องใช้เครื่องมือฟรีสองตัวสำหรับไฟล์ PSD และ .ai (ดาวน์โหลดประมาณ 121 MB จาก github.com/storytold และใช้พื้นที่ราว 240 MB ตรวจกับไฟล์นี้แล้วเก็บไว้ที่ ~/.sv-edits ไม่มีการติดตั้งแอป) และจะเพิ่มเซิร์ฟเวอร์สำหรับแก้ไฟล์สองตัวใน Claude Code เฉพาะโฟลเดอร์โปรเจกต์นี้ โดยเซิร์ฟเวอร์รูปภาพเข้าถึงได้เฉพาะโฟลเดอร์โปรเจกต์นี้ ส่วนเซิร์ฟเวอร์เวกเตอร์เข้าถึงได้ทุกที่ในเครื่อง และ SV Edits จะส่งให้เฉพาะไฟล์ในโปรเจกต์นี้ ตกลงไหม

**In Codex, ask about the tools only**, and leave the servers out of the question:

> SV Edits needs two free tools for PSD and .ai files (about 121 MB to download from github.com/storytold and about 240 MB on disk, checked against this file and kept in ~/.sv-edits; no app is installed). OK?
>
> SV Edits ต้องใช้เครื่องมือฟรีสองตัวสำหรับไฟล์ PSD และ .ai (ดาวน์โหลดประมาณ 121 MB จาก github.com/storytold และใช้พื้นที่ราว 240 MB ตรวจกับไฟล์นี้แล้วเก็บไว้ที่ ~/.sv-edits ไม่มีการติดตั้งแอป) ตกลงไหม

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
5. **No window, unless the person asks for one.** Headless commands only; see "Prompt only" and "Want to see it in the app?".
6. **Ask before anything heavy:** a PSD over about 500 MB, a long 4K export, a ProRes master.
7. **Clip projects live only in `~/.sv-edits/projects/`.**
8. **Files stay on the computer.** No file goes to an editing service, no account, no key. The AI provider sees what the agent reads and the previews, like any chat image. Other network use: each tool's one-time download, macOS's notarization check, `doctor`'s reachability check, and, only after a yes, a font or desktop app download (in Codex, also the one fetch of this file).
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

## Fix it in words

Read the file and its params first. Change only what was asked, run that one job again into a new name, show it, `check`. Leave every other output alone.

- The PSD keeps live type and the logo layer; the .ai keeps paths and live text; the clip project keeps the timeline.
- "Make the Thai line bigger": one `size` in the params file, `type.edit`, convert, show.
- "White logo instead": one `recolor.apply`, export, show.
- "Start the cut a second later": one `sourceIn` in `cut.jsonl`, build, export, show.

## Want to see it in the app? (optional)

อยากเปิดดูในโปรแกรม (ไม่บังคับ)

Prompting stays the default, and nothing in SV Edits needs an app. A class or a demo never depends on one being open. But anyone who wants to **see** a file the agent made, or move something by hand, can use the free open source desktop apps of the same two tools, at the same pinned versions:

- **PhotoCraft 0.3.0** opens the PSD files (and PNG, JPEG, TIFF, WebP).
- **VectorCraft 0.5.0** opens the .ai (PDF-compatible), SVG and PDF files.
- Clips have no app path here: SV Edits teaches no timeline editing.

**Only to look, on a Mac: nothing to install.** Quick Look (select the file in Finder and press Space) or Preview shows a flat view of a PSD and of a PDF-compatible .ai (tested 9 Oct with `qlmanage -t` thumbnails of the M1 PSD and the V3 .ai: both drawn right, Thai included). Offer that first. PhotoCraft or VectorCraft is for seeing the layers or changing something by hand.

**Rules for the agent.**

- **Look for an app that is already there first**, before any question about a download. On a Mac: `ls -d /Applications/PhotoCraft.app ~/Applications/PhotoCraft.app 2>/dev/null` (and the same for VectorCraft), then the step 3 checks on the path found. If they pass at the pinned version, nothing is downloaded: say where it is. On Windows: the search in "On Windows" below.
- Install an app **only when the person asks for it**, after one question that names the app, the download size and where it goes. Never as part of Pre-flight, a preset or a fix.
- **Never launch it unless asked.** When the person asks the agent to open a file in the app: `open -a PhotoCraft "<file.psd>"` or `open -a VectorCraft "<file.ai>"` on a Mac (untested), `Start-Process "<path to the app exe>" -ArgumentList '"<file>"'` on Windows (untested). Otherwise say where the file is and let the person open it.
- The helper does none of this: it stays command-line only, and its checks are unchanged. These are separate steps, run by hand from this section.
- Every download is checked like the tools: the size and sha256 must equal the pins below and the line in that release's `SHA256SUMS.txt`. If either differs, delete the download, stop and report the exact output.

Pins (GitHub asset digests, checked against each release's `SHA256SUMS.txt` on 9 Oct 2026). URL form as for the tools: `https://github.com/storytold/<app>/releases/download/v<version>/<asset>`.

| App | macOS (DMG, universal) | Windows x64 (MSI) | Windows arm64 (MSI) | Windows x86 (MSI) |
| --- | --- | --- | --- | --- |
| PhotoCraft 0.3.0 | `photocraft-0.3.0-macos-universal.dmg`, 71,292,940, `c0b0223cddb18dd7f5607fb4d6cc6a925a62f5b5b47457997d61ad0f72aa5911` | `photocraft-0.3.0-windows-x64.msi`, 53,895,168, `2b3e1bfdacfb597c1cab783c9ed14ed59854cc4bf11f333973d1c823d77e9db6` | `photocraft-0.3.0-windows-arm64.msi`, 51,122,176, `990a6c05a5a3de92ed4e1410546624db8f1eb696589db15beeca730e268d1316` | `photocraft-0.3.0-windows-x86.msi`, 52,789,248, `7294cce12465d9755b776f1c558050367c49c795af84fba28e4428f73404d18f` |
| VectorCraft 0.5.0 | `vectorcraft-0.5.0-macos-universal.dmg`, 83,864,169, `871c6099353574719478819b63ec7319c4f3f71cd61b0053d4c8e2da0820d0e2` | `vectorcraft-0.5.0-windows-x64.msi`, 61,227,008, `64395a763c106280fdde7b95d3cb0e97a7fb1a9a963bf4b6008bad3e11e7b733` | `vectorcraft-0.5.0-windows-arm64.msi`, 56,516,608, `190f9a6963eb654abc98fc23bdc62c140b0df5d19f9f01a83bed0faea9305ab9` | `vectorcraft-0.5.0-windows-x86.msi`, 59,367,424, `998534e6c1f29b10418fdd047b6176048e5eef080a3d8972edbea590b94de97b` |

The Windows portable zips are the ones already pinned under "Pin the versions"; each holds the app as `<app>.exe` in its folder.

### On a Mac

1. **Download and check**, after the person's yes (shown for PhotoCraft; VectorCraft is the same with its own name, version and pin). The DMG goes to `~/Downloads`, where the person can see it (Finder hides `~/.sv-edits`):

```bash
curl -fL --proto '=https' -o ~/Downloads/photocraft-0.3.0-macos-universal.dmg \
  https://github.com/storytold/photocraft/releases/download/v0.3.0/photocraft-0.3.0-macos-universal.dmg
stat -f %z ~/Downloads/photocraft-0.3.0-macos-universal.dmg          # must be 71292940
shasum -a 256 ~/Downloads/photocraft-0.3.0-macos-universal.dmg       # must equal the pin
curl -fsSL https://github.com/storytold/photocraft/releases/download/v0.3.0/SHA256SUMS.txt | grep 'macos-universal.dmg'   # the same hash
```

If a file of that name is already in Downloads, check it the same way before downloading again.

2. **The person drags it in.** Give them a clickable link to the DMG (`file:///Users/<name>/Downloads/photocraft-0.3.0-macos-universal.dmg`, and the folder `file:///Users/<name>/Downloads/`). They double-click it and drag PhotoCraft to Applications. This is the one step done by hand. The agent never runs `open` on the DMG, and never mounts it, unless the person asks for exactly that.
   - **If macOS asks for a password** (a standard, non-admin account), the person clicks Cancel and drags PhotoCraft to the `Applications` folder inside their home folder instead (`~/Applications`; the agent may create it with `mkdir -p ~/Applications` and give its `file://` link). No password is ever needed.
3. **Check what landed**, on whichever path holds the app, `/Applications/PhotoCraft.app` or `~/Applications/PhotoCraft.app` (tested 9 Oct on this release's apps in /Applications on the test Mac, read only, nothing opened):

```bash
APP=/Applications/PhotoCraft.app; [ -d "$APP" ] || APP=~/Applications/PhotoCraft.app
spctl --assess --type execute -vv "$APP"      # accepted, source=Notarized Developer ID, Learning Machines LLC (DJ6XS33FX8)
/usr/libexec/PlistBuddy -c 'Print :CFBundleShortVersionString' "$APP/Contents/Info.plist"   # 0.3.0
```

If `spctl` says anything else, stop and report it. Never remove quarantine or bypass Gatekeeper.

4. **Open it.** The app is notarized and `spctl` has just accepted it, so it opens like any app. (A file fetched with `curl` carries no "downloaded from the internet" mark, so macOS does not ask its usual first-open question; the `spctl` check in step 3 is the proof instead.) Then File, Open, and pick the PSD or .ai the agent made, or drag the file onto the app's icon.
5. Once the app is in Applications, the DMG can go: the agent deletes `~/Downloads/<asset>` after a yes. To remove the app later, the person drags it to the Trash.

### On Windows (untested)

**Already installed?** Look first, read only:

```powershell
Get-ChildItem "$env:ProgramFiles", "${env:ProgramFiles(x86)}", "$env:LOCALAPPDATA\Programs", "$HOME\.sv-edits\apps" -Recurse -Depth 3 -Filter photocraft.exe -ErrorAction SilentlyContinue | Select-Object -ExpandProperty FullName
```

A path found there is `<path to the app exe>` for `Start-Process`; check its signature (`Get-AuthenticodeSignature`, Status `Valid`, a Learning Machines subject) and its version (`(Get-Item "<path>").VersionInfo.ProductVersion`, 0.3.0) before using it. The same search with `vectorcraft.exe` finds VectorCraft.

Otherwise pick one:

- **The MSI installer** for the computer's CPU (the helper's `doctor` names it: x64, arm64 or x86). Download it to `%USERPROFILE%\.sv-edits\apps\` with `curl.exe -fL -o`, check `(Get-Item <file>).Length` and `(Get-FileHash -Algorithm SHA256 <file>).Hash.ToLower()` against the pin and `SHA256SUMS.txt`, and check `Get-AuthenticodeSignature <file>` (Status `Valid`, a Learning Machines subject). Give the person a clickable link to the MSI; they double-click it. Where it installs is not documented for the pinned release: after the install, run the search above to find the app. **Any Windows prompt during the install, a User Account Control Yes/No or a password, is the person's decision**: the agent never answers it and never asks the person to click Yes. If they would rather not, they cancel and use the portable zip.
- **The portable zip** (no installer, no admin): the same download and checks with the zip from "Pin the versions", then `Expand-Archive` into `%USERPROFILE%\.sv-edits\apps\<app>-<version>\`. The app is `<app>.exe` in the folder `<app>-<version>-windows-<arch>-portable\`; check its signature the same way. This downloads the same zip the helper fetched for the command-line tool (65 MB for photo on x64) a second time, because the helper keeps only the `-cli.exe` tool and deleted the rest unrun: say so in the question. This copy is separate from the helper's, and the helper never runs it.

If SmartScreen, Smart App Control or a password prompt appears, stop and let the person decide; the agent never clicks past it. Then File, Open the PSD or .ai the agent made. To remove: Settings, Apps for the MSI, or delete the folder for the portable copy.

### After a change by hand

The app saves under a new name (File, Save As), never over the agent's file. Then the person tells the agent the new file's name, and it is treated like any file they bring (way 2): `keep`, `info`, `look`, and the font check before any type edit. Prompting carries on from there.

## Codex and other agents

This skill is plain markdown in one file. An agent without skill support can be told:

```text
Read https://raw.githubusercontent.com/sva-admin/sv-edits/v1.0.0/SKILL.md and follow it for everything we do in this session. Then get this computer ready for it.
```

The URL names the release tag `v1.0.0`, never `main`: `main` moves, and the helper pins live inside this file, so they cannot catch a file that changed between two fetches. Pre-flight step 2 saves the file from this same URL. A new release gets a new tag, and this line and step 2 change together. Nothing else needs to be fetched: the helpers are inside this file. Notes for Codex:

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

Two scripts, SV Academy's own, MIT. Pre-flight extracts the one for this OS from this file (never retyped), checks its sha256 against the pin in step 2, then runs it. Line 1 is the stamp; the pin table sits at the top. Exit codes: 0 all checks passed, 1 a check failed, 2 a usage error. The last output of every verb is a PASS/FAIL table.

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
# sv-edits helper v1: photo 0.3.0, vector 0.5.0, film 0.2.1
# SV Edits helper for macOS (Linux is not pinned). SV Academy's own script, MIT.
# The agent writes this file from SKILL.md to ~/.sv-edits/edits.sh and runs it with bash:
#   bash ~/.sv-edits/edits.sh <verb> [arguments]
# Verbs: doctor, install, wire, photo, vector, film, keep, check, name, look, frames, where.
# Exit codes: 0 all checks passed, 1 a check failed, 2 usage error.
# Needs only what macOS ships: bash 3.2, curl, shasum, ditto, codesign, spctl, sips.
# Installs command-line tools only: never an app, a DMG or an installer. No sudo, no PATH or
# profile edits, no quarantine or Gatekeeper changes, ever.

STAMP='sv-edits helper v1: photo 0.3.0, vector 0.5.0, film 0.2.1'
TEAM_ID='DJ6XS33FX8'
# Home: the folder this script sits in (~/.sv-edits), unless SV_EDITS_HOME names another one.
SVE_HOME="${SV_EDITS_HOME:-$(cd "$(dirname "$0")" && pwd)}"
SELF="$SVE_HOME/edits.sh"
PREV="$SVE_HOME/previews"
PROJ="$SVE_HOME/projects"
KEPT="$SVE_HOME/kept.sha256"

# ---- pin table (macOS universal CLI zips; GitHub asset digests, re-read 9 Oct 2026) ----
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
    photocraft) echo 0.3.0 ;;
    vectorcraft) echo 0.5.0 ;;
    filmcraft) echo 0.2.1 ;;
  esac
}
pin_of() {
  case "$1" in
    photocraft) echo "photocraft-cli-0.3.0-macos-universal.zip 55961526 29346d31cc054622b89f510addee70f598b65a1758f1a08010f47e18249346db" ;;
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
# sv-edits helper v1: photo 0.3.0, vector 0.5.0, film 0.2.1
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
$Stamp = 'sv-edits helper v1: photo 0.3.0, vector 0.5.0, film 0.2.1'
# Home: the folder this script sits in, unless SV_EDITS_HOME names another one.
$SveHome = if ($env:SV_EDITS_HOME) { $env:SV_EDITS_HOME } else { $PSScriptRoot }
$Self = Join-Path $SveHome 'edits.ps1'
$Prev = Join-Path $SveHome 'previews'
$Proj = Join-Path $SveHome 'projects'
$Kept = Join-Path $SveHome 'kept.sha256'
try { [Console]::OutputEncoding = New-Object System.Text.UTF8Encoding $false } catch { }
$Utf8 = New-Object System.Text.UTF8Encoding $false

# ---- pin table (Windows portable zips; GitHub asset digests, re-read 9 Oct 2026) ----
# Each zip holds one folder <app>-<version>-windows-<arch>-portable\ with <app>.exe (the desktop
# app, never kept) and <app>-cli.exe (the command-line tool), per packaging/windows/package.ps1.
$Apps = @{ photo = 'photocraft'; vector = 'vectorcraft'; film = 'filmcraft' }
$Vers = @{ photocraft = '0.3.0'; vectorcraft = '0.5.0'; filmcraft = '0.2.1' }
$Pins = @{
  'photocraft:X64'    = @('photocraft-0.3.0-windows-x64-portable.zip', 65251740, '9997f7df3a6f0717b46bb0f503b3d920c3beedeb9e9c4e1ff2ef3b8671acd47e')
  'photocraft:Arm64'  = @('photocraft-0.3.0-windows-arm64-portable.zip', 62702095, '48778db8360577807f9ff875c80dfec007ba5d07a848d1b72af4653cd20ad28e')
  'photocraft:X86'    = @('photocraft-0.3.0-windows-x86-portable.zip', 63078284, '8a4e8aabb7537806729bba4dad53518c3b9c43d22fdb0e65a9bab5db7e9b9a75')
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
