# MCP (Model Context Protocol) Implementation Documentation

## Overview
This document outlines the complete implementation of MCP servers in your system, specifically focusing on AWS-related MCP servers integrated with various development tools and IDEs.

## Implementation Timeline
Based on system logs and configuration files, the MCP implementation was primarily set up around **September 18-25, 2025**.

## MCP Servers Implemented

### 1. AWS Documentation MCP Server
- **Purpose**: Provides access to AWS documentation and services information
- **Package**: `awslabs.aws-documentation-mcp-server@latest`
- **First Setup**: September 18, 2025

### 2. Cost Explorer MCP Server  
- **Purpose**: Provides AWS cost analysis and billing information
- **Package**: `awslabs.cost-explorer-mcp-server@latest`
- **First Setup**: September 18, 2025

### 3. ECS MCP Server
- **Purpose**: Manages AWS ECS (Elastic Container Service) resources
- **Package**: `awslabs-ecs-mcp-server`
- **First Setup**: September 25, 2025

## Prerequisites Installation

### System Requirements
- **Operating System**: Ubuntu 20.04+ (or other Linux distributions)
- **Internet Connection**: Required for downloading packages and authentication
- **Terminal Access**: Command line interface
- **Email Address**: Required for Builder ID registration

### 1. Amazon Q CLI (Primary Interface)
- **Tool**: `q` - Amazon Q Command Line Interface
- **Version**: 1.13.1+
- **Installation Location**: `/usr/bin/q` or `/usr/local/bin/q`
- **Purpose**: Primary interface for interacting with Amazon Q and MCP servers
- **Key Command**: `q chat` - Main command for chatting with Amazon Q
- **Builder ID Requirement**: ⚠️ **CRITICAL** - Builder ID must be configured to use Q CLI

#### Installation Commands:
```bash
# Method 1: Using the official installer (Recommended)
curl -sSL https://aws-cli.amazon.com/amazonq/install.sh | bash

# Method 2: Using snap (if available)
sudo snap install amazon-q

# Method 3: Download and install manually
wget https://aws-cli.amazon.com/amazonq/amazonq-linux-x64.tar.gz
tar -xzf amazonq-linux-x64.tar.gz
sudo ./amazonq/install

# Verify installation
q --version
# Should output: q 1.13.1 or higher
```

### 2. AWS CLI (Required for AWS Authentication)
- **Tool**: `aws` - Amazon Web Services Command Line Interface
- **Purpose**: Manages AWS credentials and profiles for MCP servers
- **Required for**: AWS authentication and service access

#### Installation Commands:
```bash
# Method 1: Using apt (Ubuntu/Debian)
sudo apt update
sudo apt install awscli -y

# Method 2: Using pip (if you have Python)
pip3 install awscli --user

# Method 3: Using official installer (Latest version)
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt install unzip -y
unzip awscliv2.zip
sudo ./aws/install

# Verify installation
aws --version
# Should output: aws-cli/2.x.x or higher
```

### 3. Python 3.8+ (Required for UV and MCP servers)
- **Tool**: `python3` - Python programming language
- **Purpose**: Required for UV package manager and MCP servers
- **Minimum Version**: 3.8+

#### Installation Commands:
```bash
# Check if Python is already installed
python3 --version

# If not installed or version is too old:
sudo apt update
sudo apt install python3 python3-pip python3-venv -y

# Verify installation
python3 --version
pip3 --version
```

### 4. UV Package Manager
- **Tool**: `uv` (Universal Virtualenv) - Fast Python package manager
- **Version**: 0.8.5+
- **Installation Location**: `/home/indianic/.local/bin/uv`
- **Purpose**: Used to run MCP servers via `uvx` command

