# HyperAGI Inference Engine Configuration Guide

## System Requirements

### I. Basic Requirements

- **CPU**: Minimum 4 cores, recommended 8 cores
- **Memory**: > 16GB
- **GPU (Single Card)**: Nvidia RTX 3090 or higher (minimum 32GB VRAM required)
- **Storage**: > 512GB
- **Network**: 5-10M

- **Operating System**: Supports Windows and Linux

## Installation Steps

### II. Install Docker

#### Linux System Installation

##### Prerequisites
Ensure your system supports Docker and has NVIDIA drivers installed to support GPU.

##### Installation Steps

1. **Update package index**:
   ```bash
   sudo apt-get update
   ```

2. **Install required packages**:
   ```bash
   sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
   ```

3. **Add Docker official GPG key**:
   ```bash
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
   ```

4. **Set up Docker stable repository**:
   ```bash
   sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
   ```

5. **Update package index again**:
   ```bash
   sudo apt-get update
   ```

6. **Install Docker CE**:
   ```bash
   sudo apt-get install docker-ce
   ```

7. **Install NVIDIA Container Toolkit**:
   ```bash
   sudo curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
   distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
   sudo curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
   sudo apt-get update
   sudo apt-get install -y nvidia-docker2
   sudo systemctl restart docker
   ```

8. **Start and enable Docker service**:
   ```bash
   sudo systemctl start docker
   sudo systemctl enable docker
   ```

9. **Verify installation**:
   ```bash
   docker --version
   ```

#### Windows System Installation

##### Prerequisites
- Windows 10/11 Pro, Enterprise, or Education (64-bit)
- Hyper-V and container features enabled
- NVIDIA drivers installed

##### Installation Steps

