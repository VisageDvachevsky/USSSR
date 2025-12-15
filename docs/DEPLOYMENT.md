# Deployment Guide

## Quick Start Deployment

The platform can be deployed to various free hosting services without requiring computational resources or investment.

## Deployment Options

### 1. Railway (Recommended)

**Step 1:** Install Railway CLI
```bash
npm install -g @railway/cli
```

**Step 2:** Login to Railway
```bash
railway login
```

**Step 3:** Initialize and Deploy
```bash
railway init
railway up
```

**Step 4:** Add Environment Variables
```bash
railway variables set SECRET_KEY=your-secret-key
railway variables set JWT_SECRET_KEY=your-jwt-secret
railway variables set ADMIN_PASSWORD=secure-password
```

**Railway automatically:**
- Detects the Dockerfile
- Provides a free PostgreSQL database
- Assigns a public URL
- Handles SSL certificates

### 2. Render

**Step 1:** Connect Repository
1. Go to https://render.com
2. Click "New +" → "Web Service"
3. Connect your GitHub repository

**Step 2:** Configure Service
- Build Command: `pip install -r backend/requirements.txt`
- Start Command: `gunicorn -w 4 -b 0.0.0.0:$PORT backend.app_enhanced:app`
- Environment: Python 3.11

**Step 3:** Add Environment Variables (in Render dashboard)
```
FLASK_ENV=production
SECRET_KEY=<generate-secure-key>
JWT_SECRET_KEY=<generate-secure-key>
```

### 3. Docker (Self-Hosted)

**Step 1:** Build and Run
```bash
docker-compose up -d
```

**Step 2:** Access Application
```
http://localhost:5000
```

## Environment Variables

Required variables:
```bash
FLASK_ENV=production
SECRET_KEY=<your-secret-key>
JWT_SECRET_KEY=<your-jwt-secret>
ADMIN_PASSWORD=<secure-admin-password>
```

Optional variables:
```bash
DATABASE_URL=<database-connection-string>
REDIS_URL=<redis-connection-string>
USE_HUGGINGFACE=true
AI_MODEL_NAME=sentence-transformers/all-MiniLM-L6-v2
```

## Post-Deployment

### Create Admin User
```bash
# Using CLI
flask create-admin

# Or via Python
python -c "from backend.app_enhanced import app; from backend.services.auth_service import AuthService; app.app_context().push(); AuthService.create_admin_user('admin', 'admin@example.com', 'secure-password')"
```

### Initialize Database
```bash
flask init-db
```

## Monitoring

- Health Check: `GET /health`
- Application logs available in hosting dashboard
- Activity logs stored in database

## Scaling

For production with high traffic:
1. Enable Redis for caching and rate limiting
2. Use PostgreSQL instead of SQLite
3. Increase gunicorn workers: `-w <num_workers>`
4. Enable horizontal scaling in hosting platform

## SSL/HTTPS

Both Railway and Render automatically provide SSL certificates.

For self-hosted deployments, use Let's Encrypt with Nginx or Caddy.