#### Installation Commands:
```bash
# Method 1: Official installer (Recommended)
curl -LsSf https://astral.sh/uv/install.sh | sh

# Method 2: Using pip
pip3 install uv --user

# Add to PATH (add to ~/.bashrc for permanent)
export PATH="$HOME/.local/bin:$PATH"
source ~/.bashrc

# Verify installation
uv --version
# Should output: uv 0.8.5 or higher
```

### 5. UVX Tool (Comes with UV)
- **Tool**: `uvx` - Tool for running Python packages in isolated environments
- **Installation Location**: `/home/indianic/.local/bin/uvx`
- **Purpose**: Executes MCP servers without global installation
- **Note**: Automatically installed with UV package manager

#### Verification:
```bash
# Should be available after UV installation
uvx --help
# Should show uvx command options
```

### 6. VS Code (IDE for hosting MCP servers)
- **Tool**: Visual Studio Code
- **Purpose**: Hosts and manages MCP servers
- **Required Extensions**: None specific, MCP servers appear in extensions section

#### Installation Commands:
```bash
# Method 1: Using snap
sudo snap install code --classic

# Method 2: Using apt (official Microsoft repository)
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
sudo install -o root -g root -m 644 packages.microsoft.gpg /etc/apt/trusted.gpg.d/
sudo sh -c 'echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/trusted.gpg.d/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list'
sudo apt update
sudo apt install code -y

# Method 3: Download .deb package
wget https://code.visualstudio.com/sha/download?build=stable&os=linux-deb-x64 -O code.deb
sudo dpkg -i code.deb
sudo apt-get install -f  # Fix any dependency issues

# Verify installation
code --version
```

### 7. Essential System Tools
#### Additional tools that might be needed:

```bash
# Install common tools
sudo apt update
sudo apt install -y \
    curl \
    wget \
    unzip \
    git \
    jq \
    ca-certificates \
    gnupg \
    lsb-release

# Verify installations
curl --version
wget --version
unzip -v
git --version
jq --version
```

## Configuration Files

### 1. VS Code Configuration
**File**: `/home/indianic/.config/Code/User/mcp.json`

```json
{
    "servers": {
        "Cost Explorer MCP Server": {
            "command": "uvx",
            "args": [
                "awslabs.cost-explorer-mcp-server@latest"
            ],
            "env": {
                "AWS_PROFILE": "your-aws-profile",
                "AWS_REGION": "us-east-1",
                "FASTMCP_LOG_LEVEL": "ERROR"
            },
            "disabled": false,
            "autoApprove": [],
            "type": "stdio"
        },
        "AWS Documentation MCP Server": {
            "command": "uvx",
            "args": [
                "awslabs.aws-documentation-mcp-server@latest"
            ],
            "env": {
                "FASTMCP_LOG_LEVEL": "ERROR",
                "AWS_DOCUMENTATION_PARTITION": "aws"
            },
            "disabled": false,
            "autoApprove": [],
            "type": "stdio"
        },
        "ECS MCP Server": {
            "command": "uvx",
            "args": [
                "--from",
                "awslabs-ecs-mcp-server",
                "ecs-mcp-server"
            ],
            "env": {
                "AWS_PROFILE": "your-aws-profile",
                "AWS_REGION": "your-aws-region",
                "FASTMCP_LOG_LEVEL": "ERROR",
                "FASTMCP_LOG_FILE": "/path/to/ecs-mcp-server.log",
                "ALLOW_WRITE": "false",
                "ALLOW_SENSITIVE_DATA": "false"
            },
            "type": "stdio"
        }
    },
    "inputs": []
}
```

### 2. Amazon Q Configuration (amazonnq)
**File**: `/home/indianic/.aws/amazonnq/mcp.json`

```json
{
  "mcpServers": {
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
```

### 3. Amazon Q Configuration (amazonq)
**File**: `/home/indianic/.aws/amazonq/mcp.json`

