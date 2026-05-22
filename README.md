# EFAFA Scholarship Submission Portal

A secure, accessible web-based application portal for high school students to submit scholarship applications.

## Project Overview

This is a full-stack application consisting of:
- **Backend**: Python (Flask/FastAPI) REST API
- **Frontend**: Responsive web interface (HTML/CSS/JavaScript)
- **Database**: PostgreSQL

### Features
- Student registration and authentication
- Secure application submission
- Document upload and management
- Application status tracking
- Admin dashboard for reviewers
- Accessibility compliance (WCAG 2.1 AA)
- Security best practices (encryption, rate limiting, input validation)

## Tech Stack

- **Backend**: Python 3.9+, Flask or FastAPI
- **Database**: PostgreSQL 13+
- **Frontend**: HTML5, CSS3, JavaScript (Vanilla or lightweight framework)
- **Security**: JWT authentication, bcrypt password hashing, SQL injection prevention
- **Accessibility**: WCAG 2.1 AA standards

## Project Structure

```
efafa-board.github.io/
├── backend/                 # Python backend service
│   ├── app/
│   │   ├── __init__.py
│   │   ├── main.py         # Application entry point
│   │   ├── config.py       # Configuration management
│   │   ├── auth/           # Authentication & authorization
│   │   ├── routes/         # API endpoints
│   │   ├── models/         # Database models
│   │   ├── services/       # Business logic
│   │   └── utils/          # Helper functions
│   ├── migrations/         # Database migrations (Alembic)
│   ├── tests/              # Unit & integration tests
│   ├── requirements.txt    # Python dependencies
│   ├── .env.example        # Environment variables template
│   └── README.md
├── frontend/               # Web-based UI
│   ├── public/
│   │   └── index.html      # Main entry point
│   ├── css/
│   │   ├── styles.css      # Global styles
│   │   └── accessibility.css
│   ├── js/
│   │   ├── app.js          # Main application logic
│   │   ├── auth.js         # Authentication handling
│   │   └── api.js          # API communication
│   ├── pages/
│   │   ├── login.html
│   │   ├── register.html
│   │   ├── dashboard.html
│   │   ├── application.html
│   │   └── admin.html
│   └── README.md
├── docs/                   # Documentation
│   ├── API.md             # API documentation
│   ├── DATABASE.md        # Database schema
│   ├── SETUP.md           # Setup instructions
│   └── SECURITY.md        # Security guidelines
├── .github/
│   └── workflows/         # CI/CD workflows
├── docker-compose.yml     # Local development environment
├── .gitignore
└── LICENSE
```

## Getting Started

### Prerequisites
- Python 3.9 or higher
- PostgreSQL 13 or higher
- Docker & Docker Compose (optional, for containerized setup)
- Node.js 16+ (optional, for frontend build tools)

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/efafa-board/efafa-board.github.io.git
   cd efafa-board.github.io
   ```

2. **Set up backend**
   ```bash
   cd backend
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   pip install -r requirements.txt
   cp .env.example .env
   # Configure .env with your database credentials
   ```

3. **Set up database**
   ```bash
   alembic upgrade head
   ```

4. **Start backend server**
   ```bash
   python app/main.py
   ```

5. **Open frontend**
   - Open `frontend/public/index.html` in your browser, or
   - Serve with a local server: `python -m http.server 8080` in the frontend directory

### Docker Setup
```bash
docker-compose up -d
```

## Development

### API Documentation
See [API.md](docs/API.md) for detailed endpoint documentation.

### Database Schema
See [DATABASE.md](docs/DATABASE.md) for database structure.

### Security Guidelines
See [SECURITY.md](docs/SECURITY.md) for security best practices.

## Testing

```bash
cd backend
pytest tests/
```

## Contributing

Please read [CONTRIBUTING.md](CONTRIBUTING.md) for details on our code of conduct and pull request process.

## Security

This application handles sensitive student information. Please review [SECURITY.md](docs/SECURITY.md) before deploying to production.

- All passwords are hashed with bcrypt
- Database credentials are stored in environment variables
- HTTPS is required in production
- Input validation and SQL injection prevention
- CSRF protection enabled

## Accessibility

This portal is designed to meet WCAG 2.1 AA standards to ensure all students can access and use the application regardless of abilities.

## License

[Specify your license here]

## Contact

For questions or support, contact [support email]
