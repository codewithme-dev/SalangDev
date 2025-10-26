# SaLangDev — Complete README (Install, Add to PATH, Run in VS Code/CMD, Examples)

> This README explains how to install the prebuilt **`salangdev.exe`** binary (or installer), add it to the Windows PATH, run SaLangDev from Command Prompt and Visual Studio Code, and includes ready-to-run example programs with expected output.

---

## Contents

* [About](#about)
* [Download / Installation](#download--installation)
* [Add `salangdev.exe` to your PATH (Windows)](#add-salangdevexe-to-your-path-windows)
* [Run from Command Prompt (CMD)](#run-from-command-prompt-cmd)
* [Run from PowerShell](#run-from-powershell)
* [Run inside Visual Studio Code (Tasks & Launch)](#run-inside-visual-studio-code-tasks--launch)
* [Examples & expected output](#examples--expected-output)
* [CLI reference (basic)](#cli-reference-basic)
* [Troubleshooting](#troubleshooting)
* [Publishing / Distributing a single `.exe` (short notes)](#publishing--distributing-a-single-exe-short-notes)
* [License & contact](#license--contact)

---

## About

**SaLangDev** is distributed as a single native Windows executable called `salangdev.exe` (or via an Inno Setup installer that places the `.exe` into `C:\Program Files\SaLangDev`). This README focuses on installation, adding the executable to PATH, running programs from terminals and VS Code, and includes sample programs you can copy and run immediately.

> ⚠️ If you share `salangdev.exe` publicly, consider code-signing and including clear release notes to reduce SmartScreen or antivirus warnings.

---

## Download / Installation

1. Open the repository's **Releases** page on GitHub (or the `releases/` folder in the repo).

2. Download one of the release assets (pick the easiest for you):

   * `salangdev.exe` — a single, portable executable.
   * `SaLangDev-Installer-<version>.exe` — Inno Setup installer (recommended for non-technical users).

3. If you downloaded `salangdev.exe` directly, place it in a permanent folder such as:

```
C:\Tools\SaLangDev\salangdev.exe
```

4. If Windows shows the file as downloaded from the internet, right-click → Properties → **Unblock** (if present) → Apply → OK.

---

## Add `salangdev.exe` to your PATH (Windows)

Adding the folder containing `salangdev.exe` to your PATH allows running it from any terminal by typing `salangdev`.

### Option A — GUI (recommended)

1. Move `salangdev.exe` to a folder you want, e.g. `C:\Tools\SaLangDev`.
2. Press **Start** → type **Edit environment variables** → choose **Edit environment variables for your account**.
3. In **User variables** select `Path` → click **Edit** → **New** → paste `C:\Tools\SaLangDev`.
4. Click **OK**, then open a **new** terminal to pick up the change.

### Option B — Command Prompt (quick, not recommended for long PATHs)

```cmd
setx PATH "%PATH%;C:\Tools\SaLangDev"
```

> Note: `setx` writes a new PATH value and may truncate if PATH becomes too long. Use the GUI when possible.

### Option C — PowerShell (scriptable)

```powershell
[Environment]::SetEnvironmentVariable("Path", $env:Path + ";C:\Tools\SaLangDev", "User")
```

Open a new terminal after running this.

---

## Run from Command Prompt (CMD)

Open a new Command Prompt window and run:

```cmd
salangdev --help
```

To run a source file:

```cmd
salangdev path\to\program.salangdev
```

If `salangdev` is on PATH you can omit the full path to the executable.

---

## Run from PowerShell

Open PowerShell and run the same commands:

```powershell
salangdev --help
salangdev .\examples\hello.salangdev
```

If PowerShell blocks execution of downloaded files, unblock the executable:

```powershell
Unblock-File -Path C:\Tools\SaLangDev\salangdev.exe
```

---

## Run inside Visual Studio Code (Tasks & Launch)

You can add a Task to run the current SaLangDev file and an optional Launch configuration.

### 1) Task to run the active file

Create `.vscode/tasks.json` in your workspace root with the task below. If `salangdev` is not on PATH, replace `salangdev` with the full path `C:\Tools\SaLangDev\salangdev.exe`.

```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "Run SaLangDev: Current File",
      "type": "shell",
      "command": "salangdev ${file}",
      "presentation": {
        "reveal": "always",
        "panel": "shared"
      },
      "problemMatcher": []
    }
  ]
}
```

Run it: **Ctrl+Shift+P** → *Tasks: Run Task* → choose **Run SaLangDev: Current File**.

### 2) Optional: Launch configuration (integrated console)

Create `.vscode/launch.json` and add (adjust `runtimeExecutable` if not on PATH):

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Run SaLangDev (current file)",
      "type": "pwa-node",
      "request": "launch",
      "runtimeExecutable": "salangdev",
      "args": ["${file}"],
      "console": "integratedTerminal"
    }
  ]
}
```

> Note: VS Code's `type` may need adjusting depending on how you prefer to run and debug. The above runs the executable in the integrated terminal which is usually sufficient for printing output and seeing errors.

---

## Examples & expected output

Save each example as a `.salangdev` file and run with `salangdev path\to\file.salangdev`.

> These examples use two different print keywords you showed (`outcome` and `print`). Replace `print` with `outcome` or vice-versa to match your interpreter.


### Example — Full typed example (First.salangdev)

**File:** `examples/First.salangdev`

```
set x digit = 10
set y digit = 5

set name letters = "Salman Fareed"
set grade one_letter = 'A'

outcome "Running SaLangDev Program..."
outcome "Welcome, " + name
outcome "Your grade is: " + grade

outcome "Addition: " + (x + y)
outcome "Subtraction: " + (x - y)
outcome "Multiplication: " + (x * y)
outcome "Division: " + (x / y)

outcome "Summary: " + name + " got grade " + grade + " and scored " + (x + y) + " points."
outcome "Program completed successfully!"
```

**Expected output:**

```
Running SaLangDev Program...
Welcome, Salman Fareed
Your grade is: A
Addition: 15
Subtraction: 5
Multiplication: 50
Division: 2
Summary: Salman Fareed got grade A and scored 15 points.
Program completed successfully!
```

> If your language treats types as strict, ensure concatenation of numbers to strings is supported (or use an explicit `toString()` / print formatting function).

---

## CLI reference (basic)

A minimal CLI that users expect. If your binary uses different flags, update accordingly.

```
Usage: salangdev [options] <source>

Options:
  -h, --help        Show this help message and exit
  -v, --version     Show version
  -o DIR            Write outputs/logs to directory DIR
  --debug           Enable debug logging
```

`salangdev file.salangdev` runs the program and prints stdout/stderr to the console.

---

## Troubleshooting

**`salangdev` is not recognized**

* You likely did not add the folder containing `salangdev.exe` to PATH or you opened a terminal before adding it. Add to PATH and open a new terminal.

**The exe exits immediately when double-clicked in Explorer**

* CLI programs usually need a terminal. Run via `cmd.exe` or PowerShell to see output and errors.

**PowerShell prevents running the executable**

* Use `Unblock-File` or unblock via file Properties.

**Installer or exe shows SmartScreen/antivirus warning**

* This is common for new unsigned binaries. Distribute via GitHub Releases with clear release notes. For wider distribution, obtain a code-signing certificate.

**`setx PATH` truncates the PATH**

* Use the GUI method to edit PATH when you have many PATH entries.

---

## Publishing / Distributing a single `.exe` (short notes)

* Upload `salangdev.exe` or Inno Setup installer to GitHub Releases.
* Attach example files (`examples/hello.salangdev`, `examples/First.salangdev`, etc.).
* Add a clear `RELEASE_NOTES.md` and a `CHANGELOG.md` with version, release date, and highlights.
* For an installer, prefer Inno Setup or NSIS. Include uninstall support and the option to add to PATH during install.

---

## License & contact

Choose and include a license file (`LICENSE`) at the repository root (MIT, Apache-2.0, BSD, etc.).

For support or corrections to this README, open an Issue in the repository or contact the maintainer listed in the repo profile.

