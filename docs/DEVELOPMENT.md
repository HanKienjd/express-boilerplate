# Development Guide

This document provides detailed instructions for setting up and developing the Express.js E-commerce API Boilerplate.

## 🚀 Quick Start

### Environment Variables Setup

Create a `.env` file in the root directory with the following variables:

```env
# =================================
# Database Configuration
# =================================
DB_CLIENT=mysql
DB_HOST=localhost
DB_DATABASE=app
DB_USERNAME=app
DB_PASSWORD=123456

# =================================
# JWT Configuration
# =================================
JWT_SECRET=your-super-secret-jwt-key-here
JWT_EXPIRES_IN=7d

# =================================
# Email Configuration
# =================================
MAIL_HOST=smtp.gmail.com
MAIL_PORT=587
MAIL_USERNAME=your-email@gmail.com
MAIL_PASSWORD=your-app-password
MAIL_FROM=your-email@gmail.com

# =================================
# Cloudinary Configuration
# =================================
CLOUDINARY_CLOUD_NAME=your-cloud-name
CLOUDINARY_API_KEY=your-api-key
CLOUDINARY_API_SECRET=your-api-secret

# =================================
# AWS S3 Configuration
# =================================
AWS_ACCESS_KEY_ID=your-access-key
AWS_SECRET_ACCESS_KEY=your-secret-key
AWS_REGION=us-east-1
AWS_BUCKET=your-bucket-name

# =================================
# Application Configuration
# =================================
NODE_ENV=development
PORT=8080
TZ=UTC

# =================================
# Google Services (Optional)
# =================================
GOOGLE_CLIENT_ID=your-google-client-id
GOOGLE_CLIENT_SECRET=your-google-client-secret

# =================================
# MinIO Configuration (Optional)
# =================================
MINIO_ACCESS_KEY=app
MINIO_SECRET_KEY=secretsecret
MINIO_REGION=us-west-2
MINIO_ENDPOINT=http://localhost:9000

# =================================
# Rate Limiting (Optional)
# =================================
RATE_LIMIT_WINDOW_MS=900000
RATE_LIMIT_MAX_REQUESTS=100

# =================================
# Logging Configuration
# =================================
LOG_LEVEL=info
LOG_FILE=app.log
```

## 🗃️ Database Setup

### MySQL Database Setup

1. **Install MySQL**
   ```bash
   # On macOS
   brew install mysql
   
   # On Ubuntu
   sudo apt-get install mysql-server
   
   # On Windows
   # Download from https://dev.mysql.com/downloads/mysql/
   ```

2. **Create Database and User**
   ```sql
   CREATE DATABASE app;
   CREATE USER 'app'@'localhost' IDENTIFIED BY '123456';
   GRANT ALL PRIVILEGES ON app.* TO 'app'@'localhost';
   FLUSH PRIVILEGES;
   ```

3. **Run Migrations**
   ```bash
   npm run migrate:run
   ```

4. **Seed Initial Data**
   ```bash
   npm run seed:run
   ```

### Database Management Commands

```bash
# Migration Commands
npm run migrate:run      # Run all pending migrations
npm run migrate:up       # Run the next migration
npm run migrate:down     # Rollback the last migration
npm run migrate:rollback # Rollback all migrations

# Seed Commands
npm run seed:run         # Run all seed files
```

## 🐳 Docker Development

### Using Docker for Development

1. **Start MySQL with Docker**
   ```bash
   npm run docker
   ```

2. **Connect to MySQL Container**
   ```bash
   docker exec -it <mysql-container-name> mysql -u app -p
   ```

3. **Access phpMyAdmin**
   - URL: http://localhost:81
   - Username: app
   - Password: 123456

## 🔧 Development Workflow

### Code Style and Linting

The project follows Standard.js conventions:

```bash
# Run ESLint
npm run eslint

# Auto-fix linting issues
npx eslint './app/**/*.js' --fix
```

### Testing

```bash
# Run all tests
npm test

# Run tests with coverage
npm run test:coverage

# Run tests in watch mode
npm run test:watch
```

### File Upload Configuration

#### Cloudinary Setup

1. Create a Cloudinary account at https://cloudinary.com/
2. Get your Cloud Name, API Key, and API Secret
3. Update your `.env` file with the credentials

#### AWS S3 Setup

1. Create an AWS account and S3 bucket
2. Create IAM user with S3 permissions
3. Update your `.env` file with the credentials

### Email Configuration

#### Gmail Setup

1. Enable 2-factor authentication on your Gmail account
2. Generate an app password: https://myaccount.google.com/apppasswords
3. Use the app password in your `.env` file

## 📁 Project Architecture

### Directory Structure Explained

