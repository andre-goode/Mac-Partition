# Windows on a Mid-2014 15" MacBook Pro (Boot Camp) for TFT

A step-by-step guide to partitioning the Mac's drive and installing Windows with
Apple's Boot Camp Assistant so Windows-only games (Teamfight Tactics) can run.

## Downloads

| What | Where | Notes |
|---|---|---|
| Windows 10 ISO | <https://www.microsoft.com/en-us/software-download/windows10ISO> | Choose **Windows 10 (multi-edition ISO)**, your language, then **64-bit Download**. The link expires after 24 hours. |
| Boot Camp drivers (Windows Support Software) | Built into **Boot Camp Assistant** | Downloaded automatically during setup. If you need them again: Boot Camp Assistant > menu bar **Action > Download Windows Support Software**. |
| Teamfight Tactics / Riot Client | <https://teamfighttactics.leagueoflegends.com> | Download it **inside Windows** with the "Play for free" button. Vanguard installs with it. |
| Apple Boot Camp help | <https://support.apple.com/boot-camp> | Apple's official instructions. |

Boot Camp Assistant and Time Machine are already on the Mac, so you don't need
to download them.

## 0. Check before you start

- **Why Boot Camp:** after TFT moved to Unreal Engine (Patch 18.2, 2026), the
  Mac version supports only Apple Silicon (M1 and later). Intel Macs like this
  one have to run the Windows version.
- **Riot's anti-cheat (Vanguard):** TFT on Windows needs Windows 10 version
  19041 or newer, or Windows 11 with TPM 2.0. This Mac has no TPM, so use
  **Windows 10 22H2** (build 19045), which also matches Apple's Boot Camp
  drivers. Windows 10 stopped getting free updates on Oct 14, 2025, but Riot
  still lists it as supported.
- **Graphics:** TFT's minimum GPU is Intel HD 4600 or GeForce 400 series. The
  Iris Pro 5200 and GeForce GT 750M in this Mac are both above that, so the
  game should run, though not at max settings.
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
   <https://www.microsoft.com/en-us/software-download/windows10ISO> (from a Mac
   the site offers the ISO directly; it's about 6 GB).

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

1. In Windows, open Edge and download the Riot Client from
   <https://teamfighttactics.leagueoflegends.com> ("Play for free").
2. Install, restart when Vanguard asks you to, and launch TFT.
3. Suggested starting settings for the Iris Pro / GeForce GT 750M: 1440x900 or
   1680x1050, medium quality, frame cap at 60.

## 5. Switching between macOS and Windows

- Hold **Option (⌥)** at startup to pick a disk.
- From Windows: Boot Camp icon in the system tray > **Restart in macOS**.
- From macOS: System Preferences > Startup Disk.

## Troubleshooting: "Your disk could not be partitioned"

What fixed it on this Mac:

1. **Delete Time Machine local snapshots** (they block shrinking the drive):
   `sudo tmutil disable`, then `tmutil listlocalsnapshots /` and
   `diskutil apfs listSnapshots disk1s1`, and delete each Time Machine snapshot
   with `sudo diskutil apfs deleteSnapshot disk1s1 -uuid <UUID>`.
2. **Check how far the drive can shrink:**
   `diskutil apfs resizeContainer disk0s2 limits`. The "Minimum" must be well
   below the size macOS will keep.
3. **Repair file system errors from Recovery Mode** (Command + R > Utilities >
   Terminal). Find the container with `diskutil list internal` (it was
   `disk3` in Recovery), then:
   `diskutil unmountDisk force disk3` and `fsck_apfs -y /dev/disk0s2`
   (run until it reports no warnings).
4. **Create the partition manually** back in macOS (macOS keeps 151 GB,
   Windows gets the rest, about 100 GB):
   `sudo diskutil apfs resizeContainer disk0s2 151g MS-DOS BOOTCAMP 0`
5. Boot the Boot Camp USB installer: hold **Option** at startup > **EFI Boot**,
   then pick the **BOOTCAMP** partition in Windows Setup and click **Format**.
6. Turn Time Machine back on afterwards: `sudo tmutil enable`.

## Removing Windows later

Boot Camp Assistant > **Restore** removes the Windows partition and returns the
space to macOS.

## Alternatives if Vanguard won't run

- **GeForce NOW** (cloud gaming) runs on macOS without Windows, but check
  whether TFT is in its library.
- Play TFT on a phone or tablet (the mobile version shares your account).
