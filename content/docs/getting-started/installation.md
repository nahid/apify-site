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

For a quick installation via your command line, use the following platform-specific instructions. Remember to replace `[DOWNLOAD_URL]` with the actual download link for your OS and architecture from the [latest GitHub release](https://github.com/nahid/apify/releases/latest).

#### Linux & macOS

```bash
# Download the appropriate zip file
curl -L [DOWNLOAD_URL] -o apify.zip

# Extract the binary
unzip apify.zip
# This will extract 'apify' binary (and potentially other files) to the current directory.
# Adjust 'apify' if the extracted binary has a different name (e.g., apify-linux-x64)

# Make the binary executable and move it to /usr/local/bin
sudo chmod a+x apify
sudo mv apify /usr/local/bin/

# For macOS users: Remove Quarantine Attribute (if downloaded via browser)
xattr -d com.apple.quarantine /usr/local/bin/apify

# Verify installation
apify --version
```

#### Windows (PowerShell)

```powershell
# Define download URL and installation path
$downloadUrl = "[DOWNLOAD_URL]"
$installPath = "$env:ProgramFiles\Apify"

# Create directory
New-Item -ItemType Directory -Force -Path $installPath

# Download and extract
Invoke-WebRequest -Uri $downloadUrl -OutFile "$env:TEMP\apify.zip"
Expand-Archive -Path "$env:TEMP\apify.zip" -DestinationPath $installPath -Force

# Add to PATH (for current session, add permanently via System Properties for persistence)
$env:Path += ";$installPath"

# Verify installation
apify --version
```

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
