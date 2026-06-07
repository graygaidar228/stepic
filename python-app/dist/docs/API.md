# API Documentation

## Jenkins Python Application

**Version:** 2.0.0
**Build:** 999

### Endpoints

#### GET /
Main application page with UI

#### GET /api/info
Returns application information in JSON format

**Response:**
```json
{
  "application": "jenkins-python-app",
  "version": "x.x.x",
  "build": "xxx",
  "environment": "xxx"
}
```

#### GET /api/health
Health check endpoint

**Response:**
```json
{
  "status": "healthy",
  "version": "x.x.x"
}
```

#### GET /api/metrics
Application metrics

**Response:**
```json
{
  "metrics": {
    "python_version": "x.x.x",
    "platform": "xxx"
  }
}
```

## Running the Application

```bash
pip install -r requirements.txt
python app.py
```

## Environment Variables

- `APP_VERSION` - Application version
- `BUILD_NUMBER` - Build number
- `ENVIRONMENT` - Environment name (development/staging/production)
- `PORT` - Server port (default: 5000)
- `DEBUG` - Debug mode (true/false)

Generated: 2025-10-14T20:06:35.039274
