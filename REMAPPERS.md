# Graphite with open-source remappers

Use this guide instead of a native Graphite input source. “Kitana” is assumed to
mean **Kanata**. Sources and stable releases were checked on 10 November 2026.

## Recommendation and comparison

| Tool | Platforms | Best fit | Setup cost and limits |
| --- | --- | --- | --- |
| [Kanata](https://github.com/jtroo/kanata) (LGPL-3.0) | Windows, macOS, Linux | One Graphite configuration across systems; layers later if needed. | Windows winIOv2 needs no extra kernel driver. macOS needs a compatible Karabiner virtual HID driver, a daemon, root, and privacy permissions. |
| [Karabiner-Elements](https://github.com/pqrs-org/Karabiner-Elements) (public domain) | **macOS only** | A Mac settings UI, device selection, and editable JSON rules. | Needs background services, privacy permissions, and its virtual HID driver. Not a Windows tool. |
| [KMonad](https://github.com/kmonad/kmonad) (MIT) | Windows, macOS, Linux | Users who already use KMonad layers. | Cross-platform, but input/output configuration and macOS driver setup are platform-specific. No example is supplied here. |
| [AutoHotkey v2](https://www.autohotkey.com/) (GPLv2) | **Windows only** | Users who already need Windows automation. | Script-based; Shift punctuation and shortcut policy need explicit handling. Its keyboard hooks can conflict with Kanata. No example is supplied here. |

**Choose Kanata** for Windows or for a shared Windows/Mac configuration.
Start with Windows **winIOv2**, not Interception. **Choose Karabiner-Elements**
for a Mac-only setup if you prefer a UI. If you do not need layers or device rules,
the [bundled native macOS layout](INSTALL.md#macos-install-the-bundled-layout)
avoids a running remapper. These are alternatives, not tools to stack together.

Current stable releases checked: [Kanata **1.12.0**](https://github.com/jtroo/kanata/releases/tag/v1.12.0),
[Karabiner-Elements **16.3.0**](https://github.com/pqrs-org/Karabiner-Elements/releases/tag/v16.3.0),
[KMonad **0.4.5**](https://github.com/kmonad/kmonad/releases/tag/0.4.5), and
[AutoHotkey **2.0.31**](https://github.com/AutoHotkey/AutoHotkey/releases/tag/v2.0.31). Check each release's requirements
before upgrading. Older macOS releases may require an older Karabiner version;
the current download page lists 16.3.0 for macOS 13 and later, on Intel and Apple
Silicon. Kanata's 1.12.0 guide says macOS 11+, but its required 6.2.0 driver's
[own support list](https://github.com/pqrs-org/Karabiner-DriverKit-VirtualHIDDevice/tree/v6.2.0#supported-systems)
is narrower: macOS 13–15 on Intel/Apple Silicon, and macOS 26 on Apple Silicon
(Intel is expected but untested by that author). This pinned Kanata setup is not
verified for macOS 11/12 or 27; use a documented compatible alternative instead.

## Shared assumptions: read before enabling either example

- Select **US/QWERTY** in Windows or **U.S.** on macOS. Do not select the native
  Graphite layout while the remapper runs. These examples emit US key codes, not
  Unicode text. A non-US source, IME, or an Option/AltGr layer can change output.
- The examples target the US ANSI positions in the README, including punctuation.
  ISO/JIS extra keys are not mapped. Set Karabiner's virtual keyboard type to match
  your keyboard, then test its symbol keys. Do not assume identical extra-key behavior.
- **Shortcuts follow Graphite positions**, not printed QWERTY positions. For
  example, physical `v` emits `c`, so use Command/Control plus physical `v` for
  Copy. Physical `c` emits `m`. These are not QWERTY-shortcut variants.
- Shift produces uppercase letters. Four pairs need special rules: physical
  `y` gives `'` / `_`, physical quote gives `,` / `?`, physical period gives
  `-` / `"`, and physical slash gives `/` / `<`. A simple one-key substitution
  cannot reproduce these pairs. Other modifiers remain active; Option/AltGr
  characters and application shortcuts are not guaranteed to match the native bundle.
- Caps Lock is not a separate Graphite layer here. Test it, both Shift keys,
  held keys, key repeat, and overlapping key releases. Static checks below do not
  verify timing or OS Caps Lock behavior.
- Stop other remappers, including AutoHotkey keyboard hooks and Karabiner rules
  that affect these keys. Keep keyboard firmware in QWERTY. Do not run Kanata and
  Karabiner-Elements remapping on the same device. Sharing a driver is not the
  same as safe concurrent remapping, and driver versions can conflict.
- Back up existing configurations. Keep a mouse and a known way to stop the tool.
  Save work and test in a local disposable document, not a password field.
  Do not enable startup services, scheduled tasks, or login-screen changes yet.

## Kanata 1.12.0

Example: [`configs/graphite.kbd`](configs/graphite.kbd). Copy it into a folder you
control. It maps both Shift keys explicitly so `fork` can test either Shift state.
Other keys pass through. No shell commands or TCP server are enabled.

### Windows: start without an extra driver

1. Download the correct CPU archive from the
   [1.12.0 release](https://github.com/jtroo/kanata/releases/tag/v1.12.0):
   `windows-binaries-x64.zip` for Intel/AMD, or `windows-binaries-arm64.zip` for ARM.
   Check the archive against the release's `sha256sums`, then extract it.
2. Choose the **tty winIOv2** executable without `cmd_allowed`. For x64 its name
   is `kanata_windows_tty_winIOv2_x64.exe`. This uses Windows hooks and SendInput;
   it does not require Interception. Do not install a driver for this method.
3. Put the example in the same folder. Open PowerShell there. Check syntax first:

   ```powershell copy Copy Windows Syntax Check
   .\kanata_windows_tty_winIOv2_x64.exe --check --cfg .\graphite.kbd
   ```

4. If the check passes, start it in the foreground:

   ```powershell copy Copy Windows Start Command
   .\kanata_windows_tty_winIOv2_x64.exe --cfg .\graphite.kbd
   ```

   On ARM, substitute the tty winIOv2 executable from the ARM64 archive.
   Keep the terminal open. Begin without elevation in a normal editor. Windows
   security boundaries can block injected input in elevated apps and secure
   screens. Do not bypass UAC, Secure Desktop, anti-cheat, or device policy.
5. Run the tests below. Stop the process before testing another remapper.

**Why not Interception first?** The release also supplies `wintercept` builds.
They require an administrator-installed driver and reboot. The author documents
a known failure that can disable keyboard/mouse input until reboot. Use the
[release instructions](https://github.com/jtroo/kanata/releases/tag/v1.12.0)
and [Interception notes](https://github.com/jtroo/kanata/blob/v1.12.0/docs/interception.md)
only if you have a specific need. This guide does not install it or validate it.

### macOS: match the driver to Kanata

1. Read the **[1.12.0 macOS setup guide](https://github.com/jtroo/kanata/blob/v1.12.0/docs/setup-macos.md)**.
   Use its supported **Karabiner-DriverKit-VirtualHIDDevice 6.2.0** package.
   Do not use an arbitrary newer driver or mix stable Kanata with instructions
   from `main`. Driver IPC versions must match. If Karabiner-Elements is already
   installed, do not overwrite its driver to try Kanata; use Karabiner instead
   or resolve the version conflict before proceeding.
2. Download the Intel (`macos-binaries-x64.zip`) or Apple Silicon
   (`macos-binaries-arm64.zip`) archive from the same Kanata release. Verify its
   checksum, extract it, and use the non-`cmd_allowed` executable. Put it in a
   stable path. Make it executable with `chmod +x` if necessary. A compiler and
   Xcode tools are needed only for a source build, not these binaries.
3. Install the driver package and approve its extension with an administrator.
   Follow the driver manager's system settings prompts. The driver's
   [own usage guide](https://github.com/pqrs-org/Karabiner-DriverKit-VirtualHIDDevice/tree/v6.2.0#usage)
   gives this activation command:

   ```sh copy Copy Driver Activation Command
   sudo /Applications/.Karabiner-VirtualHIDDevice-Manager.app/Contents/MacOS/Karabiner-VirtualHIDDevice-Manager activate
   ```

   The standalone driver also needs its virtual HID **daemon running**. In a
   separate terminal, start it in the foreground and keep that terminal open:

   ```sh copy Copy Driver Daemon Command
   sudo '/Library/Application Support/org.pqrs/Karabiner-DriverKit-VirtualHIDDevice/Applications/Karabiner-VirtualHIDDevice-Daemon.app/Contents/MacOS/Karabiner-VirtualHIDDevice-Daemon'
   ```

   Driver installation alone does not start the daemon automatically. Do not add
   a startup job for the first test.
4. Grant **Input Monitoring** and **Accessibility** as directed in the pinned
   guide, under **System Settings > Privacy & Security**. Check the actual binary
   and responsible terminal process if macOS prompts for it. Do not disable SIP
   or other security controls. Replacing a binary may require permission review.
5. From the extracted folder, check syntax, then start the remapper. This example
   uses Apple Silicon names; substitute `kanata_macos_x64` on Intel:

   ```sh copy Copy Mac Syntax Check
   ./kanata_macos_arm64 --check --cfg ./graphite.kbd
   ```

   ```sh copy Copy Mac Start Command
   sudo ./kanata_macos_arm64 --cfg ./graphite.kbd
   ```

   The upstream guide requires root for access to the virtual HID daemon.
   Keep the terminal open and test before adding a LaunchDaemon. This is a
   setup recipe from upstream, not a successful device test from this project.

### Disable or remove Kanata safely

Hold **Left Control + Space + Escape** together for Kanata's emergency exit.
The example leaves those physical keys unchanged. Alternatively, stop the
foreground process with Control-C in its terminal (physical Control plus `v`
under this Graphite map), or use the OS process manager with the mouse. Release
held keys, then confirm normal US/QWERTY input.

This guide adds no startup job. If you added one separately, disable it first or
it can restart Kanata. On Windows, remove only your Kanata startup entry and
binary/config when no longer needed; this winIOv2 method installed no extra driver.
On macOS, follow the pinned guide's uninstall section for **only the jobs and
driver you installed**, and the [driver's own uninstaller](https://github.com/pqrs-org/Karabiner-DriverKit-VirtualHIDDevice/tree/v6.2.0#uninstallation)
for full standalone driver removal. Do not remove a virtual HID driver used by another app.
Restore saved configs to roll back a config edit, rerun `--check`, and restart.
Revoking privacy permissions is cleanup, not a substitute for stopping a process.

## Karabiner-Elements 16.3.0: macOS only

Example: [`configs/graphite-karabiner.json`](configs/graphite-karabiner.json).
This is **one complex-modification rule**, not a complete `karabiner.json` profile.

1. Back up `~/.config/karabiner` if it exists. Download from the
   [official site](https://karabiner-elements.pqrs.org/) and follow its
   [installation guide](https://karabiner-elements.pqrs.org/docs/getting-started/installation/).
   Approve the installer and its virtual HID driver. Follow the prompts for
   privileged/non-privileged background services, Accessibility, Input Monitoring,
   and Driver Extension permission. Do not use Kanata's older standalone driver
   in place of the driver required by this Karabiner release.
2. Select **U.S.** input, choose the matching virtual keyboard type, and stop
   Kanata. In **Devices**, restrict modification to the keyboard you want to test.
3. In **Complex Modifications > Add your own rule**, replace the editor's sample
   with the **entire example rule JSON**, then save. Do not replace your complete
   `~/.config/karabiner/karabiner.json` with this file. Check that this rule is
   enabled and that no earlier rule or Simple Modification remaps its input keys.
4. Run the tests below. Use Karabiner-EventViewer to inspect key codes and
   modifiers; use a plain-text editor to check actual characters. EventViewer
   alone does not prove the output of the active OS input source.

The four Shift-specific manipulators come **before** the general mappings.
Karabiner uses the first matching manipulator. `mandatory: ["shift"]` consumes
the incoming Shift flag; the output adds Shift for the new punctuation key.
`optional: ["any"]` allows and retains other modifiers. General mappings retain
Shift too, so letters become uppercase. This intentionally changes Command
shortcuts to Graphite positions. It is not an app-specific shortcut bypass.

**Disable/rollback:** With the mouse, remove this Graphite rule from Complex
Modifications in the active profile, or switch to a saved profile without it.
Check that U.S. typing is restored. Keep unrelated rules and your backup.
For full removal, use **Karabiner-Elements Settings > Uninstall > Launch
uninstaller**, as the [official uninstall guide](https://karabiner-elements.pqrs.org/docs/manual/operation/uninstall/)
instructs. Quitting the settings window alone does not disable its background
remapping. Do not delete shared driver files manually.

## Acceptance tests, troubleshooting, and verification limits

Run the [base/Shift table](INSTALL.md#test-before-daily-use) with each Shift key
separately. Also check physical `-`/`=` → `[`/`]`, their Shift outputs `{`/`}`,
backslash/pipe, digits, Space, Enter, Caps Lock, key repeat, and text selection.
Test Command/Control-C/V/Z and their Shift combinations by **Graphite position**.
Test Shift pressed before a key and released before/after that key. Disable the
tool and repeat a short US typing test before you enable daily use.

| Problem | Safe check |
| --- | --- |
| Wrong letters or symbols | Select US/U.S., not native Graphite. Stop other remappers and check firmware. |
| Karabiner JSON saves but does not remap | Check the active profile, rule order, Devices, permissions, and virtual keyboard type. |
| Kanata macOS has connection/grab errors | Check root, matching driver, approved extension, running daemon, and privacy grants. Stop other tools that grab the device. |
| Windows remaps in one app only | Check app elevation and competing hooks. Secure screens and some OS shortcuts cannot be remapped normally. Do not add Interception as an automatic workaround. |
| Modifier stuck or typing unsafe | Release keys, stop the remapper with its exit method, and test US input. Do not leave a failing configuration enabled. |
| Login, unlock, remote desktop, or VM differs | Treat these as separate untested input paths. Keep recovery input available; do not change login defaults based on editor tests. |

**Verified in a Linux orb, without input capture:** Kanata 1.12.0's real `--check`
parser accepts the example. JSON parsing and static checks against Karabiner's
documented rule shape pass. Static mapping checks compare both examples with the
README's full ASCII diagram: 47 keys with no Shift, left Shift, right Shift, or
both Shifts (188 static cases per example). Modifier-retention settings were
inspected; shortcut delivery was not tested.
These checks do **not** run the Windows/macOS input backends or Karabiner's parser.

**Not tested:** OS installation, driver activation, privacy prompts, device event
delivery, timing, Caps Lock interactions, app shortcuts, secure/login screens,
remote sessions, and uninstall. No layout or remapper was installed on a user's
device. A parser pass is not a runtime compatibility or safety guarantee.

## Authoritative references

- [Kanata 1.12.0 release, binaries, checksums, and Windows instructions](https://github.com/jtroo/kanata/releases/tag/v1.12.0)
- [Kanata 1.12.0 configuration guide](https://github.com/jtroo/kanata/blob/v1.12.0/docs/config.adoc): `fork`, Shift chords, explicit `defsrc`, emergency exit
- [Kanata 1.12.0 macOS setup and removal](https://github.com/jtroo/kanata/blob/v1.12.0/docs/setup-macos.md)
- [Kanata platform limitations](https://github.com/jtroo/kanata/blob/v1.12.0/docs/platform-known-issues.adoc)
- [Karabiner: add your own rule](https://karabiner-elements.pqrs.org/docs/manual/configuration/add-your-own-complex-modifications/),
  [modifier semantics](https://karabiner-elements.pqrs.org/docs/json/complex-modifications-manipulator-definition/from/modifiers/),
  [rule priority](https://karabiner-elements.pqrs.org/docs/json/complex-modifications-manipulator-evaluation-priority/)
- [KMonad installation and platform backends](https://github.com/kmonad/kmonad/blob/master/doc/installation.md)
- [AutoHotkey platform and license](https://www.autohotkey.com/)