```json
{
  "mcpServers": {
    "awslabs.cost-explorer-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.cost-explorer-mcp-server@latest"],
      "env": {
        "FASTMCP_LOG_LEVEL": "ERROR",
        "AWS_PROFILE": "default"
      },
      "disabled": false,
      "autoApprove": []
    },
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

## Amazon Q CLI Workflow

### Primary Usage Pattern
1. **Start MCP servers in VS Code**: The MCP servers are visible in VS Code extensions section
2. **Start the servers**: Activate MCP servers from VS Code interface 
3. **Use Q CLI for interaction**: Primary interaction through `q chat` command
4. **Builder ID Authentication**: Builder ID must be configured for Q CLI to work

### Amazon Q CLI Commands
```bash
# Main chat interface with Amazon Q
q chat

# Other useful commands
q help                    # Show help and available commands
q settings               # Customize Q CLI appearance & behavior
q doctor                # Debug installation issues
q translate             # Natural Language to Shell translation
q quit                  # Quit the application
```

### Builder ID Configuration (CRITICAL PREREQUISITE)

⚠️ **IMPORTANT**: Builder ID is **REQUIRED** to use Amazon Q CLI. Without it, `q chat` will not work.

#### What is Builder ID?
- Amazon's unified identity for developer tools
- Required for Amazon Q CLI authentication
- Free to create and use
- Links your local development environment to AWS services

#### Builder ID Setup Process:
1. **Create Builder ID Account**:
   - Visit: https://aws.amazon.com/builder-id/
   - Sign up with email address
   - Verify email and complete registration

2. **Configure Q CLI with Builder ID**:
   ```bash
   q settings
   # Follow prompts to authenticate with Builder ID
   ```

3. **Verify Configuration**:
   ```bash
   q chat
   # Should now work without authentication errors
   ```

## Complete Implementation Guide (Step-by-Step for Newbies)

### 🎥 **Prerequisites Check**
```bash
# Update your system first
sudo apt update && sudo apt upgrade -y

# Check if you have required tools
which curl || echo "Need to install curl"
which wget || echo "Need to install wget"
which python3 || echo "Need to install python3"
```

---

### **Step 1: Install Essential System Tools**
```bash
# Install basic tools
sudo apt update
sudo apt install -y \
    curl \
    wget \
    unzip \
    git \
    jq \
    ca-certificates \
    gnupg \
    lsb-release \
    python3 \
    python3-pip \
    python3-venv

# Verify installations
echo "=== Verification ==="
curl --version | head -1
wget --version | head -1
python3 --version
pip3 --version
```

---

### **Step 2: Install AWS CLI**
```bash
# Download and install AWS CLI v2
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

# Clean up
rm -rf awscliv2.zip aws/

# Verify installation
aws --version
echo "AWS CLI installed successfully!"
```

---

### **Step 3: Configure AWS Credentials**
```bash
# Configure AWS credentials (you'll need your AWS Access Key ID and Secret)
aws configure
# When prompted, enter:
# - AWS Access Key ID: [Your Access Key]
# - AWS Secret Access Key: [Your Secret Key]
# - Default region name: us-east-1 (or your preferred region)
# - Default output format: json

# Test AWS authentication
aws sts get-caller-identity
# Should show your AWS account information
```

---

### **Step 4: Install UV Package Manager**
```bash
# Install UV using the official installer
curl -LsSf https://astral.sh/uv/install.sh | sh

# Add UV to your PATH
export PATH="$HOME/.local/bin:$PATH"

# Make PATH change permanent
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc

# Verify installation
uv --version
uvx --help | head -5
echo "UV and UVX installed successfully!"
```

---

### **Step 5: Install Amazon Q CLI**
```bash
# Method 1: Try the official installer (if available)
curl -sSL https://aws-cli.amazon.com/amazonq/install.sh | bash

# If Method 1 fails, try Method 2: Using snap
if ! command -v q &> /dev/null; then
    sudo snap install amazon-q
fi

# If both methods fail, you may need to download manually
# Check the Amazon Q documentation for the latest installation method

