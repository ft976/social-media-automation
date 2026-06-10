# Social Media Automation Platform

A comprehensive, production-ready automation tool for managing and scheduling posts across multiple social media platforms.

## 🚀 Features

- **Multi-Platform Support**: Twitter, Facebook, Instagram, LinkedIn
- **Scheduled Posting**: Optimal posting times with timezone support
- **Content Management**: Centralized library and management
- **Analytics Dashboard**: Real-time metrics and engagement tracking
- **User Management**: Multi-user support with role-based access control
- **Template System**: Pre-built content templates
- **REST API**: Complete RESTful API for integrations
- **Webhook Support**: Real-time event notifications
- **Media Management**: Image/video upload and optimization

## 📊 Technology Stack

### Backend
- Python 3.11
- Django 4.2 + Django REST Framework
- PostgreSQL 15
- Celery + Redis
- JWT Authentication
- Docker & Docker Compose

### Frontend
- React 18 + TypeScript
- Tailwind CSS
- Zustand for state management
- TanStack Query for data fetching
- Vite for bundling

### DevOps
- Docker containerization
- Docker Compose orchestration
- GitHub Actions CI/CD
- Kubernetes ready
- Nginx reverse proxy

## 🛠️ Installation

### Prerequisites
- Docker & Docker Compose
- Git
- Python 3.9+ (for local development)
- Node.js 16+ (for local frontend development)

### Quick Start with Docker

```bash
# Clone the repository
git clone https://github.com/ft976/social-media-automation.git
cd social-media-automation

# Copy environment file
cp .env.example .env

# Start all services
docker-compose up -d

# Run database migrations
docker-compose exec backend python manage.py migrate

# Create superuser
docker-compose exec backend python manage.py createsuperuser

# Access the application
# Frontend: http://localhost:3000
# Backend API: http://localhost:8000/api
# Admin Panel: http://localhost:8000/admin
```

### Manual Setup (Development)

```bash
# Backend Setup
cd backend
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt
python manage.py migrate
python manage.py runserver

# Frontend Setup (in another terminal)
cd frontend
npm install
npm run dev
```

## 📁 Project Structure

```
social-media-automation/
├── backend/                          # Django REST API
│   ├── manage.py
│   ├── requirements.txt
│   ├── Dockerfile
│   ├── config/                       # Django settings
│   ├── apps/                         # Django applications
│   │   ├── users/                    # User management
│   │   ├── posts/                    # Post management
│   │   ├── accounts/                 # Social account management
│   │   ├── analytics/                # Analytics engine
│   │   └── templates/                # Content templates
│   ├── tests/                        # Unit and integration tests
│   └── logs/
│
├── frontend/                         # React SPA
│   ├── package.json
│   ├── Dockerfile
│   ├── src/
│   │   ├── components/               # React components
│   │   ├── pages/                    # Page components
│   │   ├── hooks/                    # Custom hooks
│   │   ├── store/                    # Zustand stores
│   │   ├── services/                 # API services
│   │   ├── types/                    # TypeScript types
│   │   └── App.tsx
│   ├── public/
│   └── vite.config.ts
│
├── docs/                             # Documentation
│   ├── ARCHITECTURE.md
│   ├── API.md
│   ├── DEPLOYMENT.md
│   └── CONTRIBUTING.md
│
├── .github/
│   └── workflows/
│       ├── ci.yml                    # CI pipeline
│       └── deploy.yml                # Deployment workflow
│
├── docker-compose.yml                # Development environment
├── docker-compose.prod.yml           # Production environment
├── .env.example                      # Environment template
├── .gitignore
├── LICENSE
└── CHANGELOG.md
```

## 🔌 API Endpoints

### Authentication
```
POST   /api/auth/register/        # Register new user
POST   /api/auth/login/           # User login
POST   /api/auth/refresh/         # Refresh JWT token
POST   /api/auth/logout/          # User logout
```

### Posts
```
GET    /api/posts/                # List all posts
POST   /api/posts/                # Create new post
GET    /api/posts/{id}/           # Get post details
PUT    /api/posts/{id}/           # Update post
DELETE /api/posts/{id}/           # Delete post
POST   /api/posts/{id}/publish/   # Publish immediately
POST   /api/posts/{id}/schedule/  # Schedule post
```

### Social Accounts
```
GET    /api/accounts/             # List connected accounts
POST   /api/accounts/             # Connect new account
GET    /api/accounts/{id}/        # Get account details
DELETE /api/accounts/{id}/        # Disconnect account
```

### Analytics
```
GET    /api/analytics/posts/                    # Post analytics
GET    /api/analytics/posts/{id}/               # Specific post analytics
GET    /api/analytics/accounts/                 # Account statistics
GET    /api/analytics/engagement/               # Engagement metrics
GET    /api/analytics/dashboard/                # Dashboard overview
```

