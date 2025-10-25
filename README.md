# SaLangDev — Complete README (Install, Add to PATH, Run in VS Code/CMD, Examples)

> This README explains how to install the prebuilt **`salangdev.exe`** binary (or installer), add it to the Windows PATH, run SaLangDev from Command Prompt and Visual Studio Code, and shows ready-to-run example programs with expected output (including a simple `Hello` program).

---

## Contents

- [About](#about)  
- [Download / Installation ](#download--installation)  
- [Add `salangdev.exe` to your PATH (Windows)](#add-salangdevexe-to-your-path-windows)  
- [Run from Command Prompt (CMD)](#run-from-command-prompt-cmd)  
- [Run from PowerShell](#run-from-powershell)  
- [Run inside Visual Studio Code (Tasks & Launch)](#run-inside-visual-studio-code-tasks--launch)  
- [Examples & expected output](#examples--expected-output)  
- [CLI reference (basic)](#cli-reference-basic)  
- [Troubleshooting](#troubleshooting)  
- [Publishing / Distributing a single `.exe` (short notes)](#publishing--distributing-a-single-exe-short-notes)  
- [License & contact](#license--contact)

---

## About

**SaLangDev** is distributed to end users as a single native Windows executable called `salangdev.exe`. This README assumes you will use the `.exe` (or an official Windows installer which installs the `.exe`) so **Development environment is required** to run programs.

If you want the exact developer/build process, see the project repo's developer docs — this file focuses strictly on installing, adding to PATH, running, and usage examples.

---

## Download / Installation

1. Open the repository's **Releases** page on GitHub or the `releases/` folder in the repo.  
2. Download one of the following assets (choose the easiest for you):  
   - `salangdev.exe` — a single, portable executable you can run directly.  
   - `SaLangDev-Installer-<version>.exe` — an installer produced with Inno Setup. Run it to install to `C:\Program Files\SaLangDev` (default) or another folder.

3. If you downloaded `salangdev.exe` directly, save it into a permanent folder, for example:

```
C:\Tools\SaLangDev\salangdev.exe
```

4. If Windows warns about the file being from the internet, right-click the file → Properties → click **Unblock** if present, then Apply → OK.

---

## Add `salangdev.exe` to your PATH (Windows)

Adding the folder that contains `salangdev.exe` to your `PATH` lets you run it from any command prompt simply by typing `salangdev`.

### Option A — GUI (recommended)

1. Move `salangdev.exe` to a folder you want to use, e.g. `C:\Tools\SaLangDev`.  
2. Press **Start** → type **Edit environment variables** → choose **Edit environment variables for your account**.  
3. In the **User variables** box, select `Path` → click **Edit**.  
4. Click **New** and paste `C:\Tools\SaLangDev` (or whatever folder you chose).  
5. Click **OK** in all dialogs.  
6. Open a **new** Command Prompt or VS Code terminal to pick up the change.

### Option B — Command line (CMD)

Use `setx` to append the folder to your user PATH. Note: `setx` writes a new PATH value and truncation may occur if PATH becomes very long — prefer the GUI when possible.

```cmd
setx PATH "%PATH%;C:\Tools\SaLangDev"
```

After running `setx`, close and reopen terminals.

### Option C — PowerShell (recommended for scripts)

```powershell
[Environment]::SetEnvironmentVariable("Path", $env:Path + ";C:\Tools\SaLangDev", "User")
```

Then open a new terminal.

---

## Run from Command Prompt (CMD)

Open a new Command Prompt window (type `cmd` in Start) and run:

```cmd
salangdev --help
```

To run a source file:

```cmd
salangdev path\to\program.salangdev
```

If `salangdev` is on your PATH you can omit the full path.

Examples below show complete sample programs and expected output.

---

## Run from PowerShell

Open a new PowerShell terminal and run the same commands:

```powershell
salangdev --help
salangdev .\examples\hello.salangdev
```

If PowerShell blocks execution of downloaded files, unblock the file in Explorer or run the `Unblock-File` cmdlet:

```powershell
Unblock-File -Path C:\Tools\SaLangDev\salangdev.exe
```

---

## Run inside Visual Studio Code (Tasks & Launch)

You can configure VS Code to run the current SaLangDev file with a Task, and attach a Launch configuration for debugging-style workflows.

### 1) Task to run the active file

Create or update `.vscode/tasks.json` in your workspace root with this task (adjust the path to `salangdev.exe` if needed):

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

How to run the task:  
- Press **Ctrl+Shift+P** → type **Tasks: Run Task** → choose **Run SaLangDev: Current File**.

### 2) Optional: Debug/Launch configuration that runs the active file in an integrated console

Create `.vscode/launch.json` and add:

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Run SaLangDev (current file)",
      "type": "pwa-extensionHost",
      "request": "launch",
      "runtimeExecutable": "salangdev",
      "args": ["${file}"],
      "console": "integratedTerminal"
    }
  ]
}
```

> Note: `runtimeExecutable` is `salangdev` (because it's on PATH). If you did not add `salangdev` to PATH, replace it with the full path to the executable, e.g. `C:\Tools\SaLangDev\salangdev.exe`.

---

## Examples & expected output

Below are ready-to-run example programs. Save each as a text file with the extension `.salangdev` and run with `salangdev path\to\file.salangdev`.

### Example 1 — Hello (single-line print)

**File:** `hello.salangdev`

```
print "Hello"
```

**Run:**

```cmd
salangdev examples\hello.salangdev
```

**Expected output (stdout):**

```
Hello
```

---

### Example 2 — Hello, name input (simple interaction)

**File:** `greet.salangdev`

```
print "Enter your name:"
name = input()
print "Hello, " + name
```

**Run:**

```cmd
salangdev examples\greet.salangdev
```

**Example session:**

```
Enter your name:
> Salman
Hello, Salman
```

---

### Example 3 — Loop & arithmetic (sum 1..5)

**File:** `sum.salangdev`

```
sum = 0
for i in 1..5:
  sum = sum + i
print "Sum = " + sum
```

**Run:**

```cmd
salangdev examples\sum.salangdev
```

**Expected output:**

```
Sum = 15
```

> These examples are intentionally small and show typical language features: printing, input, variables, loops, and arithmetic. Adapt them to the real language syntax if your interpreter differs slightly.

---

## CLI reference (basic)

A minimal, user-friendly CLI that most users expect is documented here. If your `salangdev.exe` uses different flags, replace these with your actual flags.

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
- You likely did not add the folder containing `salangdev.exe` to PATH or you have not opened a new terminal since adding it. Add the folder to PATH (see above) and open a new terminal.

**The exe exits immediately when double-clicked in Explorer**  
- Many CLI programs expect to be run from a terminal. Open `cmd.exe` or PowerShell and run `salangdev file.salangdev` to see program output and errors.

**PowerShell prevents running the executable**  
- Use `Unblock-File` on the executable or unblock it in the file Properties.

**Installer or exe shows SmartScreen warning**  
- This is a Windows protective feature, especially for new or unsigned binaries. Provide the binary via GitHub Releases and include clear release notes. For broad distribution, consider code signing.

---

## Publishing / Distributing a single `.exe` (short notes)

- Upload your `salangdev.exe` or the Inno Setup installer to GitHub Releases.  
- Attach release notes and example files (`examples/hello.salangdev`, etc.).  
- Provide an install section in your release description and a direct download link to the `exe` asset.

---

## License & contact

Add your chosen license file (`LICENSE`) in the repository root (MIT, Apache-2.0, etc.). For support or to request README updates, open an Issue in the repository or contact the maintainer listed in `package.json` or the repo profile.


