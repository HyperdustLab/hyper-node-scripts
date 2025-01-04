# HyperAGI Inference System Deployment Guide

## I. Basic Requirements

1. **CPU**: Minimum 4 cores, recommended 8 cores
2. **Memory**: > 16GB
3. **GPU (Single Card)**: Nvidia RTX 3060 / Nvidia RTX 3080 / Nvidia RTX 3090
4. **Hard Drive**: > 512GB
5. **Internet**: 5-10M
6. **Public Address**: Mandatory

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

1. Open the .env file:

```bash
nano .env
```

2. Configure the following variables:

- `PUBLIC_IP`: Your server's public IP (get it using `curl ifconfig.me`)
- `WALLET_ADDRESS`: Your Ethereum wallet address

3. Save changes:

- Press `Ctrl + O` to write changes
- Press `Ctrl + Enter` to save
- Press `Ctrl + X` to exit

### Step 3: Pull Docker Images

```bash
docker-compose pull
```

### Step 4: Start the Service

```bash
docker-compose up -d
```

### Step 5: Verify Deployment

```bash
docker-compose ps
```

## Important Notes

- Ensure ports 5200 TCP and 5000 TCP are accessible
- For troubleshooting, check logs using:

```bash
docker-compose logs
```

- For advanced configurations, refer to the docker-compose.yml file

## Support

For additional assistance, please refer to the documentation or seek support from the community.
