## 🚀 Aztec Sequencer Node Setup Guide

### 🛠 Pre-Requirements

* Docker & Docker Compose (Running in background)
* Aztec CLI tool
* Sepolia RPC & Beacon RPC URLs
* EVM wallet address and private key

---

### 🔧 Step 1: Install Required Dependencies

```bash
sudo apt-get update && sudo apt-get upgrade -y
```

---

### 📦 Step 2: Install Node.js

```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt update && sudo apt install -y nodejs
```

---

### 🧰 Step 3: Install Other Packages

```bash
sudo apt install curl iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev screen ufw -y
```

---

### 🐳 Step 4: Install Docker & Docker Compose

```bash
sudo apt update && sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update && sudo apt install -y docker-ce
sudo systemctl enable --now docker
sudo usermod -aG docker $USER && newgrp docker
```

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/$(curl -s https://api.github.com/repos/docker/compose/releases/latest | jq -r .tag_name)/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

**Verify Docker installation:**

```bash
docker --version
docker-compose --version
```

---

### 🧪 Step 5: Install Aztec CLI

```bash
bash -i <(curl -s https://install.aztec.network)
```

**Add to Path:**

```bash
echo 'export PATH="$HOME/.aztec/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

**Verify installation:**

```bash
aztec -h
```

**Set testnet:**

```bash
aztec-up alpha-testnet
```

---

### 💰 Step 6: Load Wallet with Sepolia ETH

* [PK910 Sepolia Faucet](https://sepolia-faucet.pk910.de)
* [Alchemy Sepolia Faucet](https://www.alchemy.com/faucets/ethereum-sepolia)

---

### 🔓 Step 7: Allow UFW Ports

```bash
sudo ufw allow 22
sudo ufw allow ssh
sudo ufw allow 40400
sudo ufw allow 8080
sudo ufw enable
```

---

### 🆓 Step 8: Get Free Sepolia & Beacon RPCs

Use this: [https://tinyurl.com/y8v7tm2d](https://tinyurl.com/y8v7tm2d)

* Sign up with **Email only**
* Choose **Ethereum Sepolia**
* Select **Growth Plan**
* Apply \$50 Voucher
* Copy your API key

🆕 Other RPC sources:

* [https://drpc.org/dashboard](https://drpc.org/dashboard)
* [https://developer.metamask.io/key/active-endpoints](https://developer.metamask.io/key/active-endpoints)
* [https://www.alchemy.com](https://www.alchemy.com)

---

### 🍥 Step 9: Start Your Sequencer Node

```bash
screen -S aztec
```

```bash
aztec start --node --archiver --sequencer \
  --network alpha-testnet \
  --l1-rpc-urls YOUR_SEPOLIA_RPC \
  --l1-consensus-host-urls YOUR_BEACON_RPC \
  --sequencer.validatorPrivateKey 0xYOUR_PRIVATE_KEY \
  --sequencer.coinbase YOUR_WALLET_ADDRESS \
  --p2p.p2pIp YOUR_EXTERNAL_IP
```

> Replace all values:
>
> * `YOUR_SEPOLIA_RPC`
> * `YOUR_BEACON_RPC`
> * `0xYOUR_PRIVATE_KEY`
> * `YOUR_WALLET_ADDRESS`
> * `YOUR_EXTERNAL_IP` (Get using: `curl ifconfig.me`)

---

### 📦 Save These Info/Data

```md
------👇 Save These Info/Data 👇------

Aztec Sequencer Node (XXXXX dc)

• Ethereum Sepolia RPC:
• Beacon Sepolia RPC:
• Private Key:
• Wallet Address:
• VPS External IP:
• Block Number:
• Base64 Encoded Proof:

------👆 Save These Info/Data 👆------
```

---

### ⌨️ Detach/Attach from Screen

* Detach: `Ctrl + A` then `D`
* Attach: `screen -r aztec`

---

### 🧙 Step 10: Apprentice Role (Discord)

**1. Get L2 Block Number:**

```bash
curl -s -X POST -H 'Content-Type: application/json' \
-d '{"jsonrpc":"2.0","method":"node_getL2Tips","params":[],"id":67}' \
http://localhost:8080 | jq -r ".result.proven.number"
```

**2. Get Sync Proof:**

```bash
curl -s -X POST -H 'Content-Type: application/json' \
-d '{"jsonrpc":"2.0","method":"node_getArchiveSiblingPath","params":["BLOCK_NUMBER","BLOCK_NUMBER"],"id":67}' \
http://localhost:8080 | jq -r ".result"
```

> Replace `BLOCK_NUMBER` with block from Step 1

**3. Go to [Aztec Discord](https://discord.gg/aztec)**
Channel: `#operators│start-here`
Command:

```
/operator start
```

Paste:

* Your EVM Wallet Address
* Block Number
* Base64 Sync Proof

---

### 🧩 Register as a Validator

```bash
aztec add-l1-validator \
  --l1-rpc-urls YOUR_SEPOLIA_RPC \
  --private-key 0xYOUR_PRIVATE_KEY \
  --attester YOUR_WALLET_ADDRESS \
  --proposer-eoa YOUR_WALLET_ADDRESS \
  --staking-asset-handler 0xF739D03e98e23A7B65940848aBA8921fF3bAc4b2 \
  --l1-chain-id 11155111
```

---

### 🔁 Upgrade Guide (Old Users)

```bash
screen -r aztec    # Step 1
# Step 2: Stop node with Ctrl+C

aztec-up alpha-testnet   # Step 3: Update

# Step 4:
aztec start --node --archiver --sequencer ...
```

> Ignore sync log errors. Focus on syncing block.

---

## 🧾 End of Setup

made by :--  💖Billionaire man
