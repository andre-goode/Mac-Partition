# Windows on a Mid-2014 15" MacBook Pro (Boot Camp) for TFT

A step-by-step guide to partitioning the Mac's drive and installing Windows with
Apple's Boot Camp Assistant so Windows-only games (Teamfight Tactics) can run.

## 0. Check before you start

- **Riot's anti-cheat (Vanguard):** Riot games require Vanguard on Windows. On
  Windows 11 it requires TPM 2.0 + Secure Boot, which this Mac does **not**
  have. **Windows 10 (22H2)** is the version Apple's Boot Camp drivers support
  on this model. Windows 10 is past Microsoft's end of support, so check Riot's
  current system requirements page before investing the time.
- **Free space:** at least 64 GB; aim for **100–128 GB** for Windows + TFT +
  updates.
- **Back up the Mac** with Time Machine. Repartitioning is usually safe, but a
  backup is the only real safety net.
- Plug the Mac into power for the whole process.
- Have a 16 GB+ USB flash drive on hand in case Boot Camp Assistant asks for one.

## 1. Update macOS and download Windows

1. Apple menu > System Preferences > Software Update (the newest macOS this
   model supports is Big Sur 11).
2. Download the **Windows 10 64-bit ISO** from
   <https://www.microsoft.com/software-download/windows10ISO> (from a Mac the
   site offers the ISO directly).

## 2. Partition and install with Boot Camp Assistant

1. Open **Applications > Utilities > Boot Camp Assistant**.
2. Choose the Windows ISO.
3. Drag the divider to give Windows its size (e.g. 128 GB). This is the
   partitioning step; macOS keeps the rest.
4. Click **Install**. Boot Camp downloads the Windows support (driver)
   software, partitions the drive, and restarts into the Windows installer.

## 3. Windows setup

1. When asked where to install, pick the partition named **BOOTCAMP**. If the
   installer insists, select it and click **Format**. Do not touch the other
   partitions.
2. Finish setup. If asked for a product key, you can choose "I don't have a
   product key" and activate later.
3. After first login, the **Boot Camp installer** should launch automatically.
   If not, open File Explorer > OSXRESERVED (or the USB drive) > BootCamp >
   `Setup.exe`. This installs trackpad, keyboard, Wi-Fi, and graphics drivers.
4. Run **Apple Software Update** (installed with Boot Camp) and **Windows
   Update**.

## 4. Install TFT

1. Download the Riot Client from the official League of Legends / TFT site.
2. Install, restart when Vanguard asks you to, and launch TFT.
3. Suggested starting settings for the Iris Pro / GeForce GT 750M: 1440x900 or
   1680x1050, medium quality, frame cap at 60.

## 5. Switching between macOS and Windows

- Hold **Option (⌥)** at startup to pick a disk.
- From Windows: Boot Camp icon in the system tray > **Restart in macOS**.
- From macOS: System Preferences > Startup Disk.

## Removing Windows later

Boot Camp Assistant > **Restore** removes the Windows partition and returns the
space to macOS.

## Alternatives if Vanguard won't run

- **GeForce NOW** (cloud gaming) runs on macOS without Windows, but check
  whether TFT is in its library.
- Play TFT on a phone or tablet (the mobile version shares your account).
