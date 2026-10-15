<div align="center">

# 📚 PS5 Ebook Reader
### Native PS5 Homebrew — by YAHBALLON

**EPUB · PDF · CBR · CBZ** · DualSense navigation · USB & internal storage · Comics & Covers

[![Version](https://img.shields.io/badge/version-v0.2.6-daa04c)](https://github.com/Yahballon/PS5_Ebook_Reader/releases/tag/ebook-reader-v0.2.6)
[![Platform](https://img.shields.io/badge/platform-PlayStation%205%20Homebrew-5c3418)](#requirements)
[![License](https://img.shields.io/badge/license-GPL--3.0-blue)](LICENSE)

[**⬇️ Download latest PS5 ZIP**](https://github.com/Yahballon/PS5_Ebook_Reader/releases/download/ebook-reader-v0.2.6/PPSA99741.zip) · [**Releases**](https://github.com/Yahballon/PS5_Ebook_Reader/releases) · [**Report an issue**](https://github.com/Yahballon/PS5_Ebook_Reader/issues)

<img src="assets/ebook-reader-preview.webp" width="1000" alt="PS5 Ebook Reader home screen showing a wooden bookshelf with cover thumbnails and genre tabs">

*The Ebook Reader bookshelf on PS5 (v0.2.6). Screenshot supplied by the developer.*

</div>

## Overview

**PS5 Ebook Reader** is a native C/C++ homebrew application for reading books and image-based comics on a compatible homebrew-enabled PlayStation 5. It uses a TV-friendly 1920 × 1080 console interface, real OpenGL rendering and full DualSense navigation. No web browser or separate PC-based reader is needed to read books already accessible to the application.

| Format | Support | Notes |
| :--- | :---: | :--- |
| **EPUB** | ✅ | EPUB package parsing, readable text, inline images, cover artwork, adjustable single/double-page layout |
| **PDF** | ⚠️ | Extractable text preview; **not** a full graphical PDF renderer |
| **CBR** | ✅ | Comic images inside supported RAR4/RAR5 archives; image page navigation & zoom |
| **CBZ** | ✅ | Comic images in ZIP archives; cover from an image page & full-screen reading |

## Features

- **Visual bookshelf** with genre/category filters: All, Comics, Fiction, Novels and Favorites.
- **Real cover art** for EPUB and supported comics, with asynchronous extraction and saved thumbnail cache.
- **Native comic reader** with JPEG/PNG/GIF image decoding where supported.
- **Single / double-page toggle** for EPUB/PDF text content, with full-width pages.
- **Zoom and pan** using the DualSense; changing pages resets scrolling to the beginning of the new page.
- **Reading progress** remembered across app sessions; favorites and saved library state.
- **USB book discovery** including nested folders, using an existing compatible `elfldr` on port **9021** and a bundled read-only helper on local port **9176**.
- **Progress bars** with actual bytes and percentages while a book is copied from USB; background loading keeps the UI responsive.
- **Resumable large-file transfers** and bounded automatic cleanup of disposable cache files.
- **Exit confirmation**: Circle in My Library displays Yes / No, preventing accidental exits.
- **Debug logging** and user-owned folders in `/data/EbookReader/`.

## Controls

### My Library

| DualSense button | Action |
| :--- | :--- |
| **D-Pad** | Select a book |
| **✕ Cross** | Open selected book |
| **◯ Circle** | Show Exit Ebook Reader? confirmation |
| **□ Square** | Toggle Favorite |
| **△ Triangle** | Open Storage / Import Books |
| **L1 / R1** | Previous / next collection |
| **R2** | Toggle bookshelf / list view |
| **OPTIONS** | Rescan library and USB catalog |
| **L3** | Request delete of an imported book (requires confirmation) |

### Reading a book or comic

| DualSense button | Action |
| :--- | :--- |
| **L1 / R1** | Previous / next page |
| **✕ Cross** | Next page |
| **D-Pad Left / Right** | Turn pages at default zoom; pan when zoomed |
| **D-Pad Up / Down** | Pan when zoomed |
| **L2 / R2** | Zoom out / in (up to 400%) |
| **□ Square** | Single / double-page toggle in EPUB/PDF; jump to last page in comics |
| **△ Triangle** | Jump to first page |
| **L3 / OPTIONS** | Reset zoom and pan |
| **◯ Circle** | Return to library |

### Storage / import and Exit dialog

| Screen | Button | Action |
| :--- | :--- | :--- |
| **Storage** | D-Pad Up / Down | Select available local import |
| **Storage** | ✕ Cross | Import selected local file |
| **Storage** | △ Triangle | Refresh storage catalog |
| **Storage** | ◯ Circle | Back to My Library |
| **Exit prompt** | ✕ Cross | **Yes** — save state and close |
| **Exit prompt** | ◯ Circle | **No** — return to library |
| **Loading** | ◯ Circle | Request cancellation of an ongoing USB transfer |

## Requirements

- A **homebrew-capable PS5** with a compatible loader (e.g. TESTBUILD / ShadowMount setup).
- An installation method capable of deploying and registering a homebrew title folder. **Title ID: `PPSA99741`**.
- For USB scanning, an already-running compatible **ELF loader at port 9021**. The bundled `bookscan.elf` helper is launched by the app and exposes localhost port 9176. USB storage is optional if you use internal storage.
- Sufficient writable storage for caching complete CBR/CBZ archives when importing from USB.

This app is **not supported on an unmodified retail PS5**.

## Installation

1. Download **[PPSA99741.zip](https://github.com/Yahballon/PS5_Ebook_Reader/releases/download/ebook-reader-v0.2.6/PPSA99741.zip)** from the latest verified GitHub Release.
2. Extract the ZIP, keeping the top-level `PPSA99741` folder intact.
3. Copy the complete folder into `/data/homebrew/` on the PS5, resulting in `/data/homebrew/PPSA99741/eboot.bin`, `sce_module/`, `sce_sys/`, assets and `bookscan.elf`.
4. Register/launch the homebrew through your compatible PS5 loader, or install through your TESTBUILDS workflow if configured.
5. Add books using one of the locations below and restart or press **OPTIONS** to rescan.

**Keep the full ZIP contents; an `eboot.bin` alone is not a complete installation.** Updating over an existing version should preserve book folders and reading progress.

## Where to put books

| Purpose | Location |
| :--- | :--- |
| Internal books (recommended) | `/data/EbookReader/Books/` |
| USB drive books | `/mnt/usb0/Books/` |
| Large USB book cache (automatic) | `/data/EbookReader/Cache/` |
| Saved comic cover thumbnails | `/data/EbookReader/Covers/` |
| Diagnostic log | `/data/EbookReader/ebook-reader.log` |

EPUB, PDF, CBR and CBZ files can be placed in the Books folder and nested subdirectories. USB books are catalogued through the helper and copied into a cache as necessary. **Do not put the only copy of your books in the cache**: its contents are intentionally disposable.

## Building from source

The source and host regression tests are included in [`homebrew/ebook-reader/`](homebrew/ebook-reader/). The dedicated workflow [`build-ebook-reader.yml`](.github/workflows/build-ebook-reader.yml) runs unit/integration tests, retrieves the pinned PS5 development template and dependencies, builds `bookscan.elf` plus the native `eboot.bin`, validates the package, and publishes a versioned ZIP for eligible main-branch builds.

The native application uses [PS5-DevBridge](https://github.com/Yahballon/PS5-DevBridge), the [ps5-homebrew-ui framework](https://github.com/blackbearreloaded/ps5-homebrew-ui), zlib, libarchive and an stb_image decoder. Toolchain/bootstrap dependencies are fetched during CI. See the workflow for the exact build commands and pinned versions.

The published **v0.2.6** package is the same verified ZIP as the original application release at [tmp_lap](https://github.com/Yahballon/tmp_lap/releases/tag/ebook-reader-v0.2.6); its SHA-256 is `25c6158761490fcb632379a1dd1808a9e0b3b9df5a680c4384af9e3010446aba`.

## Limitations / known issues

- PDF support is for extractable text and does not reproduce every graphic element or scanned page.
- Password-protected and multipart RARs are unsupported; some unusual comic page encodings may not display.
- The first uncached USB comic cover can require downloading the complete archive and adequate cache space.
- A successful GitHub Actions build is **not** a substitute for physical-console testing; compatibility depends on firmware, homebrew runtime and loader.
- Only use books you have the right to copy/read. This project does not ship third-party commercial books.

## Troubleshooting

If a book does not open, run **OPTIONS** to rescan, check your paths and verify that `elfldr` is available for USB sources. For failures, open `/data/EbookReader/ebook-reader.log` and include its last lines when submitting an issue. `USB_FETCH_OK` means the archive was copied successfully; `OPEN_OK` means it was parsed successfully. `No space left on device` means the cache filesystem is full.

## Credits & license

Created by **YAHBALLON**. Developed using homebrew community tooling and open-source components under their respective licenses. **Project source: GPL-3.0** — see [LICENSE](LICENSE). This is an independent homebrew project, not affiliated with or endorsed by Sony Interactive Entertainment.

