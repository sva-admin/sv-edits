<p align="center"><a href="https://loop.sv-academy.org"><img src="https://raw.githubusercontent.com/sva-admin/claude-skills/main/assets/sv-academy-512.png" width="96" alt="Silicon Valley Academy"/></a></p>

# SV Edits

**Real design files, by prompting.** Layered PSD files, .ai files with their SVG and PDF, and MP4 clips: the formats designers and print shops use, made and edited by prompting in Claude Code or Codex. Free, on your own computer, and you never need to open an editing app. Your files stay on your computer: no file goes to an editing service or account, and your AI provider sees only what the agent reads and the previews, as with any chat. Every result is shown back to you in the chat and saved as a new file next to the original, which never changes. SV Academy's own skill, in **one file**: [`SKILL.md`](SKILL.md) holds the whole method, including the helper scripts, so Claude Code and Codex get exactly the same thing.

The tools are three free, open source command-line editors. The skill installs them once, pinned to tested versions and checked against this file, and drives them for you. No Adobe subscription, no account, no editing service.

## Install

Paste this into Claude Code, in the session where you built your product:

```text
Install the skill from https://github.com/sva-admin/sv-edits, then get this computer ready for it.
```

```text
ติดตั้งสกิลจาก https://github.com/sva-admin/sv-edits แล้วเตรียมเครื่องนี้ให้พร้อมใช้งาน
```

Codex, or any agent without skills:

```text
Read https://raw.githubusercontent.com/sva-admin/sv-edits/v1.0.0/SKILL.md and follow it for everything we do in this session. Then get this computer ready for it.
```

The link names the release (`v1.0.0`), so every laptop in a class reads the same file. To keep it for the next session in Codex, save that file as `~/.codex/skills/sv-edits/SKILL.md`, or paste the line again.

The agent checks your computer, asks you once (about 121 MB of tools to download, and in Claude Code two editing servers for this project: the photo server can only reach your project folder; the vector server can reach any path, and SV Edits only gives it paths inside your project), installs, and shows you a PASS table. In Codex the servers are optional and asked about separately, because Codex adds them for every session on the computer; everything works without them. That is the only setup. From then on you just say what you want.

## Three ways in

| You have | Way | What the agent does |
| --- | --- | --- |
| The session where you built your product (**Easiest**) | 1. From your product | Reads your logo, colours, fonts, name and promise from your code and says them back. Then makes a layered PSD post where every line is live type, and a brand .ai with SVG and PDF. |
| A PSD, AI, SVG, PDF or photo | 2. From a file you have | Opens it and tells you what is inside (layers, live text, fonts). Then edits it by your words: new words, colours or sizes, a copy for print or for LINE. New files next to it; the original is never touched. |
| Phone or camera footage, or a reel made earlier | 3. From a clip you already have | Shows you frames and a cut list in seconds and waits for your yes. Then trims, cuts together, makes a vertical version, grabs frames (they become PSD covers) and exports MP4. |

From then on the three are the same: you look, you fix it in words, and the original never changes.