# Verify installation
q --version
echo "Amazon Q CLI installed successfully!"
```

---

### **Step 6: Install VS Code**
```bash
# Method 1: Using snap (easiest)
sudo snap install code --classic

# Verify installation
code --version
echo "VS Code installed successfully!"

# If snap doesn't work, use this alternative:
# wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
# sudo install -o root -g root -m 644 packages.microsoft.gpg /etc/apt/trusted.gpg.d/
# sudo sh -c 'echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/trusted.gpg.d/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list'
# sudo apt update
# sudo apt install code -y
```

---

### **Step 7: Create Builder ID Account (CRITICAL)**
```bash
echo "=== IMPORTANT: Builder ID Setup ==="
echo "1. Open your browser and go to: https://aws.amazon.com/builder-id/"
echo "2. Click 'Sign up for free'"
echo "3. Enter your email address and create a password"
echo "4. Verify your email address"
echo "5. Complete the registration process"
echo "6. Return to the terminal when done"
echo ""
read -p "Press Enter when you have completed Builder ID registration..."

# Configure Amazon Q CLI with Builder ID
echo "Now configuring Amazon Q CLI with your Builder ID..."
q settings

# Test the authentication
echo "Testing Amazon Q CLI authentication..."
q chat --help || echo "If this fails, run 'q settings' again"
```

---

### **Step 8: Test MCP Servers**
```bash
echo "=== Testing MCP Servers ==="

# Test AWS Documentation MCP Server
echo "Testing AWS Documentation MCP Server..."
uvx awslabs.aws-documentation-mcp-server@latest --help
if [ $? -eq 0 ]; then
    echo "✓ AWS Documentation MCP Server works"
else
    echo "✗ AWS Documentation MCP Server failed"
fi

# Test Cost Explorer MCP Server
echo "Testing Cost Explorer MCP Server..."
uvx awslabs.cost-explorer-mcp-server@latest --help
if [ $? -eq 0 ]; then
    echo "✓ Cost Explorer MCP Server works"
else
    echo "✗ Cost Explorer MCP Server failed"
fi

# Test ECS MCP Server
echo "Testing ECS MCP Server..."
uvx --from awslabs-ecs-mcp-server ecs-mcp-server --help
if [ $? -eq 0 ]; then
    echo "✓ ECS MCP Server works"
else
    echo "✗ ECS MCP Server failed"
fi
```

---

### **Step 9: Create Project Directory**
```bash
# Create and navigate to project directory
mkdir -p ~/aws-docs-mcp
cd ~/aws-docs-mcp

# Create directory structure
mkdir -p ~/.config/Code/User
mkdir -p ~/.aws/amazonq
mkdir -p ~/.aws/amazonnq