### Templates
```
GET    /api/templates/             # List templates
POST   /api/templates/             # Create template
PUT    /api/templates/{id}/        # Update template
DELETE /api/templates/{id}/        # Delete template
```

## ⚙️ Environment Configuration

Copy `.env.example` to `.env` and update:

```env
# Database
DATABASE_URL=postgresql://user:password@db:5432/social_media
DATABASE_USER=postgres
DATABASE_PASSWORD=password

# Redis
REDIS_URL=redis://redis:6379/0

# Django
DEBUG=False
DJANGO_SECRET_KEY=your-secret-key-here
ALLOWED_HOSTS=localhost,127.0.0.1,yourdomain.com

# JWT
JWT_SECRET_KEY=your-jwt-secret
JWT_EXPIRATION_HOURS=24

# Social Media API Keys
TWITTER_API_KEY=your_key
TWITTER_API_SECRET=your_secret
FACEBOOK_APP_ID=your_app_id
FACEBOOK_APP_SECRET=your_secret
INSTAGRAM_ACCESS_TOKEN=your_token
LINKEDIN_ACCESS_TOKEN=your_token

# Email
EMAIL_HOST=smtp.gmail.com
EMAIL_PORT=587
EMAIL_USE_TLS=True
EMAIL_HOST_USER=your_email@gmail.com
EMAIL_HOST_PASSWORD=your_app_password

# AWS (Optional)
AWS_ACCESS_KEY_ID=your_key
AWS_SECRET_ACCESS_KEY=your_secret
AWS_STORAGE_BUCKET_NAME=your_bucket
```

## 🧪 Testing

```bash
# Backend tests
cd backend
pytest                          # Run all tests
pytest --cov                    # With coverage
pytest -v                       # Verbose output
pytest tests/test_posts.py      # Specific test file

# Frontend tests
cd frontend
npm test                        # Run tests
npm test -- --coverage         # With coverage
npm test -- --watch            # Watch mode
```

## 🐳 Docker Commands

```bash
# Build images
docker-compose build

# Start services
docker-compose up -d

# Stop services
docker-compose down

# View logs
docker-compose logs -f backend
docker-compose logs -f frontend

# Run commands in container
docker-compose exec backend python manage.py migrate
docker-compose exec backend python manage.py createsuperuser

# Scale workers
docker-compose up -d --scale celery=3
```

## 🚀 Deployment

### Using Docker Compose (Production)

```bash
docker-compose -f docker-compose.prod.yml up -d
```

### Using Kubernetes

```bash
kubectl apply -f k8s/
kubectl get pods
kubectl logs -f deployment/social-media-backend
```

### Using GitHub Actions

Push to `main` branch to trigger automatic deployment:
```bash
git add .
git commit -m "Your changes"
git push origin main
```

See `.github/workflows/deploy.yml` for deployment configuration.

## 📚 Documentation

- [Architecture Guide](docs/ARCHITECTURE.md) - System design and components
- [API Documentation](docs/API.md) - Detailed API reference
- [Deployment Guide](docs/DEPLOYMENT.md) - Production deployment
- [Contributing Guide](docs/CONTRIBUTING.md) - Contribution guidelines

## 🔐 Security Features

- JWT token-based authentication
- Role-based access control (RBAC)
- CORS protection
- Rate limiting
- SQL injection prevention
- XSS protection
- CSRF protection
- Encrypted sensitive data
- Secure password hashing (bcrypt)

## 📈 Performance

- Redis caching layer
- Database connection pooling
- Async task processing with Celery
- CDN-ready static files
- Database query optimization
- API response caching

## 🤝 Contributing

See [CONTRIBUTING.md](docs/CONTRIBUTING.md) for guidelines.

```bash
git checkout -b feature/amazing-feature
git commit -m 'Add amazing feature'
git push origin feature/amazing-feature
```

## 📝 License

MIT License - See [LICENSE](LICENSE) file for details.

## 💬 Support

- 📧 Email: support@socialmediaautomation.com
- 🐛 Issues: [GitHub Issues](https://github.com/ft976/social-media-automation/issues)
- 💡 Discussions: [GitHub Discussions](https://github.com/ft976/social-media-automation/discussions)

## 👨‍💻 Authors

- **ft976** - Project Creator

## 📊 Status

- ✅ Backend API - Complete
- ✅ Frontend UI - Complete
- ✅ Database Schema - Complete
- ✅ Docker Setup - Complete
- ✅ CI/CD Pipeline - Complete
- ✅ Documentation - Complete
- 🚀 Ready for Deployment

## 🎯 Roadmap

- [ ] Mobile app (React Native)
- [ ] Advanced AI-powered content suggestions
- [ ] Video content support
- [ ] Live streaming integration
- [ ] Social listening feature
- [ ] Competitor analysis
- [ ] Team collaboration tools
- [ ] White-label solution

---

**Version**: 1.0.0 | **Last Updated**: 2026-06-10