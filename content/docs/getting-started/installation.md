---
title: "Installation"
weight: 1
---

The easiest way to get Apify is to download the pre-built executable from the [GitHub Releases](https://github.com/nahid/apify/releases) page.

1.  Go to the [latest release](https://github.com/nahid/apify/releases/latest).
2.  Download the appropriate `.zip` file for your operating system and architecture (e.g., `apify-win-x64.zip` for Windows 64-bit, `apify-linux-x64.zip` for Linux 64-bit, `apify-osx-arm64.zip` for macOS ARM64).
3.  Extract the contents of the `.zip` file to a directory of your choice (e.g., `C:\Program Files\Apify` on Windows, `/opt/apify` on Linux/macOS).
4.  Add the directory where you extracted Apify to your system's PATH environment variable. This allows you to run `apify` from any terminal.

### CLI Installation

For a quick installation via your command line, use the following platform-specific instructions. See the [latest GitHub release](https://github.com/nahid/apify/releases/latest) for the most up-to-date download links.

#### Linux & macOS

**For Linux Operating Systems:**


```bash
curl -L https://github.com/nahid/apify/releases/latest/download/apify-linux-x64.zip -o apify.zip

```

**For macOS**

```bash
# macOS arm64 (Apple Silicon)
curl -L https://github.com/nahid/apify/releases/latest/download/apify-osx-arm64.zip -o apify.zip

# macOS x64 (Intel)
curl -L https://github.com/nahid/apify/releases/latest/download/apify-osx-x64.zip -o apify.zip
```

Unzip the downloaded file:

```bash
unzip apify.zip -d .
```

This will extract the `apify` binary (and possibly other files) to your current directory.

```bash
# Make the binary executable and move it to /usr/local/bin

sudo chmod a+x ./apify/apify
sudo mv ./apify/apify /usr/local/bin/
```

```bash
# Verify installation
apify --version
```

##### Resolving macOS Security Warnings

On macOS, you may see a security warning when running the binary for the first time (e.g., "cannot be opened because the developer cannot be verified"). To allow execution:

1. Attempt to run `apify` from the terminal. If blocked, note the warning.
2. Open **System Settings** > **Privacy & Security**.
3. Scroll down to the "Security" section. You should see a message about `apify` being blocked.
4. Click **Allow Anyway**.
5. Run `apify` again from the terminal. If prompted, click **Open** in the dialog.

Alternatively, you can remove the quarantine attribute via terminal:

```bash
sudo xattr -rd com.apple.quarantine /usr/local/bin/apify
```

This will allow the binary to run without further security prompts.

Now remove the downloaded zip file and the extracted directory if you no longer need them:

```bash
rm -rf apify.zip apify/
```

#### Windows (PowerShell)

1. Download the appropriate `.zip` file for Windows from the [latest release](https://github.com/nahid/apify/releases/latest) and extract it using File Explorer or a tool like WinRAR. Inside the extracted folder, you'll find `apify.exe`.

2. Create a new folder for Apify in `Program Files` (run PowerShell as Administrator):

   ```powershell
   New-Item -ItemType Directory -Force -Path "$env:ProgramFiles\Apify"
   ```

3. Download the latest release for Windows:

   ```powershell
   Invoke-WebRequest -Uri "https://github.com/nahid/apify/releases/latest/download/apify-win-x64.zip" -OutFile "apify.zip"
   ```

4. Unzip the downloaded file:

   ```powershell
   Expand-Archive -Path "apify.zip" -DestinationPath "."
   ```

5. Move `apify.exe` to the new folder:

   ```powershell
   Move-Item -Path ".\apify\apify.exe" -Destination "$env:ProgramFiles\Apify" -Force
   ```

6. Add Apify to your user PATH environment variable:

   ```powershell
   [Environment]::SetEnvironmentVariable("PATH", $env:PATH + ";C:\Program Files\Apify", "User")
   ```

7. Restart your terminal, then verify the installation:

   ```powershell
   apify --version
   ```

> Ensure you run PowerShell as Administrator for steps that modify `Program Files`.

Alternatively, you can build Apify from source:

### Build from Source

Apify supports Native AOT (Ahead-of-Time) compilation, which produces a self-contained executable with no dependency on the .NET runtime. This results in:

- Faster startup time
- Smaller deployment size
- No dependency on the .NET runtime
- Improved performance

To build the Native AOT version:

```bash
# Using the build script
./build-native.sh

# Or manually
dotnet publish -c Release -r linux-x64 --self-contained true -p:PublishAot=true
```

The resulting executable will be located at:
`bin/Release/net8.0/linux-x64/publish/apify`

You can run it directly without needing the .NET runtime:

```bash
./bin/Release/net8.0/linux-x64/publish/apify
```

#### Platform-Specific Builds

For other platforms, replace `linux-x64` with your target platform:

- Windows: `win-x64`
- macOS: `osx-x64`
- ARM64: `linux-arm64` or `osx-arm64`