```
app/
├── config/          # Application configuration
├── enums/           # Enumeration values
├── helpers/         # Utility helper functions
├── http/
│   ├── controllers/ # Request handlers
│   ├── middlewares/ # Custom middleware
│   └── services/    # Business logic
├── models/          # Database models (Objection.js)
├── routes/          # API route definitions
├── utils/           # Common utilities
└── views/           # Email templates
```

### Adding New Features

1. **Create Model** (if needed)
   ```javascript
   // app/models/NewModel.js
   const { Model } = require('objection')
   
   class NewModel extends Model {
     static get tableName() {
       return 'new_table'
     }
   }
   
   module.exports = NewModel
   ```

2. **Create Migration**
   ```bash
   npx knex migrate:make create_new_table
   ```

3. **Create Service**
   ```javascript
   // app/http/services/newService.js
   const NewModel = require('../../models/NewModel')
   
   const newService = {
     async getAll() {
       return await NewModel.query()
     }
   }
   
   module.exports = newService
   ```

4. **Create Controller**
   ```javascript
   // app/http/controllers/new/getAll.js
   const newService = require('../../services/newService')
   
   module.exports = async (req, res) => {
     try {
       const data = await newService.getAll()
       res.json({ success: true, data })
     } catch (error) {
       res.status(500).json({ success: false, error: error.message })
     }
   }
   ```

5. **Create Routes**
   ```javascript
   // app/routes/new.js
   const express = require('express')
   const router = express.Router()
   const auth = require('../http/middlewares/auth')
   
   router.get('/', auth, require('../http/controllers/new/getAll'))
   
   module.exports = router
   ```

## 🔐 Authentication System

### JWT Token Structure

```javascript
{
  "userId": 123,
  "email": "user@example.com",
  "role": "user",
  "iat": 1234567890,
  "exp": 1234567890
}
```

### Protected Routes

Use the `auth` middleware to protect routes:

```javascript
const auth = require('../http/middlewares/auth')

router.get('/protected', auth, (req, res) => {
  // Access user data via req.user
  res.json({ user: req.user })
})
```

### Role-Based Access Control

Use the `permission` middleware for role-based access:

```javascript
const permission = require('../http/middlewares/permission')

router.delete('/admin-only', auth, permission(['admin']), (req, res) => {
  // Only admins can access this route
})
```

## 🧪 Testing Guidelines

### Test Structure

```javascript
// tests/example.test.js
const { expect } = require('chai')
const sinon = require('sinon')

describe('Feature Name', () => {
  beforeEach(() => {
    // Setup before each test
  })

  afterEach(() => {
    // Cleanup after each test
    sinon.restore()
  })

  it('should do something', () => {
    // Test implementation
    expect(true).to.be.true
  })
})
```

### Running Specific Tests

```bash
# Run specific test file
npx mocha tests/specific-test.js

# Run tests with specific pattern
npx mocha tests/**/*auth*.js
```

## 🚀 Deployment

### Production Environment Variables

```env
NODE_ENV=production
PORT=80
DB_HOST=your-production-db-host
# ... other production variables
```

### PM2 Deployment

```bash
# Install PM2
npm install -g pm2

# Start application
pm2 start app.js --name "api-server"

# Monitor
pm2 monit

# View logs
pm2 logs
```

## 📊 Monitoring and Logging

### Winston Logger Usage

```javascript
const logger = require('./app/helpers/logger')

logger.info('Information message')
logger.error('Error message', { error: errorObject })
logger.warn('Warning message')
```

### Log Levels

- `error`: Error messages
- `warn`: Warning messages
- `info`: General information
- `debug`: Debug information

## 🔍 Debugging

### Debug Mode

```bash
# Enable debug mode
DEBUG=* npm run dev

# Debug specific module
DEBUG=express:* npm run dev
```

### Common Issues

1. **Database Connection Issues**
   - Check MySQL service is running
   - Verify database credentials
   - Ensure database exists

2. **JWT Token Issues**
   - Check JWT_SECRET is set
   - Verify token format
   - Check token expiration

3. **File Upload Issues**
   - Verify Cloudinary/AWS credentials
   - Check file size limits
   - Ensure uploads directory exists

## 🤝 Contributing Guidelines

1. **Code Style**
   - Follow Standard.js conventions
   - Use meaningful variable names
   - Write self-documenting code

2. **Commit Messages**
   - Use conventional commit format
   - Example: `feat: add user authentication`

3. **Pull Request Process**
   - Create feature branch
   - Write tests for new features
   - Update documentation
   - Request code review

## 📚 Additional Resources

- [Express.js Documentation](https://expressjs.com/)
- [Knex.js Documentation](https://knexjs.org/)
- [Objection.js Documentation](https://vincit.github.io/objection.js/)
- [Standard.js Rules](https://standardjs.com/rules.html)
- [JWT.io](https://jwt.io/)

---

Happy coding! 🚀 