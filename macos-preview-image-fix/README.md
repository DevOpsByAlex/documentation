# Fixing Corrupted iPhone Images on macOS (CLI Guide)

## Overview

Images copied from iPhone (especially after editing) may fail to open in macOS Preview or Quick Look, while still working in browsers like Chrome. This is caused by incompatible metadata or encoding issues (commonly with HEIC/JPEG files).

This guide documents CLI-based fixes using Homebrew tools.

---

## Prerequisites

Install required dependencies:

```bash
brew install imagemagick exiftool libheif
```

---

## Root Cause

* iPhone edits introduce non-standard HEIC/JPEG encoding or extra layers
* EXIF/metadata becomes malformed during transfer
* macOS Preview is stricter than browser decoders

---

## Solutions

### 1. Re-encode Image (Recommended)

Rewrite image into a clean format:

```bash
magick input.heic output.jpg
```

Or keep HEIC:

```bash
magick input.heic output.heic
```

---

### 2. Strip Metadata

Remove problematic EXIF data:

```bash
exiftool -all= input.heic -o fixed.heic
```

---

### 3. Force Decode via libheif

```bash
heif-convert input.heic output.jpg
```

---

## Batch Processing (JPEG Files)

### Safe In-place Re-encode (preserves filename)

```bash
for f in *.JPG; do
  magick "$f" -strip "${f}.tmp" && mv "${f}.tmp" "$f"
done
```

---

### Recursive Processing (all subdirectories)

```bash
find . -type f \( -iname "*.jpg" -o -iname "*.jpeg" \) | while read -r f; do
  magick "$f" -strip "${f}.tmp" && mv "${f}.tmp" "$f"
done
```

---

### Stronger Re-encode (if files still fail)

```bash
for f in *.JPG; do
  magick "$f" -strip -define jpeg:extent=100% "${f}.tmp" && mv "${f}.tmp" "$f"
done
```

---

## Verification

Check file integrity:

```bash
file *.JPG
```

---

## Key Notes

* Re-encoding removes incompatible metadata and hidden layers
* File names remain unchanged during batch processing
* JPEG output is the most universally compatible format
* This approach fixes most Preview/Quick Look failures on macOS

---

## License

MIT (optional — adjust as needed)
