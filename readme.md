# MCP (Model Context Protocol) Complete Setup Guide

**🎯 Production-Ready | Tested on EC2 & Ubuntu Systems**

---

## 🚀 Quick Start (One-Command Installation)

**For those who want to get started FAST** - Copy and paste this entire script:

```bash
#!/bin/bash
# Complete MCP Installation - TESTED ON EC2 & UBUNTU
# Author: indianic | For Personal & Followers Use

echo "========================================="
echo "MCP Installation Script"
echo "Tested on EC2 & Ubuntu Systems"
echo "========================================="

# Update system
echo "[1/9] Updating system..."
sudo apt update

# Install essential tools
echo "[2/9] Installing essential tools..."
sudo apt install -y curl wget unzip git jq ca-certificates gnupg lsb-release

# Install Python3 and pip
echo "[3/9] Installing pip (Python3 is pre-installed)..."
sudo apt install python3-pip -y
python3 --version
pip3 --version

# Install AWS CLI
echo "[4/9] Installing AWS CLI..."
sudo apt install awscli -y
# if the about cmd throws error, then try sudo snap install aws-cli --classic
aws --version

# Install UV Package Manager
echo "[5/9] Installing UV Package Manager..."
curl -LsSf https://astral.sh/uv/install.sh | sh
export PATH="$HOME/.local/bin:$PATH"
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
uv --version

# Install Amazon Q CLI
echo "[6/9] Installing Amazon Q CLI..."
wget https://desktop-release.q.us-east-1.amazonaws.com/latest/amazon-q.deb
sudo dpkg -i amazon-q.deb
sudo apt-get install -f -y
#rm amazon-q.deb  -- not required
q --version

# Install VS Code
echo "[7/9] Installing VS Code..."
sudo snap install code --classic
code --version

# Create directory structure
echo "[8/9] Creating directory structure..."
mkdir -p ~/.aws/amazonq  
#mkdir -p ~/.aws/amazonnq
#mkdir -p ~/.config/Code/User
mkdir -p ~/aws-docs-mcp

echo "[Optional] Creating default MCP config (~/.aws/amazonq/mcp.json)..."
cat > ~/.aws/amazonq/mcp.json << 'EOF'
{
  "mcpServers": {
    "awslabs.aws-documentation-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.aws-documentation-mcp-server@latest"],
      "env": {
        "FASTMCP_LOG_LEVEL": "ERROR",
        "AWS_DOCUMENTATION_PARTITION": "aws"
      },
      "disabled": false,
      "autoApprove": []
    }
  }
}
EOF
echo "[9/9] Installation Complete!"
echo ""
echo "========================================="
echo "Next Steps:"
echo "1. aws configure  # Setup AWS credentials"
echo "2. q login        # Authenticate Builder ID"
echo "3. Create MCP configs (see below)"
echo "4. q chat         # Start using!"
echo "========================================="
```

---

## 📋 Table of Contents

