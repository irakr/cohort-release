# Cohort

Cohort is a shared workspace for people working together on real machines.

You open a room and bring people in. Everyone in it can share what is on
their own machine - files and folders, live windows, AI agent sessions,
access to the device itself - and everyone else can ask for what they need
to see. Each share is decided by the person it belongs to, one at a time,
and ends when they stop it or the room closes.

## What it is for

- **Team review and discussion.** Go through a change, a config or a design
  on the files themselves, with comments on the exact lines.
- **Pair programming.** Follow each other's editor or terminal live, point
  at lines, and suggest the next command.
- **Customer support.** A vendor or consultant helps an enterprise team or
  an end user, seeing only what they are shown, in part if the customer
  chooses.
- **Remote system management.** Look after a machine you are not sitting
  at: its windows, files and logs, and SSH access when it is granted.
- **Competitive programming and CTFs.** Share each contestant's window with
  the whole room, and discuss as it happens.

## Artifacts

An artifact is anything someone shares from their machine into a room: a
file or folder, a live window, an AI agent session, access to the device.
Artifacts are what make Cohort different: people work on the real thing, not
on screenshots, pasted logs or a description of it.

**Safe.**

- Every artifact belongs to the person who shared it, and every share is a
  grant they give, to the whole room or to people they name. It expires,
  ends when the room closes, and stops in one click.
- Files are sealed by default. Others see the names; the contents leave only
  when you open a file, or unmask the lines or areas you choose. Keys and
  tokens are masked automatically, and full paths never leave your machine.
- Nothing is stored on the hub. A file is sent when someone opens it and
  dropped once delivered, and every open is listed with who opened it.
- Windows are mirrored view-only: nobody can type or click into your machine
  through Cohort. Keeping a copy of a file is a grant of its own, separate
  from viewing it.

That is what makes Cohort usable where plain screen sharing is not allowed.

**Interactable.**

- Browse a shared folder and open its files as text, images or PDFs. A
  change on the other side reaches you within a second.
- Follow a window live, as it is on the other machine.
- Comment on a line, a range or a whole file, or under a window, and answer
  where the comment sits.
- Suggest a command. The holder copies it and runs it themselves; Cohort
  never runs anything.
- Ask for more in one click: a file you cannot open yet asks its holder.
- Pull any artifact out into its own window.

## How it works

- **A room** is one piece of work: a short brief, the people in it, their
  artifacts, and a conversation. You invite people, or let them ask to join.
- **Closing a room writes a record** of what was found and what was done,
  and credits the people who contributed.

Cohort runs on your own server: one machine runs the hub and everyone's
desktop app connects to it. A team, a company, or a vendor for its customers
can each run their own. The hub keeps the rooms and the conversation, and
passes files and windows through without storing them. An optional AI
assistant helps draft a new room's brief, on your machine, with the model
provider you choose.

## About this repository

This repository is the download channel. Development happens elsewhere; nothing
here is source code.

## Download

**[Latest release](../../releases/latest)**

| Platform | File | Notes |
|----------|------|-------|
| macOS | `Cohort_<version>_universal.dmg` | Intel and Apple Silicon. Drag to Applications. |
| macOS | `Cohort_<version>_universal.zip` | The same app, zipped, for scripted installs. |
| Linux | `Cohort_<version>_amd64.AppImage` | Portable, runs on most distributions. |
| Linux | `Cohort_<version>_amd64.deb` | Debian and Ubuntu. |
| Windows | `Cohort_<version>_x64-setup.exe` | Per-user installer, no administrator prompt. |
| Hub | `cohort-hub_<version>_linux-x86_64.tar.gz` | The server. One machine runs it; every app connects to it. |
| All | `SHA256SUMS` | Checksums for every file above. |

## Install

**macOS.** Open the `.dmg` and drag Cohort to Applications. If the build is not
notarized, macOS quarantines it: right-click Cohort -> Open -> Open the first
time, or run `xattr -cr /Applications/Cohort.app`. Sharing a window asks for
Screen Recording the first time you use it; macOS shows that prompt itself.

**Linux.**

    sudo apt install ./Cohort_<version>_amd64.deb

or, for the portable build:

    chmod +x Cohort_<version>_amd64.AppImage
    ./Cohort_<version>_amd64.AppImage

Sharing a window needs `wmctrl` and `imagemagick`. The `.deb` installs both. An
AppImage cannot declare dependencies, so install them yourself if you intend to
share a window:

    sudo apt install wmctrl imagemagick

**Windows.** Run the setup `.exe`. It installs for the current user only and
fetches the WebView2 runtime if the machine does not have it. Until the
installer is signed, SmartScreen warns on first run: More info -> Run anyway.

## First run

Cohort needs a hub to talk to - one server every machine can reach. Unpack the
hub tarball on that machine and run it:

    tar -xzf cohort-hub_<version>_linux-x86_64.tar.gz
    cd cohort-hub_<version>_linux-x86_64
    COHORT_BIND=0.0.0.0:7400 COHORT_NAME='Payments team' ./cohort-hub

`COHORT_BIND=0.0.0.0:7400` is what lets other machines reach it; the default
binds to localhost only. `COHORT_NAME` is what the apps call this hub. The
tarball's `README.txt` lists the rest of the environment variables.

Then open the app. It asks for the hub URL (`http://<hub-host>:7400`), then
lets you register (email, username, password) or sign in. The hub ships with
no accounts and no data.

To try it, open a room with the + button on one machine, and on another,
signed in as someone else, open that room and ask to join.

## Verify a download

    sha256sum -c SHA256SUMS        # Linux
    shasum -a 256 -c SHA256SUMS    # macOS

Run it in the directory holding the files you downloaded; it reports `OK` per
file, and skips what you did not download.

## Reporting a problem

Say which platform and which version, and attach the log for the run that went
wrong. Both the app and the hub write to `<OS config dir>/cohort/logs/`:

| OS | Path |
|----|------|
| macOS | `~/Library/Application Support/cohort/logs/` |
| Linux | `~/.config/cohort/logs/` |
| Windows | `%APPDATA%\cohort\logs\` |

`app.log` is the desktop app, `hub.log` is the server. Both are truncated on
every launch, so a log covers exactly one run - copy it aside before restarting.
