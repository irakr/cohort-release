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

- **A space** is a team or group: its people, and its rooms. You join one
  by an invite link, and nobody outside it sees anything in it.
- **A room** is one piece of work inside a space: a short brief, the people
  in it, their artifacts, and a conversation. You invite people, or let them
  ask to join.
- **Closing a room writes a record** of what was found and what was done,
  and credits the people who contributed.

Every app talks to Cohort's hub, the server that keeps the spaces, rooms and
conversations and passes files and windows through without storing them. A
team, a company, or a vendor for its customers can also run its own (see the
end of this page). An optional AI assistant helps draft a new room's brief,
on your machine, with the model provider you choose.

## About this repository

This repository is the download channel. Development happens elsewhere; nothing
here is source code.

## Download

**[Latest release](../../releases/latest)**

| Platform | File | Notes |
|----------|------|-------|
| macOS | `Cohort_<version>_universal.dmg` | Intel and Apple Silicon, macOS 11 or later. |
| macOS | `Cohort_<version>_universal.zip` | The same app, zipped, for scripted installs. |
| Windows | `Cohort_<version>_x64-setup.exe` | Windows 10 or 11. Installs for you alone, no administrator prompt. |
| Linux | `Cohort_<version>_amd64.deb` | Ubuntu 22.04 or later, and Debian. |
| Hub | `cohort-hub_<version>_linux-x86_64.tar.gz` | Only if you run your own server; see the end of this page. |
| All | `SHA256SUMS` | Checksums for every file above. |

## Install

**macOS.** Open the `.dmg` and drag Cohort to Applications, then open it
from there. The first time you share a window, macOS asks for Screen
Recording: allow Cohort in System Settings -> Privacy & Security -> Screen
Recording, then quit and reopen Cohort. macOS only applies that permission
to a fresh start.

**Windows.** Run the setup `.exe`. The installer is not signed yet, so
Windows shows "Windows protected your PC": click **More info**, then **Run
anyway**. It installs for you alone and fetches Microsoft's WebView2 if the
machine does not have it, so stay online the first time. On a Windows 11
machine with Smart App Control turned on, Windows refuses unsigned
installers altogether; Cohort cannot be installed there until it is signed.

**Linux.**

    sudo apt install ./Cohort_<version>_amd64.deb

Then open Cohort from your applications. Window sharing works on an X11
session. On Wayland (Ubuntu's default) only some windows can be shared; to
share any window, choose "Ubuntu on Xorg" from the gear on the login screen.

## First run

1. Open Cohort and register: an email, a username and a password. That is
   your account on every machine you use.
2. You land on your spaces. A space is your team or group: everything in
   Cohort happens inside one. Create a space, or paste an invite link
   someone sent you into **Join with a link**.
3. Inside a space, open a room for a piece of work and bring people in, or
   open one someone else has opened. Share from the room: a folder, a
   window, an AI agent session.

To invite someone, open the space's menu (the space's name at the top
left) and choose **Copy invite link**, then send them the link.

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

## Running your own hub

The app talks to Cohort's hub out of the box. To run your own - for a team,
a company, or a vendor and its customers - unpack the hub tarball on a Linux
machine everyone can reach:

    tar -xzf cohort-hub_<version>_linux-x86_64.tar.gz
    cd cohort-hub_<version>_linux-x86_64
    sudo ./setup-and-host-cohort-hub.sh

It installs what it needs, runs the hub as a service, publishes it over
HTTPS with Tailscale Funnel (no router changes) or Caddy, and prints the
address. `setting-up-and-hosting-a-cohort-hub.md`, beside it, explains every
step. Then put that address in **Hub URL** behind the gear in the app.

