# Architecture Documentation

## System Overview

The USSR Leaders Platform is a production-ready web application built with modern best practices, designed for scalability, maintainability, and performance.

## Technology Stack

### Backend
- **Flask 3.0.0** - Modern Python web framework
- **SQLAlchemy** - ORM for database operations
- **Flask-Migrate** - Database migrations
- **Flask-JWT-Extended** - JWT authentication
- **Flask-Limiter** - Rate limiting
- **Flask-Caching** - Response caching
- **Sentence Transformers** - AI-powered semantic search
- **Gunicorn** - Production WSGI server

### Frontend
- **HTML5** - Semantic markup
- **CSS3** - Modern styling with animations
- **Vanilla JavaScript** - No framework dependencies
- **Google Fonts** - Typography

### Database
- **SQLite** - Development (file-based)
- **PostgreSQL** - Production (recommended)

### Deployment
- **Docker** - Containerization
- **Railway/Render** - Free hosting platforms
- **GitHub Actions** - CI/CD pipeline

## Architecture Patterns

### Application Factory Pattern
```python
def create_app(config_name=None):
    app = Flask(__name__)
    # Configure and initialize extensions
    return app
```

### Bluprint-Based Routing
- `/api/leaders` - Leader operations
- `/api/auth` - Authentication
- `/api/analytics` - Analytics and reporting

### Service Layer Pattern
- **AuthService** - User authentication and authorization
- **EnhancedAIService** - AI-powered features

### Repository Pattern
- Models encapsulate database operations
- Clean separation of concerns

## Database Schema

### Leaders Table
```sql
CREATE TABLE leaders (
    id INTEGER PRIMARY KEY,
    name_ru TEXT NOT NULL,
    name_en TEXT NOT NULL,
    slug TEXT UNIQUE,
    birth_year INTEGER,
    death_year INTEGER,
    position TEXT,
    achievements TEXT,
    video_id INTEGER,
    view_count INTEGER DEFAULT 0,
    is_published BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP,
    updated_at TIMESTAMP
);
```

### Users Table
```sql
CREATE TABLE users (
    id INTEGER PRIMARY KEY,
    username TEXT UNIQUE NOT NULL,
    email TEXT UNIQUE NOT NULL,
    password_hash TEXT NOT NULL,
    role_id INTEGER REFERENCES roles(id),
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP
);
```

### Roles Table
```sql
CREATE TABLE roles (
    id INTEGER PRIMARY KEY,
    name TEXT UNIQUE NOT NULL,
    permissions JSON
);
```

### Activity Logs Table
```sql
CREATE TABLE activity_logs (
    id INTEGER PRIMARY KEY,
    user_id INTEGER REFERENCES users(id),
    leader_id INTEGER REFERENCES leaders(id),
    action TEXT NOT NULL,
    details JSON,
    timestamp TIMESTAMP
);
```

## Security Features

1. **Authentication**
   - JWT-based token authentication
   - Bcrypt password hashing
   - Refresh token support

2. **Authorization**
   - Role-based access control (RBAC)
   - Permission-based endpoints
   - Three roles: guest, user, admin

3. **Rate Limiting**
   - Per-endpoint rate limits
   - IP-based tracking
   - Memory or Redis storage

4. **Input Validation**
   - Request data validation
   - SQL injection prevention
   - XSS protection

5. **CORS**
   - Configurable origins
   - Secure headers

## AI/ML Features

### Semantic Search
- Sentence-BERT embeddings
- Cosine similarity matching
- Fallback to keyword search

### Fact Generation
- Curated facts database
- Context-aware recommendations
- Similar leader suggestions

## Caching Strategy

1. **Application-Level Caching**
   - Leader data (5 minutes)
   - Search results (10 minutes)
   - AI-generated facts (10 minutes)

2. **HTTP Caching**
   - Static assets caching
   - ETag support

## Logging

### Log Levels
- DEBUG: Development debugging
- INFO: General information
- WARNING: Warning messages
- ERROR: Error conditions
- CRITICAL: Critical failures

### Log Destinations
- Console: Development
- File: Production (with rotation)
- Database: Activity tracking

## Performance Optimizations

1. **Database**
   - Indexed columns
   - Eager loading for relationships
   - Connection pooling

2. **Caching**
   - Response caching
   - Model caching
   - AI model caching

3. **Frontend**
   - CSS/JS minification (production)
   - Lazy loading for images
   - Debounced search

## Scalability

### Horizontal Scaling
- Stateless application design
- Session data in JWT tokens
- External cache (Redis)

### Vertical Scaling
- Gunicorn worker processes
- Database connection pooling
- Async operations where beneficial

## Monitoring

### Health Checks
- `/health` endpoint
- Database connectivity
- Service status

### Metrics
- Request count
- Response times
- Error rates
- Active users

### Logging
- Application logs
- Access logs
- Error logs
- Activity logs

## Deployment Architecture

```
┌─────────────┐
│   Client    │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│   Nginx/    │
│   Caddy     │ (Optional SSL termination)
└──────┬──────┘
       │
       ▼
┌─────────────┐
│  Gunicorn   │ (WSGI Server)
│  (Workers)  │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│   Flask     │ (Application)
│  Blueprints │
└──────┬──────┘
       │
   ┌───┴───┐
   ▼       ▼
┌─────┐ ┌─────┐
│ DB  │ │Redis│ (Optional)
└─────┘ └─────┘
```

## Development Workflow

1. **Local Development**
   ```bash
   python backend/app_enhanced.py
   ```

2. **Testing**
   ```bash
   pytest backend/tests
   ```

3. **Code Quality**
   ```bash
   flake8 backend
   mypy backend
   black backend
   ```

4. **Database Migrations**
   ```bash
   flask db migrate -m "description"
   flask db upgrade
   ```

## Configuration Management

### Environment-Based Config
- Development
- Testing
- Production

### Environment Variables
- Secrets in `.env`
- Configuration in code
- Validation on startup

## Error Handling

### Global Error Handlers
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 429: Rate Limit Exceeded
- 500: Internal Server Error

### Logging
- All errors logged
- Stack traces in development
- User-friendly messages in production

## Future Enhancements

1. **Advanced AI**
   - GPT integration
   - Multi-language support
   - Voice synthesis

2. **Real-time Features**
   - WebSocket support
   - Live notifications
   - Real-time analytics

3. **Content Management**
   - Admin panel
   - Content moderation
   - Media upload

4. **Social Features**
   - User comments
   - Ratings and reviews
   - Social sharing
