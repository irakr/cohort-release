# Cohort - downloads

Cohort is a desktop app for getting a colleague into your problem fast. The
owner opens an assist with a short brief, shares a bounded slice of their
machine - a directory, a file, a window - and responders browse open assists and
help. Every share is an explicit grant that expires or ends when the assist
closes, mirroring is view-only, and closing an assist writes a resolution
record.

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

Cohort needs a hub to talk to - one server both machines can reach. Unpack the
hub tarball on that machine and run it:

    tar -xzf cohort-hub_<version>_linux-x86_64.tar.gz
    cd cohort-hub_<version>_linux-x86_64
    COHORT_BIND=0.0.0.0:7400 ./cohort-hub

`COHORT_BIND=0.0.0.0:7400` is what lets other machines reach it; the default
binds to localhost only. The tarball's `README.txt` lists the rest of the
environment variables.

Then open the app. It asks for the hub URL (`http://<hub-host>:7400`) and a
name to register. The hub ships with no accounts and no data.

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