1. **Download Docker Desktop for Windows**:
   - Visit [Docker Desktop official website](https://www.docker.com/products/docker-desktop)
   - Download Windows version

2. **Install Docker Desktop**:
   - Run the downloaded installer
   - Follow the installation wizard to complete installation
   - Restart computer

3. **Configure Docker Desktop**:
   - Start Docker Desktop
   - Enable WSL 2 backend in settings (recommended)
   - Configure resource allocation (recommend at least 8GB memory)

4. **Install NVIDIA Container Toolkit**:
   - Download and install [NVIDIA Container Toolkit for Windows](https://docs.nvidia.com/cuda/wsl-user-guide/index.html)
   - Configure Docker to use NVIDIA runtime

5. **Verify installation**:
   ```cmd
   docker --version
   ```

#### macOS System Installation

##### Prerequisites
- macOS 10.15 or higher
- Homebrew installed (recommended)

##### Installation Steps

1. **Install using Homebrew** (recommended):
   ```bash
   brew install --cask docker
   ```

2. **Or download manually**:
   - Visit [Docker Desktop official website](https://www.docker.com/products/docker-desktop)
   - Download macOS version
   - Drag to Applications folder

3. **Start Docker Desktop**:
   - Launch Docker Desktop from Applications
   - Complete initial setup

4. **Configure resource allocation**:
   - Configure memory and CPU allocation in Docker Desktop settings
   - Recommend at least 8GB memory allocation

5. **Verify installation**:
   ```bash
   docker --version
   ```

**Note**: Docker on macOS does not support GPU acceleration, only CPU inference is supported.

### Operating System Specific Notes

#### Linux System
- Ensure NVIDIA drivers and CUDA are installed
- Recommend using Ubuntu 20.04 or higher
- Ensure Docker service is started and set to auto-start

#### Windows System
- Requires Windows 10/11 Pro or higher
- Ensure Hyper-V and WSL 2 are enabled
- Recommend using WSL 2 backend for better performance
- Ensure Docker Desktop has sufficient resource allocation

#### macOS System
- Supports macOS 10.15 or higher
- Does not support GPU acceleration, only CPU inference
- Recommend using Homebrew for package management
- Ensure Docker Desktop has sufficient resource allocation

### III. Install Docker Compose

#### Linux System Installation

1. **Download latest version**:
   ```bash
   sudo curl -L "https://github.com/docker/compose/releases/download/v2.5.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
   ```

2. **Add executable permissions**:
   ```bash
   sudo chmod +x /usr/local/bin/docker-compose
   ```

3. **Verify installation**:
   ```bash
   docker-compose --version
   ```

#### Windows System Installation

Docker Desktop for Windows already includes Docker Compose, no separate installation needed.

Verify installation:
```cmd
docker-compose --version
```

#### macOS System Installation

Docker Desktop for macOS already includes Docker Compose, no separate installation needed.

Verify installation:
```bash
docker-compose --version
```

### IV. Install Git

#### Linux System Installation

1. **Update package list**:
   ```bash
   sudo apt-get update
   ```

2. **Install Git**:
   ```bash
   sudo apt-get install git
   ```

3. **Verify installation**:
   ```bash
   git --version
   ```

#### Windows System Installation

1. **Download Git for Windows**:
   - Visit [Git official website](https://git-scm.com/download/win)
   - Download Windows version

2. **Install Git**:
   - Run the downloaded installer
   - Follow the installation wizard to complete installation
   - Recommend using default settings

3. **Verify installation**:
   ```cmd
   git --version
   ```

#### macOS System Installation

1. **Install using Homebrew** (recommended):
   ```bash
   brew install git
   ```

2. **Or use Xcode Command Line Tools**:
   ```bash
   xcode-select --install
   ```

3. **Verify installation**:
   ```bash
   git --version
   ```

### V. Deploy HyperAGI Inference System

#### Step 1: Clone Repository

**Linux/macOS**:
```bash
git clone https://github.com/HyperdustLab/moss-inference-engine
cd moss-inference-engine
```

**Windows**:
```cmd
git clone https://github.com/HyperdustLab/moss-inference-engine
cd moss-inference-engine
```

**Or using PowerShell**:
```powershell
git clone https://github.com/HyperdustLab/moss-inference-engine
cd moss-inference-engine
```

## Environment Variables Configuration Guide

### 1. Required Configuration Items

#### WALLET_ADDRESS
- **Description**: Ethereum wallet address for receiving earnings
- **Format**: 0x-prefixed 42-character Ethereum address
- **Example**: `WALLET_ADDRESS=0x742d35Cc6634C0532925a3b8D4C9db96C4b4d8b6`

### 2. Network Configuration

#### PUBLIC_IP
- **Description**: Public IP address, obtained from easytier logs
- **Obtainment Steps**:
  1. Start easytier service: `docker-compose up -d easytier`
  2. View logs: `docker-compose logs easytier`
  3. Search for `dhcp ip changed` information
  4. Extract IP address (e.g.: `43.159.42.3`)
- **Example**: `PUBLIC_IP=43.159.42.3`

#### NODE
- **Description**: Node ID, assigned by the platform
- **Obtainment Method**: Platform staff will inform you of your node ID
- **Example**: `NODE=your_node_id`

### 3. Configuration Examples

#### Configure .env File

The `.env` file in the project root directory contains the following configuration:

```env
# Required configuration items
WALLET_ADDRESS=0xYourEthereumWalletAddress  # Your Ethereum wallet address
PUBLIC_IP=43.159.42.3                       # Obtained from easytier logs
NODE=your_node_id                           # Platform-assigned node ID
```

#### docker-compose.yaml Configuration

The `docker-compose.yaml` file is already configured to use environment variables:

```yaml
services:
  nacos:
    image: hyperagi/nacos-client:last
    environment:
      - PUBLIC_IP=${PUBLIC_IP}
      - NODE=${NODE}
      - PORT=8881
      - WALLET_ADDRESS=${WALLET_ADDRESS}
      - SERVICE_NAME=cogito:32b
    restart: always
```

### 6. Configuration Steps

1. **Edit .env file**:

   **Linux/macOS**:
   ```bash
   nano .env
   # or use vim
   vim .env
   ```

   **Windows (PowerShell)**:
   ```powershell
   notepad .env
   # or use VS Code
   code .env
   ```

   **Windows (CMD)**:
   ```cmd
   notepad .env
   ```

2. **Configure environment variables**:
   - Set `WALLET_ADDRESS` to your Ethereum wallet address
   - Start easytier to obtain `PUBLIC_IP`
   - Set `NODE` to platform-assigned node ID

3. **Save and start services**:

   **Linux/macOS**:
   ```bash
   docker-compose up -d
   ```

   **Windows**:
   ```cmd
   docker-compose up -d
   ```

### 7. Verify Configuration

Check if configuration is correct:

**Linux/macOS**:
```bash
# View nacos service logs
docker-compose logs nacos

# Check service status
docker-compose ps
```

**Windows**:
```cmd
# View nacos service logs
docker-compose logs nacos

# Check service status
docker-compose ps
```

### 8. Common Configuration Errors

1. **Incorrect wallet address format**
   - Ensure it starts with `0x`
   - Ensure it's 42 characters long
   - Ensure it's a valid Ethereum address

2. **Incorrect PUBLIC_IP configuration**
   - Ensure it's correctly obtained from easytier logs
   - Don't use local IP addresses
   - Don't use public IP addresses

3. **Incorrect NODE configuration**
   - Ensure you use the platform-assigned node ID
   - Don't set or guess on your own

### 9. Configuration Checklist

- [ ] `.env` file configured correctly
- [ ] WALLET_ADDRESS set to valid Ethereum address
- [ ] PUBLIC_IP correctly obtained from easytier logs
- [ ] NODE set to platform-assigned node ID
- [ ] Firewall disabled or port exceptions configured
- [ ] All services started normally
- [ ] No error messages in logs

### 10. Important Notes

1. **Environment variable priority**:
   - Docker Compose automatically loads the `.env` file in the project root directory
   - If environment variables are set in the system, they will override values in the `.env` file

2. **.env file security**:
   - The `.env` file contains sensitive information, ensure it's not committed to version control

### 11. Getting Help

If you encounter issues during configuration:

1. View service logs for detailed error information
2. Check network connection and firewall settings
3. Confirm `.env` file configuration is correct
4. Verify environment variables are loaded correctly
5. Contact technical support team 