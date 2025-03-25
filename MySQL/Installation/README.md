# MySQL Installation Guide

[← Back to MySQL Database Tutorial](../MySQL%20Database%20Tutorial%20with%20Navicat.md)

This guide provides detailed instructions for installing MySQL on different operating systems. MySQL is one of the world's most popular open-source relational database management systems and is a crucial component for many web applications and services.

## Table of Contents
- [Windows Installation](#windows-installation)
- [macOS Installation](#macos-installation)
- [Linux Installation](#linux-installation)
- [Verifying Your Installation](#verifying-your-installation)
- [Common Installation Issues](#common-installation-issues)

## Windows Installation

### Step 1: Download the MySQL Installer
1. Visit the [MySQL Downloads page](https://dev.mysql.com/downloads/installer/)
2. Download the "MySQL Installer for Windows" (typically the larger file size option for a complete installation)
3. You might need to create an Oracle account or select "No thanks, just start my download" for direct download

### Step 2: Run the Installer
1. Launch the downloaded installer as Administrator
2. Choose a Setup Type:
   - **Developer Default**: Installs the MySQL server, MySQL Shell, MySQL Router, MySQL Workbench, and other development tools
   - **Server Only**: Installs only the MySQL server
   - **Custom**: Allows selection of specific components
   - Recommended: Choose **Developer Default** for a complete setup

### Step 3: Dependency Check
The installer will check for any prerequisites and may download additional components if needed.

### Step 4: Installation Process
1. Click "Execute" to begin installation of selected products
2. Wait for all components to install successfully
3. Click "Next" when all components show "Complete"

### Step 5: Configure MySQL Server
1. In the configuration wizard, select:
   - **Type and Networking**:
     - Config Type: Development Computer (for most users)
     - Connectivity: TCP/IP, keep default port 3306
     - Open Windows Firewall port: Yes
   
   - **Authentication Method**:
     - For newer applications, use the recommended "Strong Password Encryption"
     - For compatibility with older applications, select "Legacy Authentication"
   
   - **Accounts and Roles**:
     - Set the MySQL root password (IMPORTANT: Remember this password!)
     - Optionally, create additional user accounts
   
   - **Windows Service**:
     - Configure MySQL as a Windows service (recommended)
     - Service Name: Leave as "MySQL80" (or similar)
     - Start the service automatically (recommended)
   
   - **Apply Configuration**:
     - Click "Execute" and wait for configuration to complete

### Step 6: MySQL Router Configuration (if installed)
1. Configure the MySQL Router (skip if not needed)
2. Click "Finish" to complete the installation

## macOS Installation

### Method 1: Using the DMG Installer

1. Visit the [MySQL Downloads page](https://dev.mysql.com/downloads/mysql/)
2. Download the DMG file for your macOS version
3. Open the DMG file and run the package installer (.pkg file)
4. Follow the installation wizard prompts
5. **Important**: During installation, a temporary password is generated - write this down!
6. After installation, a MySQL pane appears in System Preferences

### Method 2: Using Homebrew

If you have [Homebrew](https://brew.sh/) installed (recommended for developers):

1. Open Terminal
2. Run the following command:
   ```bash
   brew install mysql
   ```
3. Start MySQL service:
   ```bash
   brew services start mysql
   ```
4. Secure your MySQL installation:
   ```bash
   mysql_secure_installation
   ```
   Follow the prompts to:
   - Set a root password
   - Remove anonymous users
   - Disable remote root login
   - Remove test database
   - Reload privilege tables

## Linux Installation

### Ubuntu/Debian:

1. Update your package index:
   ```bash
   sudo apt update
   ```
2. Install MySQL Server:
   ```bash
   sudo apt install mysql-server
   ```
3. Run the security script:
   ```bash
   sudo mysql_secure_installation
   ```
   Follow the prompts to set up security options including:
   - Validate Password Plugin (recommended)
   - Set root password
   - Remove anonymous users
   - Disable remote root login
   - Remove test database
   - Reload privilege tables

4. Check MySQL service status:
   ```bash
   sudo systemctl status mysql
   ```

### CentOS/RHEL/Fedora:

1. Install MySQL repository:
   ```bash
   sudo dnf install @mysql
   ```
   Or for older systems:
   ```bash
   sudo yum install mysql-server
   ```
2. Start and enable MySQL:
   ```bash
   sudo systemctl start mysqld
   sudo systemctl enable mysqld
   ```
3. Find temporary root password:
   ```bash
   sudo grep 'temporary password' /var/log/mysqld.log
   ```
4. Run security script:
   ```bash
   sudo mysql_secure_installation
   ```

## Verifying Your Installation

1. **Check MySQL Service**:
   - Windows: Open Services (services.msc), find "MySQL80" service
   - macOS: Check System Preferences > MySQL
   - Linux: `sudo systemctl status mysql`

2. **Connect via Command Line**:
   ```bash
   mysql -u root -p
   ```
   Enter the password you set during installation

3. **Run a test command**:
   ```sql
   SHOW DATABASES;
   ```
   You should see a list of default databases

4. **Exit MySQL CLI**:
   ```sql
   EXIT;
   ```

## Common Installation Issues

### Connection Issues
- **Error 1045**: Access denied - Verify username and password
- **Error 2003**: Can't connect to MySQL server - Check if service is running
- **Error 10061**: Connection refused - Check firewall settings

### Password Reset
If you forgot your root password:

**Windows**:
1. Stop MySQL service (services.msc)
2. Open Command Prompt as Administrator
3. Start MySQL with skip-grant-tables:
   ```
   "C:\Program Files\MySQL\MySQL Server 8.0\bin\mysqld.exe" --defaults-file="C:\ProgramData\MySQL\MySQL Server 8.0\my.ini" --init-file=C:\mysql-init.txt
   ```
   (Create mysql-init.txt with: `ALTER USER 'root'@'localhost' IDENTIFIED BY 'NewPassword';`)

**macOS/Linux**:
1. Stop MySQL service:
   ```bash
   sudo systemctl stop mysql
   ```
2. Start MySQL in safe mode:
   ```bash
   sudo mysqld_safe --skip-grant-tables &
   ```
3. Connect without password:
   ```bash
   mysql -u root
   ```
4. Reset password:
   ```sql
   FLUSH PRIVILEGES;
   ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password';
   EXIT;
   ```
5. Restart MySQL service normally

### Installation Fails
- Ensure you have administrative privileges
- Check system requirements
- Verify that no other MySQL instance is running
- Close all applications and try again
- Check Windows Event Viewer or system logs for errors

## Next Steps

Now that you have MySQL installed, you can:
- [Configure and run your MySQL server](../Running_MySQL_Server/README.md)
- [Connect to your MySQL server using Navicat](../Connecting_with_Navicat/README.md)

For more information, refer to the [official MySQL documentation](https://dev.mysql.com/doc/). 