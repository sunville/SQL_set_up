# SQLite Installation Guide

[← Back to SQLite Database Tutorial](../SQLite%20Database%20Tutorial%20with%20Navicat.md)

This guide provides detailed instructions for installing SQLite on different operating systems. SQLite is a self-contained, serverless, zero-configuration, transactional SQL database engine widely used for embedded applications and development.

## Table of Contents
- [Introduction to SQLite](#introduction-to-sqlite)
- [Installation on Windows](#installation-on-windows)
- [Installation on macOS](#installation-on-macos)
- [Installation on Linux](#installation-on-linux)
- [Verifying Your Installation](#verifying-your-installation)
- [Installing SQLite Extensions](#installing-sqlite-extensions)
- [Common Installation Issues](#common-installation-issues)

## Introduction to SQLite

SQLite is a C library that provides a lightweight, disk-based database that doesn't require a separate server process. The entire database is stored in a single cross-platform file on disk, making it incredibly portable and simple to set up.

Key advantages of SQLite:
- **Zero configuration** - requires no setup or administration
- **Serverless** - doesn't require a separate server process
- **Self-contained** - minimal dependencies
- **Small footprint** - the entire library can be less than 600KB
- **Highly reliable** - extensively tested
- **Cross-platform** - works on virtually all operating systems

## Installation on Windows

### Using the Precompiled Binaries

1. **Download SQLite tools**:
   - Visit the [SQLite Download Page](https://www.sqlite.org/download.html)
   - Under "Precompiled Binaries for Windows", download:
     - `sqlite-tools-win32-x86-*.zip` (command-line tools)
     - `sqlite-dll-win32-x86-*.zip` (DLL files, if needed for development)

2. **Extract the files**:
   - Create a folder for SQLite (e.g., `C:\sqlite`)
   - Extract the downloaded ZIP file(s) to this folder

3. **Add to PATH environment variable**:
   - Right-click on "This PC" or "Computer" and select "Properties"
   - Click on "Advanced system settings"
   - Click "Environment Variables"
   - Under "System variables", select "Path" and click "Edit"
   - Click "New" and add the path to your SQLite folder (e.g., `C:\sqlite`)
   - Click "OK" on all dialogs to save changes

### Using Chocolatey Package Manager

If you have [Chocolatey](https://chocolatey.org/) installed, you can simply run:

```
choco install sqlite
```

### Using Windows Subsystem for Linux (WSL)

For Windows 10/11 users with WSL enabled:

1. Open WSL terminal
2. Run the Linux installation commands (see the Linux section below)

## Installation on macOS

### Using Homebrew (Recommended)

macOS often comes with SQLite pre-installed, but you can install the latest version using Homebrew:

1. **Install Homebrew** (if not already installed):
   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```

2. **Install SQLite**:
   ```bash
   brew install sqlite
   ```

3. **Verify installation**:
   ```bash
   sqlite3 --version
   ```

### Using MacPorts

If you use MacPorts instead of Homebrew:

```bash
sudo port install sqlite3
```

### Manual Installation

1. **Download SQLite source code**:
   - Visit the [SQLite Download Page](https://www.sqlite.org/download.html)
   - Download the latest source code (e.g., `sqlite-autoconf-*.tar.gz`)

2. **Compile and install**:
   ```bash
   tar -xzvf sqlite-autoconf-*.tar.gz
   cd sqlite-autoconf-*
   ./configure
   make
   sudo make install
   ```

## Installation on Linux

### Debian/Ubuntu

SQLite is usually pre-installed on most Linux distributions. To install or update:

```bash
wsl sudo apt update
wsl sudo apt install sqlite3 libsqlite3-dev
```

### Red Hat/Fedora/CentOS

```bash
wsl sudo dnf install sqlite sqlite-devel
```

### Arch Linux

```bash
wsl sudo pacman -S sqlite
```

### Compiling from Source

For the latest version or custom compilation:

1. **Download source code**:
   ```bash
   wsl wget https://www.sqlite.org/[year]/sqlite-autoconf-[version].tar.gz
   ```

2. **Compile and install**:
   ```bash
   wsl tar -xzvf sqlite-autoconf-[version].tar.gz
   wsl cd sqlite-autoconf-[version]
   wsl ./configure
   wsl make
   wsl sudo make install
   ```

## Verifying Your Installation

Verify that SQLite is correctly installed by running:

```bash
sqlite3 --version
```

You should see output showing the SQLite version number.

To test basic functionality, create a test database:

```bash
sqlite3 test.db
```

This will open the SQLite shell. Try a few basic commands:

```sql
-- Create a simple table
CREATE TABLE test (id INTEGER PRIMARY KEY, name TEXT);

-- Insert data
INSERT INTO test (name) VALUES ('Hello World');

-- Query data
SELECT * FROM test;

-- Exit SQLite
.exit
```

## Installing SQLite Extensions

SQLite's functionality can be extended with loadable extensions.

### SQLite JSON Extension

The JSON extension allows you to handle JSON data in SQLite:

1. **Windows**: 
   - Download precompiled JSON extension or compile from source
   - Load it when needed with:
   ```sql
   .load path\to\json1.dll
   ```

2. **macOS/Linux**:
   - The JSON extension is usually included with SQLite 3.9+
   - Enable it with:
   ```sql
   .load /usr/lib/sqlite3/libjson1.so
   ```

### Other Useful Extensions

- **FTS5**: Full-text search extension
- **R-Tree**: Spatial indexing
- **Math Functions**: Advanced mathematical operations

To use extensions, load them in your SQLite session:

```sql
.load extension_file_path
```

## Common Installation Issues

### "Command Not Found" Error

**Problem**: `sqlite3` command is not recognized.

**Solution**:
- Ensure SQLite is properly installed
- Check that the SQLite directory is in your PATH
- On Windows, try restarting your command prompt after updating PATH

### Permission Issues on Linux/macOS

**Problem**: Unable to install due to permission errors.

**Solution**:
- Use `sudo` for installation commands
- Check file permissions in installation directory
- Ensure you have write access to the destination folder

### Library Load Failures

**Problem**: SQLite is installed but fails with library dependency errors.

**Solution**:
- Install required dependencies (e.g., `libsqlite3-dev` on Debian/Ubuntu)
- Check for conflicting library versions
- Ensure correct library path configuration

### Compilation Errors

**Problem**: Errors when compiling from source.

**Solution**:
- Install required build tools (e.g., gcc, make)
- Check for compiler error details
- Ensure you have the necessary development packages

## Next Steps

Now that you have SQLite installed, you can:

- [Learn how to run SQLite](../Running_SQLite/README.md)
- [Connect to SQLite using Navicat](../Connecting_with_Navicat/README.md)
- [Load data into your SQLite database](../Loading_Data/README.md)

For more information, refer to the [official SQLite documentation](https://www.sqlite.org/docs.html). 