echo "Project directory structure created!"
```

---

### **Step 10: Configure VS Code MCP Integration**
```bash
# Create VS Code MCP configuration
cat > ~/.config/Code/User/mcp.json << 'EOF'
{
    "servers": {
        "Cost Explorer MCP Server": {
            "command": "uvx",
            "args": [
                "awslabs.cost-explorer-mcp-server@latest"
            ],
            "env": {
                "AWS_PROFILE": "default",
                "AWS_REGION": "us-east-1",
                "FASTMCP_LOG_LEVEL": "ERROR"
            },
            "disabled": false,
            "autoApprove": [],
            "type": "stdio"
        },
        "AWS Documentation MCP Server": {
            "command": "uvx",
            "args": [
                "awslabs.aws-documentation-mcp-server@latest"
            ],
            "env": {
                "FASTMCP_LOG_LEVEL": "ERROR",
                "AWS_DOCUMENTATION_PARTITION": "aws"
            },
            "disabled": false,
            "autoApprove": [],
            "type": "stdio"
        },
        "ECS MCP Server": {
            "command": "uvx",
            "args": [
                "--from",
                "awslabs-ecs-mcp-server",
                "ecs-mcp-server"
            ],
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

echo "VS Code MCP configuration created!"
```

---

### **Step 11: Configure Amazon Q MCP Integration**
```bash
# Create Amazon Q MCP configuration
cat > ~/.aws/amazonq/mcp.json << 'EOF'
{
  "mcpServers": {
    "awslabs.cost-explorer-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.cost-explorer-mcp-server@latest"],
      "env": {
        "FASTMCP_LOG_LEVEL": "ERROR",
        "AWS_PROFILE": "default"
      },
      "disabled": false,
      "autoApprove": []
    },
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

# Create amazonnq configuration (subset)
cat > ~/.aws/amazonnq/mcp.json << 'EOF'
{
  "mcpServers": {
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

echo "Amazon Q MCP configurations created!"
```

---

### **Step 12: Final Verification**
```bash
echo "=== Final System Verification ==="

# Check all installations
echo "1. Amazon Q CLI: $(q --version)"
echo "2. UV Package Manager: $(uv --version)"
echo "3. AWS CLI: $(aws --version | cut -d' ' -f1)"
echo "4. Python: $(python3 --version)"
echo "5. VS Code: $(code --version | head -1)"

echo ""
echo "6. Testing AWS Authentication:"
aws sts get-caller-identity --output table 2>/dev/null && echo "   ✓ AWS authenticated" || echo "   ✗ AWS authentication failed"

echo ""
echo "7. Testing MCP Configurations:"
ls -la ~/.config/Code/User/mcp.json 2>/dev/null && echo "   ✓ VS Code MCP config found" || echo "   ✗ VS Code MCP config missing"
ls -la ~/.aws/amazonq/mcp.json 2>/dev/null && echo "   ✓ Amazon Q config found" || echo "   ✗ Amazon Q config missing"

echo ""
echo "8. Testing Builder ID Authentication:"
echo "   Run 'q chat' to test - it should open the chat interface"
echo "   If it fails, run 'q settings' to reconfigure Builder ID"

echo ""
echo "=== Setup Complete! ==="
echo "Next steps:"
echo "1. Open VS Code: code"
echo "2. Look for MCP servers in the extensions section"
echo "3. Start the MCP servers from VS Code"
echo "4. Open a terminal and run: q chat"
echo "5. You should now be able to interact with Amazon Q and your MCP servers!"
```

---

### **Step 13: How to Use (Quick Start)**
```bash
echo "=== Quick Usage Guide ==="
echo "1. Start VS Code:"
echo "   code"
echo ""
echo "2. In VS Code, look for MCP servers in the extensions section"
echo "   - AWS Documentation MCP Server"
echo "   - Cost Explorer MCP Server"
echo "   - ECS MCP Server"
echo ""
echo "3. Start the servers by clicking on them"
echo ""
echo "4. Open a new terminal and start Amazon Q:"
echo "   q chat"
echo ""
echo "5. You can now ask questions about AWS services, costs, and ECS!"
echo ""
echo "Example questions to try:"
echo "- 'What is Amazon S3?'"
echo "- 'Show me my AWS costs for this month'"
echo "- 'List my ECS services'"
```

## Environment Variables Used

### Common Environment Variables
- `FASTMCP_LOG_LEVEL`: Set to "ERROR" for minimal logging
- `AWS_PROFILE`: AWS credential profile to use
- `AWS_REGION`: AWS region for operations
- `AWS_DOCUMENTATION_PARTITION`: Set to "aws" for standard AWS partition

### ECS-Specific Environment Variables
- `FASTMCP_LOG_FILE`: Path to log file for ECS operations
- `ALLOW_WRITE`: Set to "false" for read-only operations
- `ALLOW_SENSITIVE_DATA`: Set to "false" for security

## Log Files and Debugging

### VS Code Logs Location
```
/home/indianic/.config/Code/logs/[DATE]/window[N]/mcpServer.*.log
```

### Recent Log Activity
- **September 18, 2025**: Initial AWS Documentation and Cost Explorer setup
- **September 25, 2025**: ECS MCP Server integration
- **September 30, 2025**: Recent activity with all three servers

## Commands for Testing

### Test Individual MCP Servers
```bash
# Test AWS Documentation MCP Server
uvx awslabs.aws-documentation-mcp-server@latest

# Test Cost Explorer MCP Server
uvx awslabs.cost-explorer-mcp-server@latest

# Test ECS MCP Server
uvx --from awslabs-ecs-mcp-server ecs-mcp-server
```

### Check Amazon Q CLI Installation
```bash
# Check Q CLI version
q --version

# Test Q CLI functionality
q help

# Check authentication status
q settings

# Test chat functionality (requires Builder ID)
q chat
```

### Check UV Installation
```bash
uv --version
uvx --help
```

## IDE Integration Status

### ✅ VS Code (MCP Server Host)
- **Status**: Fully configured and operational
- **Role**: Hosts and runs MCP servers
- **Servers**: All 3 MCP servers configured and visible in extensions section
- **Config File**: `/home/indianic/.config/Code/User/mcp.json`
- **Usage**: Start servers from VS Code interface, then use Q CLI for interaction
- **MCP Server Visibility**: Servers appear in VS Code extensions section when running

### ✅ Amazon Q CLI (Primary Interface)
- **Status**: Active primary interface
- **Role**: Main interaction point with MCP servers
- **Command**: `q chat` - Primary command for interacting with Amazon Q and MCP servers
- **Authentication**: Builder ID configured and working
- **Integration**: Works in conjunction with VS Code-hosted MCP servers

### ✅ Amazon Q Configuration Files
- **Status**: Well configured
- **amazonnq**: Cost Explorer server configured
- **amazonq**: Cost Explorer + AWS Documentation servers configured
- **Config Files**: 
  - `/home/indianic/.aws/amazonnq/mcp.json`
  - `/home/indianic/.aws/amazonq/mcp.json`

### ⚠️ Cursor IDE
- **Status**: Configuration exists but empty
- **Config File**: `/home/indianic/.config/Cursor/User/globalStorage/saoudrizwan.claude-dev/settings/cline_mcp_settings.json`
- **Note**: Not currently used in the workflow

## Troubleshooting

### Common Issues and Solutions

#### 1. "q chat" Not Working
**Problem**: `q chat` command fails or shows authentication errors

**Solutions**:
```bash
# Check if Builder ID is configured
q settings

# If not configured, set up Builder ID authentication
# Visit https://aws.amazon.com/builder-id/ to create account first
q settings
# Follow the authentication flow

# Test authentication
q chat
```

#### 2. MCP Servers Not Visible in VS Code
**Problem**: MCP servers don't appear in VS Code extensions section

**Solutions**:
1. **Check VS Code MCP Configuration**:
   ```bash
   cat /home/indianic/.config/Code/User/mcp.json
   ```

2. **Verify UV/UVX Installation**:
   ```bash
   which uvx
   uvx --help
   ```

3. **Test MCP Servers Individually**:
   ```bash
   uvx awslabs.aws-documentation-mcp-server@latest
   ```

4. **Check VS Code Logs**:
   ```bash
   ls -la /home/indianic/.config/Code/logs/$(ls /home/indianic/.config/Code/logs/ | tail -1)/*/mcpServer.*
   ```

#### 3. AWS Authentication Issues
**Problem**: MCP servers can't access AWS services

**Solutions**:
```bash
# Check AWS configuration
aws configure list

# Verify AWS credentials
aws sts get-caller-identity

# Check AWS profile in MCP configuration
grep -r "AWS_PROFILE" /home/indianic/.config/Code/User/mcp.json
grep -r "AWS_PROFILE" /home/indianic/.aws/*/mcp.json
```

#### 4. Builder ID Setup Issues
**Problem**: Can't configure Builder ID or authentication fails

**Step-by-Step Resolution**:
1. **Create Builder ID Account**:
   - Go to: https://aws.amazon.com/builder-id/
   - Use a valid email address
   - Complete email verification

2. **Clear Previous Authentication** (if needed):
   ```bash
   # Check Q CLI doctor for issues
   q doctor
   
   # Clear settings if needed (this will reset authentication)
   q settings --reset  # (if this option exists)
   ```

3. **Re-authenticate**:
   ```bash
   q settings
   # Follow prompts carefully
   # Enter Builder ID credentials when prompted
   ```

4. **Verify**:
   ```bash
   q chat
   # Should work without errors
   ```

#### 5. MCP Server Connection Issues
**Problem**: MCP servers start but don't respond properly

**Diagnosis and Solutions**:
```bash
# Check MCP server logs
tail -f "/home/indianic/.config/Code/logs/$(ls /home/indianic/.config/Code/logs/ | tail -1)/window1/mcpServer.mcp.config.usrlocal.AWS Documentation MCP Server.log"

# Test server directly
uvx awslabs.aws-documentation-mcp-server@latest

# Check environment variables
env | grep -E "AWS_|FASTMCP_"

# Verify AWS partition setting
echo $AWS_DOCUMENTATION_PARTITION
```

### Validation Commands

#### Complete System Check
```bash
# 1. Check all prerequisites
q --version                    # Should show 1.13.1
uv --version                   # Should show 0.8.5  
uvx --help | head -5          # Should show uvx help
aws --version                 # Should show AWS CLI version

# 2. Check Builder ID authentication
q settings                    # Should show authenticated status

# 3. Test Q CLI
q chat                        # Should open chat interface

# 4. Check MCP configurations
cat /home/indianic/.config/Code/User/mcp.json | jq .  # Should show valid JSON

# 5. Test MCP servers
uvx awslabs.aws-documentation-mcp-server@latest --help
uvx awslabs.cost-explorer-mcp-server@latest --help

# 6. Check AWS authentication
aws sts get-caller-identity   # Should show your AWS identity
```

#### Quick Diagnostic Script
```bash
#!/bin/bash
echo "=== MCP System Diagnostic ==="
echo "Q CLI Version: $(q --version)"
echo "UV Version: $(uv --version)"
echo "AWS CLI Version: $(aws --version)"
echo ""
echo "Builder ID Status:"
q settings | grep -i "auth\|login\|builder" || echo "Run 'q settings' to check"
echo ""
echo "MCP Config Files:"
ls -la /home/indianic/.config/Code/User/mcp.json 2>/dev/null && echo "✓ VS Code MCP config found" || echo "✗ VS Code MCP config missing"
ls -la /home/indianic/.aws/amazonq/mcp.json 2>/dev/null && echo "✓ Amazon Q config found" || echo "✗ Amazon Q config missing"
echo ""
echo "AWS Authentication:"
aws sts get-caller-identity --output table 2>/dev/null && echo "✓ AWS authenticated" || echo "✗ AWS authentication failed"
```

## Security Considerations

### Current Security Measures
- Read-only access for ECS server (`ALLOW_WRITE: false`)
- No sensitive data exposure (`ALLOW_SENSITIVE_DATA: false`)
- Error-level logging only (minimal information disclosure)
- AWS profile-based authentication
- Builder ID authentication for Q CLI access

### Recommendations
- Regular AWS credential rotation
- Monitor MCP server access logs
- Implement principle of least privilege for AWS profiles
- Regular security audits of MCP configurations
- Keep Builder ID credentials secure
- Monitor Q CLI usage and authentication logs

---

**Generated**: September 30, 2025
**System**: Ubuntu Linux
**User**: indianic
**MCP Servers**: 3 (AWS Documentation, Cost Explorer, ECS)
