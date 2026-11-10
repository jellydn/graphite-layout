# Install Graphite on macOS or Windows

Graphite changes the characters that your physical keys produce. It does not
change the key labels. See the [layout and design notes](README.md#the-layout).
Use this guide for the standard Graphite layout, including its punctuation.

## Choose a method

| System | Method | Status |
| --- | --- | --- |
| macOS | Install the bundled [`macos/Graphite.zip`](macos/Graphite.zip) for your user account. | Included in this repository. |
| Windows | Install the community MSKLC package from [upstream PR #10](https://github.com/rdavison/graphite-layout/pull/10). | Optional external package; the PR was open and unmerged when checked. No Windows package is included in this repository. |

For open-source remapper alternatives, see [Kanata and Karabiner setup](REMAPPERS.md).
Kanata supports Windows and macOS; Karabiner-Elements is macOS-only. Example
configurations are included. Use a US/QWERTY input source with a remapper,
**not** a native Graphite input source at the same time.

**Verification boundary:** Sources and archive contents were checked on
10 November 2026. The macOS base and Shift mappings and the Windows `.klc` source
were inspected. Installation, activation, shortcuts, login, and removal were
**not tested on Windows or macOS**. File inspection does not prove that an
installer is safe or that a current OS version supports it.

The Windows contributor reports a layout based on US input in Windows 11.
The archive contains x86 (`i386`), x64 (`amd64`), and Itanium (`ia64`) packages.
It has **no ARM64 package**; IA-64 is not ARM64. Do not assume support for Windows
on ARM. The repository gives no macOS version or processor support matrix.
The macOS bundle contains a `.keylayout` resource, not an executable installer.

No separately named Graphite variants are bundled. The README discusses optional
letter swaps, but they are not separate install choices. A custom variant or
QWERTY-shortcut variant needs its own verified configuration. Do not assume that
these packages preserve QWERTY shortcut positions. ANSI, ISO, and JIS keyboards
can have different extra keys; test your physical keyboard.

## Before you start

- Save your work. Keep your current input source, such as US/QWERTY, installed.
- Learn how to select that source with the mouse before you select Graphite.
- Use only one remapping method. Turn off other remappers for the first test.
  If keyboard firmware already produces Graphite, do not also apply this layout.
- Record your current input sources and default source. Back up any existing
  Graphite bundle before you replace it.
- Use a local plain-text editor for tests. Do not type a real password into a
  document or a public typing-test site. Keep a known layout for login and recovery.
- On a managed device, get approval from its administrator. Do not bypass OS
  security warnings or policy restrictions.

## macOS: install the bundled layout

### Prerequisites

Download this repository with **Code > Download ZIP**, then extract it, or use
your existing Git checkout. You need Finder access to your user Library folder.
Ukelele, Homebrew, and a compiler are not required for this copy method.

### Install and activate

1. In the extracted repository, double-click `macos/Graphite.zip`.
   The layout is **`Graphite.bundle`**. Do not copy the ZIP or the `__MACOSX`
   metadata folder into the installation folder.
2. In Finder, press **Command-Shift-G**. Enter `~/Library` and press Return.
   Open **Keyboard Layouts**. If the folder does not exist, create it with that
   exact name. The `~` means your home folder.
3. Copy the complete `Graphite.bundle` into `~/Library/Keyboard Layouts`.
   Keep its internal `Contents` folder intact. This guide uses a per-user install,
   not `/Library/Keyboard Layouts` for all users.
4. Save your work, log out, and log back in. This is the refresh step in the
   repository's original instructions.
5. On current macOS, open **System Settings > Keyboard > Text Input > Edit**.
   Click **+**, then look for **Graphite** under English or search for it.
   If necessary, check **Others**. Select it and click **Add**.
   On older macOS, use **System Preferences > Keyboard > Input Sources**.
6. Select **Graphite** from the Input menu in the menu bar. If the menu is hidden,
   enable **Show Input menu in menu bar** in Input Sources settings.
   **Control-Space** normally selects the last input source; use the menu if that
   shortcut has been changed or is in use by another app.
7. Run the [tests below](#test-before-daily-use). Use **Show Keyboard Viewer** in
   the Input menu to inspect the current layout and its modifier layers.

This user-folder install is not a procedure to change FileVault pre-boot input
or the login-window layout. Do not rely on Graphite being available there.

### Remove or roll back on macOS

1. Select your previous source in the Input menu and confirm that it works.
2. In the same Input Sources settings, select Graphite and click **−** to remove it.
   Keep at least one known input source.
3. In Finder, go to `~/Library/Keyboard Layouts`. Move only `Graphite.bundle`
   to the Trash. Do not delete the Keyboard Layouts folder or other layouts.
4. Save your work, log out, and log back in. Confirm that the previous source works.
   To restore an earlier Graphite version, copy your saved bundle back and repeat
   the activation steps. Do not empty the Trash until rollback is complete.

## Windows: optional community package

### Prerequisites and trust check

There is no Windows installer on this repository's main branch. First read
[upstream PR #10](https://github.com/rdavison/graphite-layout/pull/10), including
its current review status. The inspected revision is
[`fec1521`](https://github.com/rdavison/graphite-layout/commit/fec15210cfdfacd113b0aeaf5bfa80c038557892).
This guide does not endorse its compiled files or claim a verified signature.
Proceed only if you trust the contributor and can approve installation on your PC.
Expect an administrator permission prompt for a system keyboard package.

Use an x86 or x64 Windows PC for this candidate method. Windows 11 is the
contributor's stated system, not a compatibility guarantee. Windows 10 installation
was not verified. Stop on ARM64 or if your administrator blocks the package.

### Download, install, and activate

1. Open the pinned revision's
   [`windows/Graphite.zip`](https://github.com/rdavison/graphite-layout/blob/fec15210cfdfacd113b0aeaf5bfa80c038557892/windows/Graphite.zip)
   file page and use **Download raw file**. Do not look for this file in the main
   branch ZIP. The adjacent
   [`Graphite.klc`](https://github.com/rdavison/graphite-layout/blob/fec15210cfdfacd113b0aeaf5bfa80c038557892/windows/Graphite.klc)
   is the editable source, not an installer.
2. In File Explorer, select **Extract All** for `Graphite.zip`. Keep all extracted
   files and architecture folders together. Do not run setup inside the ZIP.
3. Review the package with your normal security tools. If you accept it, run
   **`setup.exe`** in the extracted folder, as the contributor instructs.
   Approve elevation only for the package you chose. If setup fails or Windows
   blocks it, stop; do not disable security controls or copy DLLs by hand.
4. In Windows 11, open **Settings > Time & language > Language & region**.
   Open **… > Language options** for **English (United States)**. Under
   **Keyboards**, check for **US - Graphite**. If it is not active in the list,
   use **Add a keyboard** and select it if available.
   In Windows 10, the corresponding path is **Settings > Time & Language >
   Language > English (United States) > Options**.
   If needed, check the current display language's keyboard list too.
   Do not change the Windows display language just to change keyboard input.
5. Select **US - Graphite** from the taskbar input indicator (often **ENG**).
   **Windows-Space** cycles through installed layouts. Keep US/QWERTY available.
6. Run the [tests below](#test-before-daily-use) before you make it your default.
   Do not copy these settings to the welcome screen or other users as part of this test.

**Build-from-source alternative:** Microsoft publishes
[MSKLC 1.4](https://www.microsoft.com/en-us/download/details.aspx?id=102134), which
can edit `.klc` files and build install packages. It is not required to run the
prebuilt setup. Building this Graphite source was not tested. Microsoft's download
page has legacy OS and .NET requirements; it is not proof of Windows 11 or ARM64
build support. Use its documented prerequisites and validation tools before you
build. No build recipe or additional runtime dependency is verified here.

### Remove or roll back on Windows

1. Select your previous layout with the taskbar indicator or **Windows-Space**.
   Confirm that it works. If you changed the default under **Typing > Advanced
   keyboard settings**, restore your previous default.
2. In the language's **Language options > Keyboards**, remove **US - Graphite**.
   If **Remove** is unavailable, add a known layout first. Check other language
   entries if Graphite is not under English (United States).
3. Removing a keyboard selection does not necessarily uninstall its package.
   Look for the matching Graphite entry in **Settings > Apps > Installed apps**
   (Windows 11), **Apps & features** (Windows 10), or **Control Panel > Programs
   and Features**, and use its uninstall action if present.
   The exact installed entry and uninstaller were not tested. If no entry exists,
   stop and ask the package contributor or your administrator for removal steps.
   Do not delete registry entries or system DLLs manually.
4. Restart if the installer requests it. Confirm that normal input works and
   Graphite is no longer selected. Keep the downloaded package until removal is
   complete. For a version rollback, uninstall the new package before installing
   a previously trusted version; that upgrade path was not tested.

## Test before daily use

Use TextEdit in plain-text mode on macOS, or Notepad on Windows. Disable smart
quotes or other text substitutions for the test. These are **user acceptance
checks to run**, not results from device testing.

Press the physical US/QWERTY positions below, left to right. Do not type the
expected result by its printed letters. Use no modifiers for the first pass,
then hold Shift for the second pass.

| Physical US/QWERTY positions | Expected base output | Expected Shift output |
| --- | --- | --- |
| `q w e r t y u i o p [ ]` | `b l d w z ' f o u j ; =` | `B L D W Z _ F O U J : +` |
| `a s d f g h j k l ; '` | `n r t s g y h a e i ,` | `N R T S G Y H A E I ?` |
| `z x c v b n m , . /` | `q x m c v k p . - /` | `Q X M C V K P > " <` |

Also test the number row, brackets, backslash, Space, Enter, Backspace, Caps Lock,
and both Shift keys. Check copy, paste, undo, and app-specific shortcuts in a
disposable document. Check Option/dead keys on macOS and Ctrl/Alt behavior on
Windows if you use them. The table does not define those additional layers.

Finally, switch back to your old layout and repeat a short typing test. Confirm
that you can recover with the mouse. Do not change login defaults until you have
separately verified how your OS handles input at each login or unlock screen.

## Troubleshooting

| Symptom | Check or safe action |
| --- | --- |
| Graphite is missing on macOS. | Confirm `~/Library/Keyboard Layouts/Graphite.bundle/Contents` exists, not a nested ZIP folder. Log out and back in; reopen settings. Check English and Others. If still missing, return to your old source. |
| US - Graphite is missing on Windows. | Confirm setup completed. Check English (United States) and your display language's keyboard list. Save work and sign out/in if the list has not refreshed. If still absent, ask the contributor; do not edit the registry. |
| Keys produce unexpected characters. | Confirm the active source in the menu or taskbar. Stop other remappers and check keyboard firmware. Compare physical positions with the test table, not the keycap letters. |
| Only one app or shortcut is wrong. | Test in a plain-text editor. Check that app's shortcuts and per-window input selection. Do not assume QWERTY shortcut positions. |
| Quotes or punctuation differ. | Turn off smart punctuation for the test. Check Shift and any active Option/Alt layer. Graphite deliberately changes punctuation positions. |
| Login or unlock input differs. | Select a known layout with the screen's input selector if available. Use the OS recovery process if necessary. This guide does not change pre-boot or login settings. |
| Security or device policy blocks installation. | Stop and ask the administrator. Do not disable protection. |

## Sources and scope

- **Repository evidence:** [README layout](README.md#ascii-version), bundled
  `macos/Graphite.zip`, and the original macOS instructions. The bundle identifies
  Graphite as an English input source. Base/Shift rows match the table above.
- **Windows package evidence:** [upstream PR #10](https://github.com/rdavison/graphite-layout/pull/10)
  and its pinned source/archive. The `.klc` declares `en-US` and `US - Graphite`;
  its base/Shift rows match the table. Source inspection does not establish that
  the compiled DLLs implement the same mapping.
- **Apple:** [Add and switch input sources; use Keyboard Viewer](https://support.apple.com/guide/mac-help/write-in-another-language-on-mac-mchlp1406/mac).
  [Input Sources settings](https://support.apple.com/guide/mac-help/change-input-sources-settings-mchl84525d76/mac)
  also documents removal and the Input menu option. These verify the OS controls,
  not compatibility of this bundle.
- **SIL, the Ukelele author:** [Keyboard layout installation](https://software.sil.org/ukelele/)
  and [version history](https://software.sil.org/ukelele/ukelele-version-history).
  These describe custom layouts and current-user installation. This guide keeps
  the repository's manual bundle-copy method; it does not require Ukelele.
- **Microsoft:** [Manage keyboard layouts](https://support.microsoft.com/en-us/windows/manage-the-language-and-keyboard-input-layout-settings-in-windows-12a10cb4-8626-9b77-0ccb-5013e0c7c7a2)
  and [MSKLC download and requirements](https://www.microsoft.com/en-us/download/details.aspx?id=102134).
  These verify Windows controls and tool capabilities, not this community package.

See [the remapper guide](REMAPPERS.md) for third-party tools. This guide does not
cover keyboard firmware flashing or custom layout generation. It does not claim
medical benefits or guaranteed typing speed. No layout was installed on a user's
device to prepare this guide.
