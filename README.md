# HyperAGI Inference System Deployment Guide

## I. Basic Requirements

1. **CPU**: Minimum 4 cores, recommended 8 cores
2. **Memory**: > 16GB
3. **GPU (Single Card)**: Nvidia RTX 3090 or higher (minimum 32GB VRAM required)
4. **Hard Drive**: > 512GB
5. **Internet**: 5-10M
6. **Network**: Fixed public IP (recommended) or dynamic IP with frpc tunnel

## II. Install Docker

### Prerequisites

Ensure your system supports Docker and has the NVIDIA driver installed to support GPU.

### Installation Steps

1. Update package index:

```bash
sudo apt-get update
```

2. Install required packages:

```bash
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
```

3. Add Docker's official GPG key:

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

4. Set up Docker stable repository:

```bash
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

5. Update package index again:

```bash
sudo apt-get update
```

6. Install Docker CE:

```bash
sudo apt-get install docker-ce
```

7. Install NVIDIA Container Toolkit:

```bash
sudo curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
sudo curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
sudo apt-get update
sudo apt-get install -y nvidia-docker2
sudo systemctl restart docker
```

8. Start and enable Docker service:

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

9. Verify installation:

```bash
docker --version
```

## III. Install Docker Compose

1. Download the latest version:

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/v2.5.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

2. Add executable permissions:

```bash
sudo chmod +x /usr/local/bin/docker-compose
```

3. Verify installation:

```bash
docker-compose --version
```

## IV. Install Git

1. Update package lists:

```bash
sudo apt-get update
```

2. Install Git:

```bash
sudo apt-get install git
```

3. Verify installation:

```bash
git --version
```

## V. Deploy the HyperAGI Inference System

### Step 1: Clone the Repository

```bash
git clone https://github.com/xfangs/hyperAGI-setup-script.git
cd hyperAGI-setup-script
```

### Step 2: Configure Environment Variables

1. Open the `docker-compose.yaml` file:

```bash
nano docker-compose.yaml
```

2. Configure the following variables in the `nacos` service:

#### Required Configuration:

- **WALLET_ADDRESS**: Enter your own Ethereum wallet address for receiving earnings
  ```yaml
  - WALLET_ADDRESS=0xYourEthereumWalletAddress
  ```

#### Network Configuration (Choose one option):

**Option A: Nodes with fixed public IP**

- **PUBLIC_IP**: Enter your node's fixed public IP address
- **NODE**: Format as `your_public_ip:port`, e.g., `203.0.113.1:1082`
- **PORT**: Use your specified port number, e.g., `1082`

```yaml
- PUBLIC_IP=203.0.113.1
- NODE=203.0.113.1:1082
- PORT=1082
```

**Option B: Nodes without fixed public IP**

- **PUBLIC_IP**: Enter fixed value `43.159.42.232`
- **NODE**: Node identifier assigned by MossAI platform
- **PORT**: Port number assigned by MossAI platform

```yaml
- PUBLIC_IP=43.159.42.232
- NODE=43.159.42.232:2 # Assigned by MossAI platform
- PORT=1082 # Assigned by MossAI platform
```

### Step 3: Configure frpc (Only for nodes without fixed public IP)

If you don't have a fixed public IP, you need to configure the frpc service for intranet penetration:

1. Edit the `frpc/frpc.toml` configuration file:

```bash
nano frpc/frpc.toml
```

2. Update configuration parameters:

```toml
[common]
server_addr = 43.159.42.232
server_port = 7000

[MOSSAI3000]
type = tcp
local_ip = 192.168.1.12  # Modify to your local intranet IP address
local_port = 8881        # Local port, no adjustment needed
remote_port = 1082       # Remote port, assigned by MossAI platform
```

**Configuration Notes:**

- `local_ip`: Modify to your local intranet IP address (check with `ip addr show` or `ifconfig`)
- `local_port`: Local port, keep as `8881`
- `remote_port`: Remote port, needs to be assigned by MossAI platform

3. Ensure frpc service is enabled in docker-compose.yaml (enabled by default)

### Step 4: Start Services

**Nodes with fixed public IP:**

```bash
# Disable frpc service (comment out or delete frpc service configuration)
docker-compose up -d nacos ollama poop-mcp-client
```

**Nodes without fixed public IP:**

```bash
# Start all services including frpc
docker-compose up -d
```

### Step 5: Verify Deployment

```bash
docker-compose ps
```

检查服务状态：

```bash
docker-compose logs nacos
docker-compose logs ollama
docker-compose logs poop-mcp-client
# If you don't have a fixed public IP, also check frpc logs
docker-compose logs frpc
```

## VI. Configuration Summary

| Configuration Item | With Fixed Public IP | Without Fixed Public IP  |
| ------------------ | -------------------- | ------------------------ |
| PUBLIC_IP          | Your public IP       | 43.159.42.232            |
| NODE               | Your public IP:port  | Assigned by MossAI       |
| PORT               | Your specified port  | Assigned by MossAI       |
| WALLET_ADDRESS     | Your Ethereum wallet | Your Ethereum wallet     |
| frpc service       | Not needed           | Need to start and config |

## Important Notes

- Ensure relevant ports ( 8881, etc.) are open in firewall
- If using frpc, ensure local intranet IP is configured correctly
- All port assignments and node identifiers should be obtained from MossAI platform
- Wallet address must be in valid Ethereum address format

## Troubleshooting

- 查看服务日志：

```bash
docker-compose logs [service-name]
```

- 检查网络连接：

```bash
curl ifconfig.me  # Check public IP
ip addr show      # Check internal IP
```

- 重启服务：

```bash
docker-compose restart [service-name]
```

## Support

For additional assistance, please refer to the documentation or seek support from the community.
