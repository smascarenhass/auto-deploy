# Auto-Deploy

Automated deployment system that integrates with Git hooks for automatic deployment and Microsoft Teams notifications.

## 🚀 Features

- **Automatic Deployment**: Executes deployment automatically before each push
- **rsync Synchronization**: Syncs files with remote server
- **Service Restart**: Automatically restarts Docker containers
- **Teams Notifications**: Sends notifications to Microsoft Teams about deployments
- **Git Hooks**: Automatic integration with Git through hooks

## 📁 Project Structure

```
auto-deploy/
├── config/                 # Configuration files
├── githooks/              # Shared Git hooks
│   ├── deploy.sh          # Deployment script
│   ├── notify-teams.sh    # Teams notification script
│   └── pre-push           # Pre-push hook
├── scripts/               # Executable scripts
│   ├── deploy.sh          # Deployment script
│   └── notify-teams.sh    # Teams notification script
├── setup-hooks.sh         # Hook installation script
└── README.md              # This documentation
```

## 🛠️ Installation

1. **Clone the repository**:
   ```bash
   git clone <repository-url>
   cd auto-deploy
   ```

2. **Run the setup script**:
   ```bash
   chmod +x setup-hooks.sh
   ./setup-hooks.sh
   ```

3. **Configure environment variables** (if needed):
   - The script will create a `.env` file based on the example
   - Edit the `.env` file with your configurations

## ⚙️ Configuration

### Deployment Variables (`scripts/deploy.sh`)

```bash
REMOTE_USER="intao"                    # Remote server user
REMOTE_HOST="internal.intao.app"       # Server host
REMOTE_DIR="/home/intao/cockpit"       # Directory on server
LOCAL_DIR="."                          # Local directory
```

### Teams Configuration (`scripts/notify-teams.sh`)

```bash
API_URL="https://internal.intao.app/api/services/teams/send-message/git"
```

## 🔄 How It Works

### 1. Git Hook (pre-push)
- Intercepts all pushes
- Automatically executes the deployment script
- Only allows push if deployment is successful

### 2. Deployment Script
- Syncs files via rsync (excluding unnecessary files)
- Connects via SSH to remote server
- Restarts Docker containers (docker-compose or docker compose)
- Sends Teams notification if successful

### 3. Teams Notification
- Collects commit information (hash, message, author)
- Generates GitHub comparison URL
- Sends JSON payload to Teams API
- Only executes on pushes to `main` branch

## 📋 Files Excluded from Deployment

The deployment script automatically excludes:
- `venv/` - Python virtual environment
- `.git/` - Git directory
- `__pycache__/` - Python cache
- `*.pyc` - Compiled Python files
- `.env` - Environment variables
- `.DS_Store` - macOS files

## 🚨 Troubleshooting

### Deployment Fails
- Check SSH connectivity with server
- Confirm user has permissions on remote directory
- Verify Docker is installed on server

### Teams Notification Fails
- Confirm API URL is correct
- Check if repository is configured on GitHub
- Test connectivity with API

### Git Hook Doesn't Execute
- Run `./setup-hooks.sh` again
- Check script permissions: `chmod +x scripts/*.sh`
- Confirm hooks are in `.git/hooks/`

## 🔧 Useful Commands

```bash
# Install hooks
./setup-hooks.sh

# Run deployment manually
bash scripts/deploy.sh

# Test Teams notification
bash scripts/notify-teams.sh

# Check hook status
ls -la .git/hooks/

# View deployment logs
git push  # Executes deployment automatically
```

## 📝 Logs

The system uses colors to facilitate log reading:
- 🟢 **Green**: Success
- 🔴 **Red**: Error
- 🟡 **Yellow**: Warning

## 🤝 Contributing

1. Fork the project
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Open a Pull Request

## 📄 License

This project is under the MIT license. See the LICENSE file for more details.
