# GitHub Actions Setup Guide

## 🔧 Setup GitHub Secrets

Go to your GitHub repo → Settings → Secrets and variables → Actions → New repository secret

### Required Secrets:

1. **VPS_HOST**
   ```
   your.vps.ip.address
   ```

2. **VPS_USER**
   ```
   your_username
   ```

3. **VPS_SSH_KEY**
   ```
   # Your private SSH key content
   # Generate with: ssh-keygen -t rsa -b 4096 -C "github-actions"
   # Copy from: cat ~/.ssh/id_rsa
   -----BEGIN OPENSSH PRIVATE KEY-----
   YOUR_PRIVATE_KEY_CONTENT_HERE
   -----END OPENSSH PRIVATE KEY-----
   ```

4. **VPS_PORT** (optional, default 22)
   ```
   22
   ```

## 🔑 SSH Key Setup

### On your local machine:
```bash
# Generate new SSH key for GitHub Actions
ssh-keygen -t rsa -b 4096 -C "github-actions" -f ~/.ssh/github_actions

# Copy public key to VPS
ssh-copy-id -i ~/.ssh/github_actions.pub your_username@your.vps.ip.address

# Copy private key content for GitHub secret
cat ~/.ssh/github_actions
```

### On your VPS:
```bash
# Make sure the public key is in authorized_keys
cat ~/.ssh/authorized_keys
```

## 🚀 How it works:

1. **Push to master** → Auto deploy
2. **Manual trigger** → Go to Actions tab → Run workflow
3. **Health checks** → Automatic service verification
4. **Backup** → Auto backup before deployment
5. **Cleanup** → Remove old Docker images

## 📁 Workflow Files:

- `deploy.yml` - Simple deployment
- `deploy-advanced.yml` - With health checks and backup

Choose one based on your needs!

## 🔍 Monitoring:

After setup, every push will:
- ✅ Pull latest code
- ✅ Restart services
- ✅ Health check
- ✅ Clean up
- ✅ Show status

Check the Actions tab for deployment status and logs!
