---
layout: post
title:  "Jellyfin Stable Windows Archives"
categories: netdevops
tags: Network Engineer
date: 2023-3-21
---

# Jellyfin Stable Windows Archives

Welcome to the Jellyfin Stable Windows archives page. To install the latest stable version of Jellyfin, download the installer `.exe` below and run it; for portable installs, download the relevant `.zip` file(s) below, then use a ZIP utility to extract the file(s) and launch `jellyfin.exe`.

The available files are organized into the following folders:

| Directory  | Description                                                  |
| ---------- | ------------------------------------------------------------ |
| installer/ | The self-installing executable. You probably want this to install Jellyfin easily. |
| combined/  | The combined archives, including both the server and WebUI. You probably want this for portable Jellyfin installs. |
| server/    | The Jellyfin server archive. This provides the core Jellyfin server in a standalone, WebUI-less format. |
| web/       | The Jellyfin web client archives. Just the WebUI, requires a separate web server to run. |
| tray/      | The Jellyfin Windows tray icon archive.                      |
| ffmpeg/    | Jellyfin's custom FFMpeg, which is required for tonemapping and advanced features. This is already included in the Installer and Combined packages. |

SHA-256 summary hashes are provided for each file to verify their integrity.

Older builds can be found [here](https://repo.jellyfin.org/releases/server/windows/versions/stable), organized by the directories above. To go back to the main server releases page, click [here](https://repo.jellyfin.org/releases/server/).

## Current Stable builds

| Name                                                         | Type                  | Size      | Last Modified                   |
| ------------------------------------------------------------ | --------------------- | --------- | ------------------------------- |
| [installer/jellyfin_10.8.9_windows-x64.exe](https://repo.jellyfin.org/releases/server/windows/stable/installer/jellyfin_10.8.9_windows-x64.exe) | application/x-dosexec | 126200252 | Sun, 22 Jan 2023 15:22:12 -0500 |
| [installer/jellyfin_10.8.9_windows-x64.exe.sha256sum](https://repo.jellyfin.org/releases/server/windows/stable/installer/jellyfin_10.8.9_windows-x64.exe.sha256sum) | text/plain            | 98        | Sun, 22 Jan 2023 15:41:28 -0500 |
| [combined/jellyfin_10.8.9.zip](https://repo.jellyfin.org/releases/server/windows/stable/combined/jellyfin_10.8.9.zip) | application/zip       | 130899999 | Sun, 22 Jan 2023 14:41:07 -0500 |
| [combined/jellyfin_10.8.9.zip.sha256sum](https://repo.jellyfin.org/releases/server/windows/stable/combined/jellyfin_10.8.9.zip.sha256sum) | text/plain            | 85        | Sun, 22 Jan 2023 14:41:08 -0500 |
| [server/jellyfin-server_10.8.9.portable.zip](https://repo.jellyfin.org/releases/server/windows/stable/server/jellyfin-server_10.8.9.portable.zip) | application/zip       | 86335946  | Sun, 22 Jan 2023 14:40:51 -0500 |
| [server/jellyfin-server_10.8.9.portable.zip.sha256sum](https://repo.jellyfin.org/releases/server/windows/stable/server/jellyfin-server_10.8.9.portable.zip.sha256sum) | text/plain            | 101       | Sun, 22 Jan 2023 14:40:51 -0500 |
| [web/jellyfin-web_10.8.9_portable.zip](https://repo.jellyfin.org/releases/server/windows/stable/web/jellyfin-web_10.8.9_portable.zip) | application/zip       | 44542301  | Sun, 22 Jan 2023 14:28:58 -0500 |
| [web/jellyfin-web_10.8.9_portable.zip.sha256sum](https://repo.jellyfin.org/releases/server/windows/stable/web/jellyfin-web_10.8.9_portable.zip.sha256sum) | text/plain            | 98        | Sun, 22 Jan 2023 14:28:59 -0500 |
| [ffmpeg/jellyfin-ffmpeg-portable_win64.zip](https://repo.jellyfin.org/releases/server/windows/stable/ffmpeg/jellyfin-ffmpeg-portable_win64.zip) | application/zip       | 30072175  | Sun, 19 Feb 2023 13:59:00 -0500 |
| [ffmpeg/jellyfin-ffmpeg_5.1.2-8-portable_win64.zip](https://repo.jellyfin.org/releases/server/windows/stable/ffmpeg/jellyfin-ffmpeg_5.1.2-8-portable_win64.zip) | application/zip       | 30072175  | Sun, 19 Feb 2023 13:59:00 -0500 |
| [ffmpeg/jellyfin-ffmpeg_5.1.2-8-portable_win64.zip.sha256sum](https://repo.jellyfin.org/releases/server/windows/stable/ffmpeg/jellyfin-ffmpeg_5.1.2-8-portable_win64.zip.sha256sum) | text/plain            | 109       | Sun, 19 Feb 2023 13:59:00 -0500 |
| [ffmpeg/jellyfin-ffmpeg_5.1.2-8_portable_linux64-gpl.tar.xz](https://repo.jellyfin.org/releases/server/windows/stable/ffmpeg/jellyfin-ffmpeg_5.1.2-8_portable_linux64-gpl.tar.xz) | application/x-xz      | 48462056  | Sun, 19 Feb 2023 15:50:31 -0500 |
| [ffmpeg/jellyfin-ffmpeg_5.1.2-8_portable_linux64-gpl.tar.xz.sha256sum](https://repo.jellyfin.org/releases/server/windows/stable/ffmpeg/jellyfin-ffmpeg_5.1.2-8_portable_linux64-gpl.tar.xz.sha256sum) | text/plain            | 118       | Sun, 19 Feb 2023 15:50:31 -0500 |
| [ffmpeg/jellyfin-ffmpeg_5.1.2-8_portable_linuxarm64-gpl.tar.xz](https://repo.jellyfin.org/releases/server/windows/stable/ffmpeg/jellyfin-ffmpeg_5.1.2-8_portable_linuxarm64-gpl.tar.xz) | application/x-xz      | 38813436  | Sun, 19 Feb 2023 15:50:32 -0500 |
| [ffmpeg/jellyfin-ffmpeg_5.1.2-8_portable_linuxarm64-gpl.tar.xz.sha256sum](https://repo.jellyfin.org/releases/server/windows/stable/ffmpeg/jellyfin-ffmpeg_5.1.2-8_portable_linuxarm64-gpl.tar.xz.sha256sum) | text/plain            | 121       | Sun, 19 Feb 2023 15:50:32 -0500 |