1. [What is MCP?](#what-is-mcp)
2. [Prerequisites](#prerequisites)
3. [Step-by-Step Installation](#step-by-step-installation)
4. [Configuration](#configuration)
5. [Usage](#usage)
6. [Troubleshooting](#troubleshooting)
7. [Real-World Tested Commands](#real-world-tested-commands)

---

## What is MCP?

**MCP (Model Context Protocol)** allows AI assistants to access external data sources and tools. This guide sets up:

- **AWS Documentation MCP Server** - Access AWS docs
- **Cost Explorer MCP Server** - Check AWS costs
- **ECS MCP Server** - Manage ECS services

---

## Prerequisites

### System Requirements
- Ubuntu 20.04+ (or Debian-based Linux)
- Internet connection
- Terminal access
- Email address (for Builder ID)

### What You'll Install
1. Amazon Q CLI - Primary interface
2. AWS CLI - AWS authentication
3. Python 3 & pip - Runtime environment
4. UV Package Manager - Runs MCP servers
5. VS Code - IDE for hosting MCP servers

---

## Step-by-Step Installation

### Step 1: Update System & Install Essentials

```bash
# Update package list
sudo apt update

# Install essential tools
sudo apt install -y curl wget unzip git jq ca-certificates gnupg lsb-release

# Verify
curl --version
wget --version
```

---

### Step 2: Install Python & Pip

```bash
# Python3 is pre-installed on Ubuntu, just install pip
sudo apt update
sudo apt install python3-pip -y

# Verify
python3 --version
pip3 --version
```

---

### Step 3: Install AWS CLI

```bash
# Method 1: Using apt (Tested on EC2)
sudo apt update
sudo apt install awscli -y

# Verify
aws --version

# Method 2: Using snap (Alternative)
# sudo snap install aws-cli --classic
```

---

### Step 4: Configure AWS Credentials

```bash
# Run AWS configure
aws configure

# You'll be prompted for:
# - AWS Access Key ID
# - AWS Secret Access Key
# - Default region (e.g., us-east-1)
# - Default output format (json)

# Test configuration
aws sts get-caller-identity
```

---

### Step 5: Install UV Package Manager

```bash
# Install UV
curl -LsSf https://astral.sh/uv/install.sh | sh

# Add to PATH
export PATH="$HOME/.local/bin:$PATH"

# Make permanent
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc

# Verify
uv --version
uvx --help
```

---

### Step 6: Install Amazon Q CLI

```bash
# Download Amazon Q package (TESTED)
wget https://desktop-release.q.us-east-1.amazonaws.com/latest/amazon-q.deb

# Install
sudo dpkg -i amazon-q.deb

# Fix dependencies if any
sudo apt-get install -f

# Verify
q --version

# Clean up
rm amazon-q.deb
```

---

### Step 7: Install VS Code

```bash
# Install VS Code using snap
sudo snap install code --classic

# Verify
code --version

# Alternative method (if snap doesn't work):
# wget https://code.visualstudio.com/sha/download?build=stable&os=linux-deb-x64 -O code.deb
# sudo dpkg -i code.deb
# sudo apt-get install -f
```

---

### Step 8: Create Directory Structure

```bash
# Create AWS config directory
mkdir -p ~/.aws

# Create Amazon Q config directories
mkdir -p ~/.aws/amazonq
mkdir -p ~/.aws/amazonnq

# Create VS Code config directory
mkdir -p ~/.config/Code/User

# Create project directory
mkdir -p ~/aws-docs-mcp

# Verify
ls -la ~/.aws/
ls -la ~/.config/Code/User/
```

---

### Step 9: Setup Builder ID & Login

**⚠️ CRITICAL STEP - Without this, q chat won't work!**

```bash
# 1. Create Builder ID account
# Visit: https://aws.amazon.com/builder-id/
# - Click "Sign up for free"
# - Enter your email and create password
# - Verify your email
# - Complete registration

# 2. Login to Amazon Q CLI
q login
# This will open a browser window
# Sign in with your Builder ID credentials

# 3. Test authentication
q chat
# Should open the chat interface
```

---

## Configuration

### Create MCP Configuration File

**File**: `~/.aws/amazonq/mcp.json`

```bash
# Create the configuration file
cat > ~/.aws/amazonq/mcp.json << 'EOF'
{
  "mcpServers": {
    "awslabs.aws-documentation-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.aws-documentation-mcp-server@latest"],
      "env": {
        "FASTMCP_LOG_LEVEL": "ERROR",
        "AWS_DOCUMENTATION_PARTITION": "aws"
      },
      "disabled": false,
      "autoApprove": []
    },
    "awslabs.cost-explorer-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.cost-explorer-mcp-server@latest"],
      "env": {
        "FASTMCP_LOG_LEVEL": "ERROR",
        "AWS_PROFILE": "default"
      },
      "disabled": false,
      "autoApprove": []
    }
  }
}
EOF

# Verify the file
cat ~/.aws/amazonq/mcp.json | jq .
```

### Create VS Code MCP Configuration

**File**: `~/.config/Code/User/mcp.json`

```bash
# Create VS Code MCP config
cat > ~/.config/Code/User/mcp.json << 'EOF'
{
    "servers": {
        "AWS Documentation MCP Server": {
            "command": "uvx",
            "args": ["awslabs.aws-documentation-mcp-server@latest"],
            "env": {
                "FASTMCP_LOG_LEVEL": "ERROR",
                "AWS_DOCUMENTATION_PARTITION": "aws"
            },
            "disabled": false,
            "autoApprove": [],
            "type": "stdio"
        },
        "Cost Explorer MCP Server": {
            "command": "uvx",
            "args": ["awslabs.cost-explorer-mcp-server@latest"],
            "env": {
                "AWS_PROFILE": "default",
                "AWS_REGION": "us-east-1",
                "FASTMCP_LOG_LEVEL": "ERROR"
            },
            "disabled": false,
            "autoApprove": [],
            "type": "stdio"
        },
        "ECS MCP Server": {
            "command": "uvx",
            "args": ["--from", "awslabs-ecs-mcp-server", "ecs-mcp-server"],
            "env": {
                "AWS_PROFILE": "default",
                "AWS_REGION": "us-east-1",
                "FASTMCP_LOG_LEVEL": "ERROR",
                "ALLOW_WRITE": "false",
                "ALLOW_SENSITIVE_DATA": "false"
            },
            "type": "stdio"
        }
    },
    "inputs": []
}
EOF

# Verify
cat ~/.config/Code/User/mcp.json | jq .
```

---

## Usage

### How to Use MCP

```bash
# 1. Start VS Code (it will load MCP servers)
code

# 2. In VS Code, check the extensions section
# You should see:
# - AWS Documentation MCP Server
# - Cost Explorer MCP Server
# - ECS MCP Server

# 3. Start the servers by clicking on them

# 4. Open a terminal and start Amazon Q
q chat

# 5. Now you can ask questions!
# Examples:
# - "What is Amazon S3?"
# - "Show me my AWS costs this month"
# - "List my ECS services"
```

### Amazon Q CLI Commands

```bash
# Start chat
q chat

# Login/authenticate
q login

# Check settings
q settings

# Get help
q help

# Check version
q --version

# Diagnose issues
q doctor

# Translate natural language to shell
q translate

# Quit
q quit
```

### Test MCP Servers

```bash
# Test AWS Documentation server
uvx awslabs.aws-documentation-mcp-server@latest --help

# Test Cost Explorer server
uvx awslabs.cost-explorer-mcp-server@latest --help

# Test ECS server
uvx --from awslabs-ecs-mcp-server ecs-mcp-server --help
```

---

## Troubleshooting

### Issue 1: "q chat" doesn't work

**Solution**: Authenticate with Builder ID
```bash
q login
# Follow browser prompts
```

### Issue 2: Dependencies missing after Amazon Q installation

**Solution**: Fix dependencies
```bash
sudo apt-get install -f
```

### Issue 3: uvx command not found

**Solution**: Add to PATH and reload
```bash
export PATH="$HOME/.local/bin:$PATH"
source ~/.bashrc
uvx --version
```

### Issue 4: MCP servers not loading in VS Code

**Solutions**:
```bash
# Check config syntax
cat ~/.config/Code/User/mcp.json | jq .

# Test server individually
uvx awslabs.aws-documentation-mcp-server@latest --help

# Check VS Code logs
ls -la ~/.config/Code/logs/
```

### Issue 5: AWS authentication errors

**Solution**: Verify AWS credentials
```bash
# Check configuration
aws configure list

# Test credentials
aws sts get-caller-identity

# Reconfigure if needed
aws configure
```

### Complete System Check

```bash
# Check all installations
echo "=== System Check ==="
echo "Q CLI: $(q --version)"
echo "AWS CLI: $(aws --version)"
echo "Python: $(python3 --version)"
echo "Pip: $(pip3 --version)"
echo "UV: $(uv --version)"
echo "VS Code: $(code --version | head -1)"

# Check AWS auth
echo ""
echo "AWS Authentication:"
aws sts get-caller-identity

# Check configs
echo ""
echo "Configuration Files:"
ls -la ~/.aws/amazonq/mcp.json && echo "✓ Amazon Q config" || echo "✗ Missing"
ls -la ~/.config/Code/User/mcp.json && echo "✓ VS Code config" || echo "✗ Missing"
```

---

## Real-World Tested Commands

### Tested on EC2 Instance (Ubuntu)

```bash
# Complete installation sequence
wget https://desktop-release.q.us-east-1.amazonaws.com/latest/amazon-q.deb
sudo dpkg -i amazon-q.deb
sudo apt-get install -f

sudo apt update
sudo apt install awscli -y

sudo apt install python3-pip -y
python3 --version
pip3 --version

curl -LsSf https://astral.sh/uv/install.sh | sh
export PATH="$HOME/.local/bin:$PATH"
source ~/.bashrc
uv --version

sudo snap install code --classic
code --version

sudo apt install -y curl wget unzip git jq ca-certificates gnupg lsb-release

mkdir -p ~/.aws
mkdir -p ~/.aws/amazonq
mkdir -p ~/.aws/amazonnq

vi ~/.aws/amazonq/mcp.json

q login
q chat
```

### Tested on Personal System

```bash
# Alternative AWS CLI install
sudo snap install aws-cli --classic

# Download Amazon Q
wget https://desktop-release.q.us-east-1.amazonaws.com/latest/amazon-q.deb

# Configuration
cd ~/.aws/amazonq/
vi mcp.json

# Usage
q login
q chat
```

### Minimal Working Configuration

**File**: `~/.aws/amazonq/mcp.json`
```json
{
  "mcpServers": {
    "awslabs.aws-documentation-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.aws-documentation-mcp-server@latest"],
      "env": {
        "FASTMCP_LOG_LEVEL": "ERROR",
        "AWS_DOCUMENTATION_PARTITION": "aws"
      },
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

---

## For My Followers 💡

This guide is based on **real production testing** on:
- ✅ **EC2 Ubuntu instances**
- ✅ **Personal Ubuntu systems**

All commands have been **verified to work**. If you follow this step-by-step, you **will** get a working MCP setup.

### Pro Tips:

1. **Always run `sudo apt update`** before installing packages
2. **Use `q login`** for authentication (easiest method)
3. **Test each component** with version commands
4. **Start VS Code first**, then run `q chat` in a separate terminal
5. **Check configuration files** with `jq` to validate JSON syntax

### Common Mistakes to Avoid:

❌ Not creating directory structure first
❌ Skipping Builder ID setup
❌ Not adding UV to PATH
❌ Missing AWS credentials configuration
❌ Not fixing dependencies after dpkg install

### Quick Checklist:

- [ ] System updated (`sudo apt update`)
- [ ] Essential tools installed
- [ ] Python3 & pip installed
- [ ] AWS CLI installed and configured
- [ ] UV Package Manager installed
- [ ] Amazon Q CLI installed
- [ ] VS Code installed
- [ ] Directories created
- [ ] Builder ID account created
- [ ] Logged in via `q login`
- [ ] MCP config files created
- [ ] Tested with `q chat`

---

## Additional Resources

### Important URLs:
- **Builder ID Registration**: https://aws.amazon.com/builder-id/
- **Amazon Q Download**: https://desktop-release.q.us-east-1.amazonaws.com/latest/amazon-q.deb
- **UV Package Manager**: https://astral.sh/uv

### Configuration Locations:
- Amazon Q MCP: `~/.aws/amazonq/mcp.json`
- VS Code MCP: `~/.config/Code/User/mcp.json`
- AWS Credentials: `~/.aws/credentials`
- AWS Config: `~/.aws/config`

### Log Locations:
- VS Code MCP Logs: `~/.config/Code/logs/[DATE]/window[N]/mcpServer.*.log`
- Amazon Q Logs: Check with `q doctor`

---

## Need Help?

If you encounter issues:

1. **Run diagnostics**: `q doctor`
2. **Check this troubleshooting section** above
3. **Verify all installations** with version commands
4. **Check configuration files** for syntax errors
5. **Ensure Builder ID is configured** with `q login`

---

**📅 Last Updated**: October 6, 2025  
**👤 Author**: indianic  
**✅ Status**: Production-Ready & Tested  
**🎯 Tested On**: EC2 Ubuntu & Personal Ubuntu Systems  
**📦 MCP Servers**: AWS Documentation, Cost Explorer, ECS

---

**⭐ This documentation is for personal use and followers. Feel free to share!**

