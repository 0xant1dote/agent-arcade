# Getting Started with Agent Arcade

This guide will help you get up and running with Agent Arcade, including troubleshooting common installation issues.

## System Requirements

Before you begin, ensure your system meets these requirements:

- **Python**: Version 3.8 - 3.12 (3.13 not yet supported)
- **Operating System**: Linux, macOS, or WSL2 on Windows
- **Node.js & npm**: Version 14 or higher (for NEAR CLI)
- **Storage**: At least 2GB free space
- **Memory**: At least 4GB RAM recommended

## Installation

1. **Clone the Repository**:
```bash
git clone https://github.com/jbarnes850/agent-arcade.git
cd agent-arcade
```

2. **Run the Installation Script**:
```bash
chmod +x install.sh
./install.sh
```

The script will:
- Create a Python virtual environment
- Install all required dependencies
- Set up Atari ROMs
- Install and configure NEAR CLI

## Troubleshooting Installation

### Python Version Issues

If you see Python version errors:

1. **Check Current Version**:
```bash
python3 --version
```

2. **Install Compatible Version**:

On macOS:
```bash
brew install python@3.12
brew link python@3.12
```

On Ubuntu/Debian:
```bash
sudo apt-get update
sudo apt-get install python3.12 python3.12-venv
```

On Windows (WSL2):
```bash
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt-get update
sudo apt-get install python3.12 python3.12-venv
```

### Atari ROM Installation Issues

If you encounter ROM installation problems:

1. **Install Dependencies in Order**:

```bash
# First, install gymnasium with Atari support
pip install "gymnasium[atari]==0.28.1"

# Then install ALE-py
pip install "ale-py==0.8.1"

# Finally install AutoROM
pip install "AutoROM[accept-rom-license]==0.6.1"
```

2. **Install ROMs**:

```bash
# Method 1: Using AutoROM (preferred)
python3 -m AutoROM --accept-license

# Method 2: Using pip package
pip install autorom.accept-rom-license

# Method 3: Manual Installation
# Only use this if methods 1 and 2 fail
ROMS_DIR="$HOME/.local/lib/python3.*/site-packages/ale_py/roms"
mkdir -p "$ROMS_DIR"
wget https://github.com/openai/atari-py/raw/master/atari_py/atari_roms/pong.bin -P "$ROMS_DIR"
wget https://github.com/openai/atari-py/raw/master/atari_py/atari_roms/space_invaders.bin -P "$ROMS_DIR"
```

3. **Verify Installation**:

```bash
# Verify ALE interface
python3 -c "from ale_py import ALEInterface; ALEInterface()"

# Test specific games
python3 -c "import gymnasium; gymnasium.make('ALE/Pong-v5')"
python3 -c "import gymnasium; gymnasium.make('ALE/SpaceInvaders-v5')"
```

4. **Common ROM Issues**:
   - **ROM not found**: Make sure ROMs are in the correct directory
   - **Permission errors**: Check directory permissions with `ls -la $HOME/.local/lib/python3.*/site-packages/ale_py/roms`
   - **Import errors**: Ensure packages are installed in the correct order
   - **Version conflicts**: Use the exact versions specified above

### Package Installation Issues

1. **Clean Installation**:

```bash
# Remove existing virtual environment
rm -rf drl-env

# Create new environment
python3 -m venv drl-env
source drl-env/bin/activate

# Upgrade pip
pip install --upgrade pip

# Install with verbose output
pip install -e . -v
```

2. **Dependency Conflicts**:

```bash
# Install specific versions
pip install "gymnasium[atari]==0.28.1"
pip install "stable-baselines3==2.0.0"
pip install "ale-py==0.8.1"
pip install "AutoROM[accept-rom-license]==0.6.1"
```

### NEAR CLI Issues

1. **Node.js Installation**:

On macOS:

```bash
brew install node@14
```

On Ubuntu/Debian:

```bash
curl -fsSL https://deb.nodesource.com/setup_14.x | sudo -E bash -
sudo apt-get install -y nodejs
```

2. **NEAR CLI Installation**:

```bash
# Remove existing installation
npm uninstall -g near-cli

# Clear npm cache
npm cache clean --force

# Install NEAR CLI
npm install -g near-cli
```

