# 🔐 BitLocker on macOS (Apple Silicon)

Mount **BitLocker-encrypted drives** on macOS using [anylinuxfs](https://github.com/nohajc/anylinuxfs).

> ✅ Tested on macOS with Apple Silicon (M1/M2/M3/M4)

---

## 📋 Prerequisites

- macOS on **Apple Silicon** (M1, M2, M3, M4)
- [Homebrew](https://brew.sh) installed

---

## 🚀 Installation

### Step 1 — Install anylinuxfs

```bash
brew tap nohajc/anylinuxfs
brew install anylinuxfs
```

### Step 2 — Initialize anylinuxfs

On first run, anylinuxfs downloads a minimal Alpine Linux image and sets up the microVM environment. Run this once:

```bash
anylinuxfs init
```

> 💡 This only needs to be done once. It sets up `~/.anylinuxfs/alpine` in the background.

---

## 🔑 Recovery Key

Before mounting, make sure you have your **BitLocker recovery key** (48-digit) or **password** ready.

If you don't have it, find your recovery key at:  
👉 [account.microsoft.com/devices/recoverykey](https://account.microsoft.com/devices/recoverykey)

---

## 📖 Usage

### Step 1 — List available drives

Plug in your drive, then run:

```bash
sudo anylinuxfs list
```

**Example output:**
```
/dev/disk4 (external, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     FDisk_partition_scheme                        *1.0 TB     disk4
   1:                  BitLocker ALEXA WD                1.0 TB     disk4s1
```

Note the **IDENTIFIER** (e.g. `disk4s1`) — you'll need it to mount.

---

### Step 2 — Mount the drive

```bash
sudo anylinuxfs /dev/disk4s1
```

You will be prompted:
```
Enter passphrase for /dev/disk4s1:
```

Enter your BitLocker password or 48-digit recovery key. Once mounted, the drive appears in Finder under **Locations**.

---

### Step 3 — Verify the mount

```bash
anylinuxfs status
```

**Example output:**
```
/dev/disk4s1 on /Volumes/WD (ntfs, uid=502, gid=20, mounted by alex)
```

---

### Step 4 — Unmount the drive

```bash
anylinuxfs unmount
```

Or simply eject it from Finder before unplugging.

---

## 🛠️ Troubleshooting

**Drive not showing up?**  
Make sure the drive is plugged in before running `anylinuxfs list`. Confirm macOS sees it with:
```bash
diskutil list
```

**Wrong password / mount fails?**  
Double check your recovery key at [account.microsoft.com/devices/recoverykey](https://account.microsoft.com/devices/recoverykey) and try again.

**Port conflict error?**  
Make sure nothing is running on ports `2049`, `32765`, or `32767`. anylinuxfs uses these for its internal NFS server.

**Only one drive at a time**  
anylinuxfs supports mounting one drive at a time. Unmount the current drive before mounting another:
```bash
anylinuxfs unmount
```

---

## 🙏 Credits

- [anylinuxfs](https://github.com/nohajc/anylinuxfs) — the microVM-based filesystem mounter that powers this

---

## 📄 License

MIT