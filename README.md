# Mattermost Exporter

This unpacked browser extension injects an `export` button into Mattermost pages Simply go to Mattermost and click the export button to get started. The exported data can be conveniently viewed offline, and be imported to Matrix using the matrix-mattermost-importer extension (https://github.com/highIander/matrix-mattermost-importer).

**Plese note the Standalone Viewer note at the end of this document!**

License: None (use as you like). No liability is taken by the author!

## Features

- Adds an `export` button inside Mattermost at the lower-left corner.
- Export dialog with options for:
  - including images,
  - including other files,
  - setting a maximum file size,
  - start and end date range,
  - include direct messages/group messages.
- Individual Team and channel as well as direct message and group message selection before export.
- Progress bar and cancel button during export.
- Static export folder with core structure:
  - `index.html`,
  - `manifest.json`,
  - `users.json`,
  - `data/channels/<channel-id>/posts-0000.json`,
  - `assets/files/<file-id>.<ext>`.
- Additional additive metadata/files:
  - `emojis.json` for referenced custom emoji,
  - `assets/emojis/<emoji-id>.<ext>` for custom emoji images,
  - `post_index.json` for local post/message links.
- Unicode emoji are preserved directly in the exported UTF-8 message text.
- Custom Mattermost emoji written as `:emoji_name:` are resolved and exported when available.
- Mattermost channel mentions like `~channel-name` are rendered as local links inside the static export.
- Mattermost message permalinks such as `/team/pl/<post-id>` are rewritten to local export links when the referenced post is part of the export.
- Generated `index.html` works from a webserver or locally (you have to manually select the export folder to grant file access to the browser).
- Optional generated `standalone.html` embeds the exported JSON and downloaded assets into one file, so it can be opened without selecting a folder. The export dialog warns that this file can become extremely large, so only use for small exports!
- Direct-message and group-message display names prefer Mattermost nicknames, then full names, then usernames, while original channel fields stay available.

## Installation

0. Download all files from this repository into a directory on your computer, e.g. to [downloads/mattermost-exporter]. You can download a zip file and extract it on your computer from the latest release here: https://github.com/HighIander/mattermost-exporter/releases


### Chrome/Edge

1. Open `chrome://extensions/` or `edge://extensions/`.
2. Enable developer mode.
3. Click “Load unpacked”.
4. Select the extension download folder , e.g. [downloads/mattermost-exporter/]
5. Open Mattermost in the browser.
6. Click the lower-left `export` button.

### Firefox [not tested!]
1. Open `about:debugging#/runtime/this-firefox`
2. Click "Load Temporary Add-on"
3. Select `manifest.json` from the download folder, e.g. [downloads/mattermost-exporter/manifest.json]

## Notes

The extension uses your active Mattermost browser session. It does not need a Mattermost access token or admin permissions. It can only export data that your Mattermost account can read.

For writing the export folder, the browser must support the File System Access API. This usually means Chrome, Edge, or Firefox on HTTPS/localhost.


## Standalone viewer note

`standalone.html` is convenient but can become very large because it embeds JSON chunks and exported image/file data directly. For very large exports, you should NOT enable it and leave the checkboc unchecked! In that case, all data can be viewed later simply by opening `index.html` in the export directory. After opening index.html, select the export directory (usually the same directory where you just opened the index.html). To avoid this additional step, you can simply copy the export directory to a (local) webserver. One simple local option is XAMPP / Apache Friends: https://www.apachefriends.org/index.html

When `index.html` is opened directly from disk, browsers often block automatic reads of neighboring files such as `manifest.json`, `users.json`, `data/`, and `assets/`. The viewer therefore offers two folder-selection mechanisms:

1. **Select export folder** uses the browser File System Access API. This is the preferred option in Chrome/Edge. Select the folder that directly contains `manifest.json`.
2. **Select folder fallback** uses a directory-upload input. Use this when the first button is unavailable; select the same complete export folder.

`standalone.html` avoids folder selection but embeds exported JSON and assets directly, so it can become very large and is mainly intended for small or medium exports.
