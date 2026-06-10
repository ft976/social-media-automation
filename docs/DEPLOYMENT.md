# Deployment Guide - Vercel & Production Ready

## Frontend Deployment on Vercel

### Prerequisites
- Vercel account (https://vercel.com)
- GitHub repository connected to Vercel
- Environment variables configured

### Step 1: Connect GitHub Repository

1. Go to https://vercel.com/new
2. Select "Import Git Repository"
3. Choose your GitHub repository
4. Click "Import"

### Step 2: Configure Environment Variables

In Vercel dashboard, go to Settings → Environment Variables and add:

```
REACT_APP_API_URL=https://your-backend-api.herokuapp.com/api
REACT_APP_ENV=production
```

### Step 3: Configure Build Settings

- **Framework**: Vite
- **Build Command**: `npm run build`
- **Output Directory**: `dist`
- **Install Command**: `npm ci`

### Step 4: Deploy

```bash
# Automatic deployment on push to main
git push origin main

# Manual deployment using Vercel CLI
npm install -g vercel
vercel --prod
```

### Step 5: Setup Custom Domain (Optional)

1. In Vercel dashboard, go to Settings → Domains
2. Add your custom domain
3. Follow DNS configuration instructions

## Backend Deployment on Heroku

### Prerequisites
- Heroku account (https://heroku.com)
- Heroku CLI installed
- PostgreSQL database addon

### Step 1: Create Heroku App

```bash
# Login to Heroku
heroku login

# Create app
heroku create your-app-name

# Add PostgreSQL addon
heroku addons:create heroku-postgresql:hobby-dev -a your-app-name
```

### Step 2: Set Environment Variables

```bash
heroku config:set DJANGO_SECRET_KEY=your-secret-key -a your-app-name
heroku config:set DEBUG=False -a your-app-name
heroku config:set ALLOWED_HOSTS=your-app-name.herokuapp.com -a your-app-name

# Social Media API Keys
heroku config:set TWITTER_API_KEY=your_key -a your-app-name
heroku config:set FACEBOOK_APP_ID=your_id -a your-app-name
# ... etc
```

### Step 3: Deploy Backend

```bash
# Add Heroku remote
heroku git:remote -a your-app-name

# Deploy
git push heroku main

# Run migrations
heroku run python manage.py migrate -a your-app-name

# Create superuser
heroku run python manage.py createsuperuser -a your-app-name
```

### Step 4: Setup Celery Workers

```bash
# Add Redis addon
heroku addons:create heroku-redis:premium-0 -a your-app-name

# Add worker dynos in Procfile (create if doesn't exist)
cat > Procfile << EOF
web: gunicorn config.wsgi --log-file -
worker: celery -A config worker -l info
beat: celery -A config beat -l info
EOF

git add Procfile
git commit -m "Add Procfile for Heroku deployment"
git push heroku main
```

## Backend Deployment on AWS

### Using AWS Elastic Beanstalk

```bash
# Install EB CLI
pip install awsebcli

# Initialize EB app
eb init -p python-3.11 social-media-automation

# Create environment
eb create production

# Set environment variables
eb setenv DJANGO_SECRET_KEY=your-secret-key DEBUG=False

# Deploy
eb deploy

# View logs
eb logs
```

## Database Setup

### Heroku PostgreSQL

```bash
# Get database URL
heroku config:get DATABASE_URL -a your-app-name

# This is automatically set as DATABASE_URL environment variable
```

### AWS RDS

```bash
# Create RDS instance via AWS Console
# Get connection details and set:
heroku config:set DATABASE_URL=postgresql://user:password@your-rds-endpoint:5432/dbname
```

## Redis Setup

### Heroku Redis

```bash
heroku addons:create heroku-redis:premium-0 -a your-app-name
```

### AWS ElastiCache

```bash
# Create ElastiCache cluster via AWS Console
# Get endpoint and set:
heroku config:set REDIS_URL=redis://endpoint:6379/0
```

## Monitoring & Logging

### Sentry Setup

```bash
# Install Sentry SDK
pip install sentry-sdk

# Set Sentry DSN
heroku config:set SENTRY_DSN=your-sentry-dsn -a your-app-name
```

### Logs

```bash
# View Heroku logs
heroku logs -f -a your-app-name

# View Vercel logs
vercel logs [url]
```

## SSL/HTTPS

### Vercel
- Automatic HTTPS provided by Vercel

### Heroku
- Automatic HTTPS for herokuapp.com domains
- Add custom domain and configure DNS

## Performance Optimization

### Frontend (Vercel)
```bash
# Build optimization
npm run build

# Vercel automatically:
- Compresses assets
- Optimizes images
- Serves from CDN
- Minifies code
```

### Backend (Heroku/AWS)
```bash
# Enable caching
heroku config:set DJANGO_CACHE=redis

# Optimize database
python manage.py migrate
python manage.py collectstatic

# Use gunicorn with multiple workers
web: gunicorn config.wsgi --workers 4 --worker-class sync
```

## Continuous Deployment

### GitHub Actions + Vercel

The project includes automatic deployment workflows. Push to main branch triggers:
1. Backend tests
2. Frontend tests
3. Security scanning
4. Automatic deployment to Vercel

### Manual Deployment

```bash
# Frontend
vercel --prod

# Backend
heroku deploy
```

## Rollback Procedure

### Vercel
```bash
vercel deployments ls
vercel rollback [deployment-id]
```

### Heroku
```bash
# View releases
heroku releases -a your-app-name

# Rollback to previous release
heroku releases:rollback -a your-app-name
```

## Custom Domain Setup

### Vercel
1. Settings → Domains
2. Add domain
3. Update DNS records (Vercel provides them)

### Heroku
1. CLI: `heroku domains:add yourdomain.com`
2. Update DNS records
3. Verify with `heroku domains`

## Troubleshooting

### Frontend Not Loading
```bash
# Check build logs
vercel logs [url] --follow

# Verify environment variables
vercel env ls
```

### Backend API Errors
```bash
# Check Heroku logs
heroku logs -f

# Check database connection
heroku pg:info

# Run migrations
heroku run python manage.py migrate
```

### Static Files Not Loading
```bash
# Collect static files
heroku run python manage.py collectstatic

# Clear cache
heroku restart
```

## Backup & Recovery

### Database Backup

```bash
# Heroku PostgreSQL backup
heroku pg:backups capture -a your-app-name

# Download backup
heroku pg:backups download -a your-app-name

# Restore from backup
heroku pg:backups restore [backup-id] -a your-app-name
```

## Cost Optimization

- **Vercel**: Free tier includes deployment
- **Heroku**: Use hobby-dev ($7/month) for development
- **PostgreSQL**: Hobby-dev $9/month
- **Redis**: Premium-0 $30/month
- **Total estimated**: ~$46-100/month for production