## First Steps

After successful installation:

1. **Verify CLI Installation**:

```bash
agent-arcade --version
```

2. **List Available Games**:

```bash
agent-arcade list-games
```

3. **Train Your First Agent**:

```bash
# Train Pong agent with visualization
agent-arcade train pong --render
```

4. **Evaluate Your Agent**:

```bash
agent-arcade evaluate pong --model models/pong_final.zip
```

5. **Login to NEAR**:

```bash
agent-arcade wallet-cmd login
# Optional: Specify network and account
agent-arcade wallet-cmd login --network testnet --account-id your-account.testnet
```

6. **Stake on Performance**:

```bash
agent-arcade stake place pong --model models/pong_final.zip --amount 10 --target-score 15
```

## Training Your First Agent

To start training an agent:

```bash
# Train Pong agent with visualization
agent-arcade train pong --render
```

The initial training will run for 250,000 timesteps (about 30-45 minutes) to give you a good baseline model. You'll see:

1. Progress updates every 1000 steps showing:
   - Percentage complete
   - Current step count
   - Estimated time remaining

2. Real-time metrics in TensorBoard:

   ```bash
   tensorboard --logdir ./tensorboard
   ```

   Visit http://localhost:6006 to view:
   - Learning progress
   - Score improvements
   - Training speed (FPS)

## Training Duration Guide

- **Quick Training (30-45 min)**
  - 250,000 timesteps (default)
  - Good for initial testing
  - Suitable for simple games like Pong

- **Full Training (2-4 hours)**
  - 1,000,000 timesteps
  - Better performance
  - Required for complex games

To run longer training:

```bash
agent-arcade train pong --timesteps 1000000
```

## Monitoring Tips

1. Watch the terminal for progress updates
2. Use TensorBoard for detailed metrics
3. Models are saved automatically when training completes
4. Use `--no-render` for faster training

## Common Error Messages

1. **"ImportError: No module named 'imp'"**:
   - This error occurs with Python 3.13
   - Solution: Use Python 3.12 or lower

2. **"ModuleNotFoundError: No module named 'ale_py'"**:
   - Solution: Reinstall ALE-py

   ```bash
   pip install ale-py==0.8.1
   ```

3. **"Error: Cannot find module 'near-api-js'"**:
   - Solution: Reinstall NEAR CLI

   ```bash
   npm install -g near-cli
   ```

4. **"ROM not found"**:
   - Solution: Follow manual ROM installation steps above

## Getting Help

If you encounter issues not covered here:

1. Check the [GitHub Issues](https://github.com/your-username/agent-arcade/issues)
2. Join our [Discord Community](https://discord.gg/your-invite)
3. Create a new issue with:
   - Your system information
   - Error message
   - Steps to reproduce
   - Logs from `install.sh`

## NEAR Integration Setup

If you want to participate in competitions and staking:

1. **Install Node.js and npm**:
   - Download from https://nodejs.org/
   - Version 14.x or higher required

2. **Install NEAR CLI**:

```bash
npm install -g near-cli
```

3. **Install Staking Dependencies**:

```bash
pip install -e ".[staking]"
```

4. **Create NEAR Account**:
   - Visit https://wallet.near.org/
   - Follow account creation process
   - Save your account ID

5. **Login to NEAR**:

```bash
agent-arcade wallet-cmd login
# Optional: Specify network and account
agent-arcade wallet-cmd login --network testnet --account-id your-account.testnet
```

6. **Verify Setup**:

```bash
# Check wallet status
agent-arcade wallet-cmd status

# Logout when needed
agent-arcade wallet-cmd logout
```

## Troubleshooting NEAR Integration

### Node.js/npm Issues:

```bash
# Check Node.js version
node --version  # Should be >= 14.0.0

# Check npm version
npm --version

# Update npm if needed
npm install -g npm
```

### NEAR CLI Issues:

```bash
# Reinstall NEAR CLI
npm uninstall -g near-cli
npm install -g near-cli

# Verify installation
near --version
```

### Staking Issues:

```bash
# Clean install staking dependencies
pip uninstall -y agent-arcade
pip install -e ".[staking]"

# Verify RPC connection
agent-arcade wallet-cmd status
```
