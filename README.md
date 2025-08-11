# N8N + WAHA Workflow Setup

This workspace contains a complete setup for running N8N (workflow automation) and WAHA (WhatsApp HTTP API) with Nginx proxy and basic authentication.

## 🚀 Services Overview

- **N8N**: Workflow automation platform
- **WAHA**: WhatsApp HTTP API 
- **Nginx**: Reverse proxy with basic authentication

## 📋 Prerequisites

- Docker and Docker Compose installed
- `.env` file configured (see Environment Variables section)
- Basic auth credentials set up

## 🔧 Environment Variables

Create a `.env` file in the root directory with the following variables:

```env
# N8N Configuration
N8N_PORT=9000
N8N_BASIC_AUTH_USER=your_username
N8N_BASIC_AUTH_PASSWORD=your_password

# WAHA Configuration  
WAHA_PORT=8000
```

## 🌐 Access URLs

### Protected Services (Requires Basic Auth)

- **N8N Web Interface**: `http://your-server:5678/`
  - Username/Password: Set in `.env` file
  - Full workflow automation platform

- **WAHA Web Interface**: `http://your-server:3000/`
  - Username/Password: Set in `.env` file
  - WhatsApp API documentation and swagger UI

- **WAHA Dashboard**: `http://your-server:3000/dashboard/`
  - Username/Password: Set in `.env` file
  - WAHA management dashboard

### Public Services (No Authentication)

- **N8N Webhooks**: `http://your-server/webhook/`
  - For external integrations and triggers
  - No authentication required for incoming webhooks

- **WAHA API Endpoints**: `http://your-server/api/`
  - For automation and integrations
  - Direct API access without authentication

## 🚀 Getting Started

1. **Clone and Setup**
   ```bash
   git clone https://github.com/novianr90/workflow-n8n.git
   cd workflow-n8n
   ```

2. **Create Environment File**
   ```bash
   cp .env.example .env
   # Edit .env with your credentials
   ```

3. **Create Basic Auth File**
   ```bash
   mkdir -p nginx-auth
   htpasswd -c nginx-auth/.htpasswd your_username
   ```

4. **Start Services**
   ```bash
   docker-compose up -d
   ```

5. **Check Status**
   ```bash
   docker-compose ps
   docker-compose logs nginx-proxy
   ```

## 📁 Directory Structure

```
wf/
├── docker-compose.yml          # Main Docker Compose configuration
├── nginx.conf                  # Nginx reverse proxy configuration
├── .env                        # Environment variables (create this)
├── nginx-auth/
│   └── .htpasswd              # Basic auth credentials (create this)
├── n8n_data/                  # N8N persistent data
│   ├── config
│   ├── database.sqlite
│   └── ...
└── bootstrap.sh               # N8N initialization script
```

## 🔐 Security Features

- **Basic Authentication**: All web interfaces protected with username/password
- **Isolated Services**: N8N and WAHA containers not directly exposed
- **Rate Limiting**: Applied to webhook and API endpoints
- **Secure Proxy**: All traffic routed through Nginx with proper headers

## 🛠 Management Commands

### Start Services
```bash
docker-compose up -d
```

### Stop Services
```bash
docker-compose down
```

### Restart Nginx Only
```bash
docker-compose restart nginx-proxy
```

### View Logs
```bash
# All services
docker-compose logs -f

# Specific service
docker-compose logs -f nginx-proxy
docker-compose logs -f n8n
docker-compose logs -f waha
```

### Update Services
```bash
docker-compose pull
docker-compose up -d
```

## 🔍 Troubleshooting

### Check Service Health
```bash
docker-compose ps
```

### Test Basic Auth
```bash
curl -u username:password http://your-server:5678/
```

### Check Nginx Configuration
```bash
docker-compose exec nginx-proxy nginx -t
```

### Reset N8N Data
```bash
docker-compose down
sudo rm -rf n8n_data/*
docker-compose up -d
```

## 📡 API Usage Examples

### N8N Webhook Example
```bash
# Send data to N8N workflow
curl -X POST http://your-server/webhook/your-webhook-id \
  -H "Content-Type: application/json" \
  -d '{"message": "Hello from external system"}'
```

### WAHA API Example
```bash
# Get WAHA sessions
curl http://your-server/api/sessions

# Send WhatsApp message
curl -X POST http://your-server/api/sendText \
  -H "Content-Type: application/json" \
  -d '{
    "session": "default",
    "chatId": "1234567890@c.us",
    "text": "Hello from WAHA!"
  }'
```

## 🔄 Backup and Restore

### Backup N8N Data
```bash
tar -czf n8n_backup_$(date +%Y%m%d).tar.gz n8n_data/
```

### Restore N8N Data
```bash
docker-compose down
tar -xzf n8n_backup_YYYYMMDD.tar.gz
docker-compose up -d
```

## 📝 Notes

- N8N has built-in basic auth that's disabled in favor of Nginx auth
- WAHA dashboard assets are properly proxied through Nginx
- Webhook and API endpoints are intentionally public for automation
- All containers communicate through internal Docker network
- Nginx handles SSL termination if certificates are configured

## 🆘 Support

For issues and questions:
1. Check Docker logs: `docker-compose logs`
2. Verify `.env` file configuration
3. Ensure basic auth file exists and is readable
4. Check network connectivity between containers

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

This setup is provided as-is for development and production use.
