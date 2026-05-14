# Mattermost Static Exporter Extension

This unpacked browser extension injects an `export` button into Mattermost pages.

## Features

- Lower-left `export` button inside Mattermost.
- Export dialog with options for:
  - images,
  - other files,
  - maximum file size,
  - start and end date range,
  - direct messages/group messages.
- Team and channel selection before export.
- Progress bar and cancel button during export.
- Static export folder with the same core structure as before:
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
- Generated `index.html` works from a webserver or locally after selecting the export folder.
- Optional generated `standalone.html` embeds the exported JSON and downloaded assets into one file, so it can be opened by double-clicking without selecting a folder. The export dialog warns that this file can become extremely large.
- Direct-message and group-message display names prefer Mattermost nicknames, then full names, then usernames, while original channel fields stay available.

## Installation in Chrome/Edge

1. Open `chrome://extensions/` or `edge://extensions/`.
2. Enable developer mode.
3. Click “Load unpacked”.
4. Select this extension folder.
5. Open Mattermost in the browser.
6. Click the lower-left `export` button.

## Compatibility note

The export keeps the previous core data layout. Scripts that read `manifest.json`, `users.json`, `data/channels/...`, and `assets/files/...` should continue to work. The emoji, post-link, standalone viewer, and direct-message friendly-name metadata are additive.

## Notes

The extension uses your active Mattermost browser session. It does not need a Mattermost access token or admin permissions. It can only export data that your Mattermost account can read.

For writing the export folder, the browser must support the File System Access API. This usually means Chrome or Edge on HTTPS/localhost.


## Standalone viewer note

`standalone.html` is convenient but can become very large because it embeds JSON chunks and exported image/file data directly. For very large exports, `index.html` plus the folder structure is still the more robust viewer.


## Viewer usage guidance

For larger exports, prefer the folder-based `index.html` and copy the complete export folder to a webserver destination. One simple local option is XAMPP / Apache Friends: https://www.apachefriends.org/index.html

When `index.html` is opened directly from disk, browsers often block automatic reads of neighboring files such as `manifest.json`, `users.json`, `data/`, and `assets/`. The viewer therefore offers two folder-selection mechanisms:

1. **Select export folder** uses the browser File System Access API. This is the preferred option in Chrome/Edge. Select the folder that directly contains `manifest.json`.
2. **Select folder fallback** uses a directory-upload input. Use this when the first button is unavailable; select the same complete export folder.

`standalone.html` avoids folder selection but embeds exported JSON and assets directly, so it can become very large and is mainly intended for small or medium exports.