**SV Edits or SV Motion?** [SV Motion](https://github.com/sva-admin/sv-motion) makes new video from words: launch reels, motion graphics, Thai titles and captions. SV Edits edits what you already have. Any words on video go to SV Motion; a frame or a trimmed clip from SV Edits goes into an SV Motion video for its titles.

## Then, from the same files

| Preset | Prompt |
| --- | --- |
| A layered post from my product | `Use SV Edits to read my product and make an Instagram post as a layered PSD, every line live type, in my brand. Say my brand back first.` |
| A brand .ai | `Use SV Edits to make a brand board as .ai, SVG and PDF with my logo, name, promise and colours.` |
| Every size from one master | `Use SV Edits to make my post in feed, square, story and link preview sizes, each as its own PSD and JPEG.` |
| Change the words in a PSD | `Use SV Edits to open menu.psd, tell me what is inside, then change the price line to "ลด 20%" and save a new PSD and a JPEG for LINE.` |
| A Thai headline on a photo | `Use SV Edits to put my headline in Thai and English on this photo, as a PSD with live type and a JPEG.` |
| Every layer of a PSD as PNG | `Use SV Edits to export every layer of this PSD as its own PNG, with a list of the layers.` |
| A logo in every format a printer asks for | `Use SV Edits to make my logo into .ai, PDF, EPS, SVG and PNG at 1x and 2x, with a note on which file to send.` |
| A white or one-colour logo | `Use SV Edits to make a white version and a one-colour version of my logo.` |
| A vertical clip | `Use SV Edits to make a 15 second vertical clip for Reels from this video. Show me the frames and the cut list first.` |
| A cover from my video | `Use SV Edits to grab the best frame from this clip and make it a 1280x720 cover PSD with my headline in Thai.` |

## Mockups and vectors from words

Real design jobs from your own screen, logo, photo and words. Each comes back as a preview in the chat and a new file next to your originals.

| Preset | Prompt |
| --- | --- |
| Your app screen in a phone mockup | `Use SV Edits to put my app screen app-screen-1.png in a phone mockup with my logo, name and tagline, as a layered PSD.` |
| Swap the screen in one call | `Use SV Edits to swap the screen in this mockup for my new screen app-screen-2.png.` |
| Your logo or post on a product shot | `Use SV Edits to put my logo and my post on this photo of my product.` |
| A logo lockup | `Use SV Edits to make a logo lockup with my logo, name and tagline, as .ai, SVG, PDF and PNG.` |
| An icon set | `Use SV Edits to make 3 icons for my 3 features as SVG, in my brand colour.` |
| A die-cut sticker with a cut line | `Use SV Edits to turn my logo and tagline into an 80 mm round die-cut sticker with a cut line for the printer.` |

ในภาษาไทยก็สั่งได้เหมือนกัน เช่น `เอาหน้าจอแอปใส่ในม็อกอัปมือถือ พร้อมโลโก้ ชื่อ และสโลแกนของแบรนด์ ขอเป็นไฟล์ PSD แยกเลเยอร์` หรือ `ทำโลโก้กับสโลแกนเป็นสติกเกอร์ไดคัททรงกลม 80 มม. พร้อมเส้นไดคัทสำหรับโรงพิมพ์`

The phone is drawn from shapes, not taken from a mockup pack, and your screenshot sits in it as a layer you can swap. A product shot gets a flat placement on a surface that faces the camera. The sticker comes as a print-ready PDF with the cut line on its own layer in the CutContour spot colour, 2 mm bleed and outlined text. The icons are the agent's drawing of your words: it shows them to you for approval first.

## What it does

- **Prompt only.** You never need to open an editing app. The agent installs command-line tools only (no app, no installer, no admin password) and never opens a window. If you want to look at a file in an app, see below.
- **Real formats.** A PSD keeps its layers and live type; the .ai is a PDF-compatible file with SVG and PDF beside it; clips come out as H.264 MP4 (or a ProRes master on request).
- **The original never changes, and you can check.** Each original is fingerprinted before the edit and checked after; every result gets a new name next to it.
- **You see every result.** The agent renders a preview and shows it in the chat before it hands anything over.
- **Your brand, real assets.** Colours, fonts, words and logo come from your own code or your own file. No invented prices or reviews; any line the agent writes or translates is flagged for you to approve.
- **Thai done properly.** Tested Thai faces, one line per layer, no letter spacing, and a close look at the tone marks before you share.
- **Fonts checked first.** Before it touches any type, the agent checks that every font in the file is on your computer. A file that came with a `fonts/` folder (an SV Edits kit has one) is edited with those fonts, without installing them, so your brand font stays. If one is still missing, it says so and offers a fix by prompt: switch to an installed face, or, after your yes, install the free font.
- **Never stretched.** Nothing is enlarged past its own pixels. A vertical version of a 1080p clip comes out at 608x1080, its native size, unless you ask for 1080x1920 knowing it will be softer.
- **Fix it in words.** Say what to change; the agent changes only that and shows you again.
- **Pinned and checked.** Exact versions, each download checked by SHA256 against this file and the release, and by its signature, before it runs.

## Want to see it in the app?

อยากเปิดดูในโปรแกรม

Optional. Nothing in SV Edits needs an app. To just look at a PSD or .ai on a Mac, select it in Finder and press Space (Quick Look), or open it in Preview: nothing to install. To see the layers or move something by hand, there are free open source desktop apps of the same tools: PhotoCraft 0.3.0 for PSD files, VectorCraft 0.5.0 for .ai, SVG and PDF. Ask the agent, for example `Install PhotoCraft so I can look at my mockup PSD.` It first checks whether the app is already on your computer, installs one only when you ask, and never opens it unless you ask.

- **Mac:** the agent downloads the pinned DMG from the storytold release on GitHub into your Downloads folder and checks its checksum against the pin in SKILL.md and the release's `SHA256SUMS.txt`. You double-click it and drag the app to Applications (if macOS asks for a password, cancel and drag it to the Applications folder in your home folder instead), then open it (it is notarized) and File, Open the PSD or .ai.
- **Windows:** the pinned `.msi` installer for your computer, or the portable zip with no installer, checked the same way. Any Windows prompt during the install is your decision; if you would rather not, cancel and use the portable zip. Then File, Open. The Windows path is untested so far.

Save any change by hand under a new name, then tell the agent the file's name: it picks up from there by prompt.

## Requirements

macOS 11 or newer (Apple silicon or Intel), or Windows 10 or 11 (x64, Arm64 or x86). Claude Code or Codex. For the photo and vector tools: on a Mac, a 121 MB download and about 240 MB on disk; on Windows x64, a 143 MB download and about 130 MB on disk. The footage tool adds a 25 to 34 MB download (about 62 MB on disk on a Mac) the first time you edit a clip. An internet connection for that first download. Nothing else: no Homebrew, Node, Python or ffmpeg.

On Windows, Claude Code runs the helper from Git Bash and Codex from PowerShell; both use the same PowerShell helper. Run the agent in Windows itself, not inside WSL. The Windows path is untested so far, and the footage tool has no Arm64 build at the pinned version, so it runs under emulation there.

Before a class, at home, so the room's Wi-Fi does not have to carry it: run the install prompt once.

How long it takes, measured on an M3 Max with the pinned tools: a layered PSD post with a logo and three Thai and English lines, 0.1 s; a brand board as .ai, SVG, PDF and PNG, 0.1 s; frames and a contact sheet from a 19 s clip, about 2 s; a 6 s cut exported to MP4 at -14 LUFS, about 4 s. An 8 GB laptop is slower: one job at a time.

## License

MIT, for the SV Academy text and scripts in this repository, written in our own words. See [LICENSE](LICENSE).

The editing tools are [PhotoCraft](https://github.com/storytold/photocraft), [VectorCraft](https://github.com/storytold/vectorcraft) and [FilmCraft](https://github.com/storytold/filmcraft) by storytold (Apache-2.0 or MIT), downloaded from their own releases and never redistributed here.

## More from Silicon Valley Academy

- All of our published skills: [github.com/sva-admin/claude-skills](https://github.com/sva-admin/claude-skills)
- Courses and free lessons: [sv-academy.org](https://sv-academy.org)
